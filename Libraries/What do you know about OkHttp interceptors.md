# OkHttp interceptors
Interceptors are pluggable Java components that we can use to intercept and process requests before they are sent to our application code.

Likewise, they provide a powerful mechanism for us to process the server response before the container sends the response back to the client.

**This is particularly useful when we want to change something in an HTTP request, like adding a new control header, changing the body of our request, or simply producing logs to help us debug.**

Another nice feature of using interceptors is that they let us encapsulate common functionality in one place. Let's imagine we want to apply some logic globally to all our request and response objects, such as error handling.

There are at least a couple of advantages of putting this kind of logic into an interceptor:
- We only need to maintain this code in one place rather than all of our endpoints;
- Every request made deals with the error in the same way.

Finally, we can also monitor, rewrite, and retry calls from our interceptors as well.<sup>[1](https://www.baeldung.com/java-okhttp-interceptors#:~:text=interceptors%20are%20pluggable,interceptors%20as%20well.)</sup>

Here’s a simple interceptor that logs the outgoing request and the incoming response:
```
class LoggingInterceptor implements Interceptor {
  @Override public Response intercept(Interceptor.Chain chain) throws IOException {
    Request request = chain.request();

    long t1 = System.nanoTime();
    logger.info(String.format("Sending request %s on %s%n%s",
        request.url(), chain.connection(), request.headers()));

    Response response = chain.proceed(request);

    long t2 = System.nanoTime();
    logger.info(String.format("Received response for %s in %.1fms%n%s",
        response.request().url(), (t2 - t1) / 1e6d, response.headers()));

    return response;
  }
}
```

A call to `chain.proceed(request)` is a critical part of each interceptor’s implementation. This simple-looking method is where all the HTTP work happens, producing a response to satisfy the request. If `chain.proceed(request)` is being called more than once previous response bodies must be closed. <sup>[2](
https://square.github.io/okhttp/features/interceptors/#:~:text=Here%E2%80%99s%20a%20simple,must%20be%20closed.)</sup>

## Interceptor Types
Interceptors are classified into two types:
- Interceptors added between the Application Code (our written code) and the OkHttp Core Library are referred to as application interceptors. These are the interceptors that we add with `addInterceptor()`;
- Interceptors on the network: These are interceptors placed between the OkHttp Core Library and the server. These can be added to OkHttpClient by using the `addNetworkInterceptor()`.<sup>[3](https://www.geeksforgeeks.org/okhttp-interceptor-in-android/#:~:text=Interceptor%20Types,the%20addNetworkInterceptor%20command%20().)</sup>

![](./res/interceptors.png "Interceptors")

### [Application Interceptors](https://square.github.io/okhttp/features/interceptors/#:~:text=called%20in%20order.-,Application%20Interceptors,-%C2%B6)
Register an *application* interceptor by calling `addInterceptor()` on `OkHttpClient.Builder`:
```
OkHttpClient client = new OkHttpClient.Builder()
    .addInterceptor(new LoggingInterceptor())
    .build();

Request request = new Request.Builder()
    .url("http://www.publicobject.com/helloworld.txt")
    .header("User-Agent", "OkHttp Example")
    .build();

Response response = client.newCall(request).execute();
response.body().close();
```

The URL `http://www.publicobject.com/helloworld.txt` redirects to `https://publicobject.com/helloworld.txt`, and OkHttp follows this redirect automatically. Our application interceptor is called **once** and the response returned from `chain.proceed()` has the redirected response:
```
INFO: Sending request http://www.publicobject.com/helloworld.txt on null
User-Agent: OkHttp Example

INFO: Received response for https://publicobject.com/helloworld.txt in 1179.7ms
Server: nginx/1.4.6 (Ubuntu)
Content-Type: text/plain
Content-Length: 1759
Connection: keep-alive
```

We can see that we were redirected because `response.request().url()` is different from `request.url()`. The two log statements log two different URLs.

### [Network Interceptors](https://square.github.io/okhttp/features/interceptors/#:~:text=two%20different%20URLs.-,Network%20Interceptors,-%C2%B6)
Registering a network interceptor is quite similar. Call `addNetworkInterceptor()` instead of `addInterceptor()`:
```
OkHttpClient client = new OkHttpClient.Builder()
    .addNetworkInterceptor(new LoggingInterceptor())
    .build();

Request request = new Request.Builder()
    .url("http://www.publicobject.com/helloworld.txt")
    .header("User-Agent", "OkHttp Example")
    .build();

Response response = client.newCall(request).execute();
response.body().close();
```

When we run this code, the interceptor runs twice. Once for the initial request to `http://www.publicobject.com/helloworld.txt`, and another for the redirect to `https://publicobject.com/helloworld.txt`:
```
NFO: Sending request http://www.publicobject.com/helloworld.txt on Connection{www.publicobject.com:80, proxy=DIRECT hostAddress=54.187.32.157 cipherSuite=none protocol=http/1.1}
User-Agent: OkHttp Example
Host: www.publicobject.com
Connection: Keep-Alive
Accept-Encoding: gzip

INFO: Received response for http://www.publicobject.com/helloworld.txt in 115.6ms
Server: nginx/1.4.6 (Ubuntu)
Content-Type: text/html
Content-Length: 193
Connection: keep-alive
Location: https://publicobject.com/helloworld.txt

INFO: Sending request https://publicobject.com/helloworld.txt on Connection{publicobject.com:443, proxy=DIRECT hostAddress=54.187.32.157 cipherSuite=TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA protocol=http/1.1}
User-Agent: OkHttp Example
Host: publicobject.com
Connection: Keep-Alive
Accept-Encoding: gzip

INFO: Received response for https://publicobject.com/helloworld.txt in 80.9ms
Server: nginx/1.4.6 (Ubuntu)
Content-Type: text/plain
Content-Length: 1759
Connection: keep-alive
```

### [Rewriting Requests](https://square.github.io/okhttp/features/interceptors/#:~:text=carries%20the%20request.-,Rewriting%20Requests,-%C2%B6)
Interceptors can add, remove, or replace request headers. They can also transform the body of those requests that have one. For example, you can use an application interceptor to add request body compression if you’re connecting to a webserver known to support it.
```
/** This interceptor compresses the HTTP request body. Many webservers can't handle this! */
final class GzipRequestInterceptor implements Interceptor {
  @Override public Response intercept(Interceptor.Chain chain) throws IOException {
    Request originalRequest = chain.request();
    if (originalRequest.body() == null || originalRequest.header("Content-Encoding") != null) {
      return chain.proceed(originalRequest);
    }

    Request compressedRequest = originalRequest.newBuilder()
        .header("Content-Encoding", "gzip")
        .method(originalRequest.method(), gzip(originalRequest.body()))
        .build();
    return chain.proceed(compressedRequest);
  }

  private RequestBody gzip(final RequestBody body) {
    return new RequestBody() {
      @Override public MediaType contentType() {
        return body.contentType();
      }

      @Override public long contentLength() {
        return -1; // We don't know the compressed length in advance!
      }

      @Override public void writeTo(BufferedSink sink) throws IOException {
        BufferedSink gzipSink = Okio.buffer(new GzipSink(sink));
        body.writeTo(gzipSink);
        gzipSink.close();
      }
    };
  }
}
```

### [Rewriting Responses](https://square.github.io/okhttp/features/interceptors/#:~:text=%7D%3B%0A%20%20%7D%0A%7D-,Rewriting%20Responses,-%C2%B6)
Symmetrically, interceptors can rewrite response headers and transform the response body. This is generally more dangerous than rewriting request headers because it may violate the webserver’s expectations!

If you’re in a tricky situation and prepared to deal with the consequences, rewriting response headers is a powerful way to work around problems. For example, you can fix a server’s misconfigured `Cache-Control` response header to enable better response caching:
```
/** Dangerous interceptor that rewrites the server's cache-control header. */
private static final Interceptor REWRITE_CACHE_CONTROL_INTERCEPTOR = new Interceptor() {
  @Override public Response intercept(Interceptor.Chain chain) throws IOException {
    Response originalResponse = chain.proceed(chain.request());
    return originalResponse.newBuilder()
        .header("Cache-Control", "max-age=60")
        .build();
  }
};
```

### [Choosing between application and network interceptors](https://square.github.io/okhttp/features/interceptors/#:~:text=to%20the%20webserver.-,Choosing%20between%20application%20and%20network%20interceptors,-%C2%B6)
Each interceptor chain has relative merits.

**Application interceptors:**
- Don’t need to worry about intermediate responses like redirects and retries;
- Are always invoked once, even if the HTTP response is served from the cache;
- Observe the application’s original intent. Unconcerned with OkHttp-injected headers like `If-None-Match`;
- Permitted to short-circuit and not call `Chain.proceed()`;
- Permitted to retry and make multiple calls to `Chain.proceed()`;
- Can adjust Call timeouts using `withConnectTimeout`, `withReadTimeout`, `withWriteTimeout`.

**Network Interceptors:**
- Able to operate on intermediate responses like redirects and retries;
- Not invoked for cached responses that short-circuit the network;
- Observe the data just as it will be transmitted over the network;
- Access to the `Connection` that carries the request.

# Links
[Interceptors](https://square.github.io/okhttp/features/interceptors/)

[Adding Interceptors in OkHTTP](https://www.baeldung.com/java-okhttp-interceptors)

[OkHttp Interceptor in Android](https://www.geeksforgeeks.org/okhttp-interceptor-in-android/)
