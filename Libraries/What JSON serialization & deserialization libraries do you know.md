# JSON Serialization & Deserialization libraries

There are four main libraries for working with JSON in Android:
- [Gson](#Gson)
- [Moshi](#Moshi)
- [Jackson](#Jackson)
- [Kotlinx Serialization](#Kotlinx-Serialization)

## [Gson](https://github.com/google/gson)
Gson is a Java library that can be used to convert Java Objects into their JSON representation. It can also be used to convert a JSON string to an equivalent Java object. Gson can work with arbitrary Java objects including pre-existing objects that you do not have source-code of.

**Important**. Gson's main focus is on Java. Using it with other JVM languages such as Kotlin or Scala might work fine in many cases, but language-specific features such as Kotlin's non-`null` types or constructors with default arguments are not supported. This can lead to confusing and incorrect behavior.
When using languages other than Java, prefer a JSON library with explicit support for that language.

Key Features:
- Provide simple `toJson()` and `fromJson()` methods to convert Java objects to JSON and vice-versa;
- Allow pre-existing unmodifiable objects to be converted to and from JSON;
- Extensive support of Java Generics;
- Allow custom representations for objects;
- Support arbitrarily complex objects (with deep inheritance hierarchies and extensive use of generic types).

Example of usage:
```
Gson gson = new Gson();
String json = gson.toJson(myObject);  // Serialization
MyClass obj = gson.fromJson(json, MyClass.class);  // Deserialization
```

## [Moshi](https://github.com/square/moshi)
Moshi is a modern JSON library for Android, Java and Kotlin. It makes it easy to parse JSON into Java and Kotlin classes:
```
val json: String = ...

val moshi: Moshi = Moshi.Builder().build()
val jsonAdapter: JsonAdapter<BlackjackHand> = moshi.adapter<BlackjackHand>()

val blackjackHand = jsonAdapter.fromJson(json)
println(blackjackHand)
```

And it can just as easily serialize Java or Kotlin objects as JSON:
```
val blackjackHand = BlackjackHand(
    Card('6', SPADES),
    listOf(Card('4', CLUBS), Card('A', HEARTS))
  )

val moshi: Moshi = Moshi.Builder().build()
val jsonAdapter: JsonAdapter<BlackjackHand> = moshi.adapter<BlackjackHand>()

val json: String = jsonAdapter.toJson(blackjackHand)
println(json)
```

Moshi uses the same streaming and binding mechanisms as Gson. But the two libraries have a few important differences:
- **Moshi has fewer built-in type adapters**. For example, you need to configure your own date adapter. Most binding libraries will encode whatever you throw at them. Moshi refuses to serialize platform types (`java.*`, `javax.*`, and `android.*`) without a user-provided type adapter. This is intended to prevent you from accidentally locking yourself to a specific JDK or Android release;
- **Moshi is less configurable**. There’s no field naming strategy, versioning, instance creators, or long serialization policy. Instead of naming a field `visibleCards` and using a policy class to convert that to `visible_cards`, Moshi wants you to just name the field `visible_cards` as it appears in the JSON;
- **Moshi doesn’t have a JsonElement model**. Instead it just uses built-in types like `List` and `Map`;
- **No HTML-safe escaping**. Gson encodes `=` as `\u003d` by default so that it can be safely encoded in HTML without additional escaping. Moshi encodes it naturally (as `=`) and assumes that the HTML encoder – if there is one – will do its job.

## [Jackson](https://github.com/FasterXML/jackson)
Jackson has been known as "the Java JSON library" or "the best JSON parser for Java". Or simply as "JSON for Java". More than that, Jackson is a suite of data-processing tools for Java (and the JVM platform), including the flagship streaming JSON parser / generator library, matching data-binding library (POJOs to and from JSON).

Example of usage:
```
ObjectMapper objectMapper = new ObjectMapper();
String json = objectMapper.writeValueAsString(myObject);  // Serialization
MyClass obj = objectMapper.readValue(json, MyClass.class);  // Deserialization
```

## [Kotlinx Serialization](https://github.com/Kotlin/kotlinx.serialization)
Kotlin Serialization is a cross-platform and multi-format framework for data serialization—converting trees of objects to strings, byte arrays, or other *serial* representations and back. Kotlin Serialization fully supports and enforces the Kotlin type system, making sure only valid objects can be deserialized. Kotlin Serialization is not just a library. It is a compiler plugin that is bundled with the Kotlin compiler distribution itself.

Key Features:
- Complete multiplatform support: JVM, JS and Native;
- Provides JSON, Protobuf, CBOR, Hocon and Properties formats;
- Supports Kotlin classes marked as @Serializable and standard collections.

Example of usage:
```
import kotlinx.serialization.*
import kotlinx.serialization.json.*

@Serializable 
data class Project(val name: String, val language: String)

fun main() {
    // Serializing objects
    val data = Project("kotlinx.serialization", "Kotlin")
    val string = Json.encodeToString(data)  
    println(string) // {"name":"kotlinx.serialization","language":"Kotlin"} 
    // Deserializing back into objects
    val obj = Json.decodeFromString<Project>(string)
    println(obj) // Project(name=kotlinx.serialization, language=Kotlin)
}
```

# Links
[Gson](https://github.com/google/gson)

[Moshi](https://github.com/square/moshi)

[Jackson](https://github.com/FasterXML/jackson)

[Kotlinx Serialization](https://github.com/Kotlin/kotlinx.serialization)
