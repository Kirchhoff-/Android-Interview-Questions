# Retrofit 
The Retrofit library is a type-safe REST client for Android, Java, and Kotlin, developed by Square. Retrofit by default leverages OkHttp as the networking layer and is built on top of it. Retrofit automatically serialises the JSON response using a POJO(Plain Old Java Object) which must be defined in advanced for the JSON Structure. <sup>[1](https://www.digitalocean.com/community/tutorials/retrofit-android-example-tutorial#:~:text=Retrofit%202%20by%20default%20leverages%20OkHttp%20as%20the%20networking%20layer%20and%20is%20built%20on%20top%20of%20it.%20Retrofit%20automatically%20serialises%20the%20JSON%20response%20using%20a%20POJO(Plain%20Old%20Java%20Object)%20which%20must%20be%20defined%20in%20advanced%20for%20the%20JSON%20Structure.)</sup>

Retrofit turns your HTTP API into a Java interface:
```
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}
```
Annotations on the interface methods and its parameters indicate how a request will be handled.


## Initialization
`Retrofit` is the class through which your API interfaces are turned into callable objects. By default, Retrofit will give you sane defaults for your platform but it allows for customization.

Exmaple of the `Retrofit` class generates an implementation of the GitHubService interface:
```
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();

GitHubService service = retrofit.create(GitHubService.class);
```

Each `Call` from the created `GitHubService` can make a synchronous or asynchronous HTTP request to the remote webserver:
```
Call<List<Repo>> repos = service.listRepos("octocat");
```

## Request method
Every method must have an HTTP annotation that provides the request method and relative URL. There are eight built-in annotations: `HTTP`, `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS` and `HEAD`. The relative URL of the resource is specified in the annotation:
```
@GET("users/list")
```

You can also specify query parameters in the URL:
```
@GET("users/list?sort=desc")
```

## Url Manipulation
A request URL can be updated dynamically using replacement blocks and parameters on the method. A replacement block is an alphanumeric string surrounded by `{` and `}`.  A corresponding parameter must be annotated with `@Path` using the same string:
```
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId);
```
Query parameters can also be added.
```
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @Query("sort") String sort);
```
For complex query parameter combinations a `Map` can be used.
```
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @QueryMap Map<String, String> options);
```

## Request body
An object can be specified for use as an HTTP request body with the `@Body` annotation.
```
@POST("users/new")
Call<User> createUser(@Body User user);
```
The object will also be converted using a converter specified on the `Retrofit` instance. If no converter is added, only `RequestBody` can be used.

## Header manipulation
You can set static headers for a method using the `@Headers` annotation:
```
@Headers("Cache-Control: max-age=640000")
@GET("widget/list")
Call<List<Widget>> widgetList();
```
```
@Headers({
    "Accept: application/vnd.github.v3.full+json",
    "User-Agent: Retrofit-Sample-App"
})
@GET("users/{username}")
Call<User> getUser(@Path("username") String username);
```
Note that headers do not overwrite each other. All headers with the same name will be included in the request.

A request Header can be updated dynamically using the `@Header` annotation. A corresponding parameter must be provided to the `@Header`. If the value is null, the header will be omitted. Otherwise, `toString` will be called on the value, and the result used.
```
@GET("user")
Call<User> getUser(@Header("Authorization") String authorization)
```
Similar to query parameters, for complex header combinations, a `Map` can be used.
```
@GET("user")
Call<User> getUser(@HeaderMap Map<String, String> headers)
```

## Converters
By default, Retrofit can only deserialize HTTP bodies into OkHttp's `ResponseBody` type and it can only accept its `RequestBody` type for `@Body`.

Converters can be added to support other types. Six sibling modules adapt popular serialization libraries for your convenience:
- [Gson](https://github.com/google/gson): `com.squareup.retrofit2:converter-gson`
- [Jackson](https://github.com/FasterXML/jackson): `com.squareup.retrofit2:converter-jackson`
- [Moshi](https://github.com/square/moshi/): `com.squareup.retrofit2:converter-moshi`
- [Protobuf](https://protobuf.dev/): `com.squareup.retrofit2:converter-protobuf`
- [Wire](https://github.com/square/wire): `com.squareup.retrofit2:converter-wire`
- [Simple XML](https://simple.sourceforge.net/): `com.squareup.retrofit2:converter-simplexml`
- [JAXB](https://docs.oracle.com/javase/tutorial/jaxb/intro/index.html): `com.squareup.retrofit2:converter-jaxb`
- Scalars (primitives, boxed, and String): `com.squareup.retrofit2:converter-scalars`

Here's an example of using the `GsonConverterFactory` class to generate an implementation of the `GitHubService` interface which uses Gson for its deserialization:
```
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build();

GitHubService service = retrofit.create(GitHubService.class);
```

If you need to communicate with an API that uses a content-format that Retrofit does not support out of the box (e.g. YAML, txt, custom format) or you wish to use a different library to implement an existing format, you can easily create your own converter. Create a class that extends the `Converter.Factory` class and pass in an instance when building your adapter.

# Links
[Retrofit](https://github.com/square/retrofit)

[Retrofit](https://square.github.io/retrofit/)

[Retrofit Android Example Tutorial](https://www.digitalocean.com/community/tutorials/retrofit-android-example-tutorial)

# Next questions
[What do you know about OkHttp?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Libraries/What%20do%20you%20know%20about%20OkHttp.md)
