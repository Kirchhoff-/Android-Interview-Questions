# [OkHttp](https://square.github.io/okhttp/#okhttp)
OkHttp is an HTTP client that’s efficient by default:
- HTTP/2 support allows all requests to the same host to share a socket;
- Connection pooling reduces request latency (if HTTP/2 isn’t available);
- Transparent GZIP shrinks download sizes;
- Response caching avoids the network completely for repeat requests.

OkHttp perseveres when the network is troublesome: it will silently recover from common connection problems. If your service has multiple IP addresses, OkHttp will attempt alternate addresses if the first connect fails. This is necessary for IPv4+IPv6 and services hosted in redundant data centers. OkHttp supports modern TLS features (TLS 1.3, ALPN, certificate pinning). It can be configured to fall back for broad connectivity.

Using OkHttp is easy. Its request/response API is designed with fluent builders and immutability. It supports both synchronous blocking calls and async calls with callbacks.

## [Requirements](https://square.github.io/okhttp/#requirements)
OkHttp works on Android 5.0+ (API level 21+) and Java 8+.

OkHttp depends on [Okio](https://github.com/square/okio) for high-performance I/O and the [Kotlin standard library](https://kotlinlang.org/). Both are small libraries with strong backward-compatibility.

The OkHttp 3.12.x branch supports Android 2.3+ (API level 9+) and Java 7+. These platforms lack support for TLS 1.2 and should not be used. 

## [Recipes](https://square.github.io/okhttp/recipes/)

### [Synchronous Get](https://square.github.io/okhttp/recipes/#synchronous-get-kt-java)

Download a file, print its headers, and print its response body as a string.

The `string()` method on response body is convenient and efficient for small documents. But if the response body is large (greater than 1 MiB), avoid `string()` because it will load the entire document into memory. In that case, prefer to process the body as a stream.
```
private val client = OkHttpClient()

  fun run() {
    val request = Request.Builder()
        .url("https://publicobject.com/helloworld.txt")
        .build()

    client.newCall(request).execute().use { response ->
      if (!response.isSuccessful) throw IOException("Unexpected code $response")

      for ((name, value) in response.headers) {
        println("$name: $value")
      }

      println(response.body!!.string())
    }
  }
```

### [Asynchronous Get](https://square.github.io/okhttp/recipes/#asynchronous-get-kt-java)
Download a file on a worker thread, and get called back when the response is readable. The callback is made after the response headers are ready. Reading the response body may still block. OkHttp doesn’t currently offer asynchronous APIs to receive a response body in parts.
```
private val client = OkHttpClient()

  fun run() {
    val request = Request.Builder()
        .url("http://publicobject.com/helloworld.txt")
        .build()

    client.newCall(request).enqueue(object : Callback {
      override fun onFailure(call: Call, e: IOException) {
        e.printStackTrace()
      }

      override fun onResponse(call: Call, response: Response) {
        response.use {
          if (!response.isSuccessful) throw IOException("Unexpected code $response")

          for ((name, value) in response.headers) {
            println("$name: $value")
          }

          println(response.body!!.string())
        }
      }
    })
  }
```

### [Posting a String](https://square.github.io/okhttp/recipes/#posting-a-string-kt-java)
Use an HTTP POST to send a request body to a service. This example posts a markdown document to a web service that renders markdown as HTML. Because the entire request body is in memory simultaneously, avoid posting large (greater than 1 MiB) documents using this API.
```
  private val client = OkHttpClient()

  fun run() {
    val postBody = """
        |Releases
        |--------
        |
        | * _1.0_ May 6, 2013
        | * _1.1_ June 15, 2013
        | * _1.2_ August 11, 2013
        |""".trimMargin()

    val request = Request.Builder()
        .url("https://api.github.com/markdown/raw")
        .post(postBody.toRequestBody(MEDIA_TYPE_MARKDOWN))
        .build()

    client.newCall(request).execute().use { response ->
      if (!response.isSuccessful) throw IOException("Unexpected code $response")

      println(response.body!!.string())
    }
  }

  companion object {
    val MEDIA_TYPE_MARKDOWN = "text/x-markdown; charset=utf-8".toMediaType()
  }
```

### [Posting a File](https://square.github.io/okhttp/recipes/#posting-a-file-kt-java)
It’s easy to use a file as a request body.
```
  private val client = OkHttpClient()

  fun run() {
    val file = File("README.md")

    val request = Request.Builder()
        .url("https://api.github.com/markdown/raw")
        .post(file.asRequestBody(MEDIA_TYPE_MARKDOWN))
        .build()

    client.newCall(request).execute().use { response ->
      if (!response.isSuccessful) throw IOException("Unexpected code $response")

      println(response.body!!.string())
    }
  }

  companion object {
    val MEDIA_TYPE_MARKDOWN = "text/x-markdown; charset=utf-8".toMediaType()
  }
```

### [Posting a multipart request](https://square.github.io/okhttp/recipes/#posting-a-multipart-request-kt-java)
`MultipartBody.Builder` can build sophisticated request bodies compatible with HTML file upload forms.  Each part of a multipart request body is itself a request body, and can define its own headers. If present, these headers should describe the part body, such as its `Content-Disposition`. The `Content-Length` and `Content-Type` headers are added automatically if they’re available.
```
  private val client = OkHttpClient()

  fun run() {
    // Use the imgur image upload API as documented at https://api.imgur.com/endpoints/image
    val requestBody = MultipartBody.Builder()
        .setType(MultipartBody.FORM)
        .addFormDataPart("title", "Square Logo")
        .addFormDataPart("image", "logo-square.png",
            File("docs/images/logo-square.png").asRequestBody(MEDIA_TYPE_PNG))
        .build()

    val request = Request.Builder()
        .header("Authorization", "Client-ID $IMGUR_CLIENT_ID")
        .url("https://api.imgur.com/3/image")
        .post(requestBody)
        .build()

    client.newCall(request).execute().use { response ->
      if (!response.isSuccessful) throw IOException("Unexpected code $response")

      println(response.body!!.string())
    }
  }

  companion object {
    /**
     * The imgur client ID for OkHttp recipes. If you're using imgur for anything other than running
     * these examples, please request your own client ID! https://api.imgur.com/oauth2
     */
    private val IMGUR_CLIENT_ID = "9199fdef135c122"
    private val MEDIA_TYPE_PNG = "image/png".toMediaType()
  }
```

### [Timeouts](https://square.github.io/okhttp/recipes/#timeouts-kt-java)
Use timeouts to fail a call when its peer is unreachable. Network partitions can be due to client connectivity problems, server availability problems, or anything between. OkHttp supports connect, write, read, and full call timeouts.
```
private val client: OkHttpClient = OkHttpClient.Builder()
      .connectTimeout(5, TimeUnit.SECONDS)
      .writeTimeout(5, TimeUnit.SECONDS)
      .readTimeout(5, TimeUnit.SECONDS)
      .callTimeout(10, TimeUnit.SECONDS)
      .build()

  fun run() {
    val request = Request.Builder()
        .url("http://httpbin.org/delay/2") // This URL is served with a 2 second delay.
        .build()

    client.newCall(request).execute().use { response ->
      println("Response completed: $response")
    }
  }
```

# Links
[OkHttp](https://square.github.io/okhttp/)

[OkHttp Github page](https://github.com/square/okhttp)
