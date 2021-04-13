# DataStore
Jetpack DataStore is a data storage solution that allows you to store key-value pairs or typed objects with protocol buffers. DataStore uses Kotlin coroutines and Flow to store data asynchronously, consistently, and transactionally.

## [Preferences DataStore and Proto DataStore](https://developer.android.com/topic/libraries/architecture/datastore#prefs-vs-proto)
DataStore provides two different implementations: Preferences DataStore and Proto DataStore.
- **Preferences DataStore** stores and accesses data using keys. This implementation does not require a predefined schema, and it does not provide type safety.
- **Proto DataStore** stores data as instances of a custom data type. This implementation requires you to define a schema using protocol buffers, but it provides type safety.

## [Store key-value pairs with Preferences DataStore](https://developer.android.com/topic/libraries/architecture/datastore#preferences-datastore)
The Preferences DataStore implementation uses the `DataStore` and `Preferences` classes to persist simple key-value pairs to disk.

### [Create a Preferences DataStore](https://developer.android.com/topic/libraries/architecture/datastore#preferences-create)
Use the property delegate created by `preferencesDataStore` to create an instance of `Datastore<Preferences>`. Call it once at the top level of your kotlin file, and access it through this property throughout the rest of your application. This makes it easier to keep your `DataStore` as a singleton. The mandatory `name` parameter is the name of the Preferences DataStore.

```
// At the top level of your kotlin file:
val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "settings")
```

### [Read from a Preferences DataStore](https://developer.android.com/topic/libraries/architecture/datastore#preferences-read)
Because Preferences DataStore does not use a predefined schema, you must use the corresponding key type function to define a key for each value that you need to store in the `DataStore<Preferences>` instance. For example, to define a key for an int value, use `intPreferencesKey()`. Then, use the `DataStore.data` property to expose the appropriate stored value using a `Flow`.

```
val EXAMPLE_COUNTER = intPreferencesKey("example_counter")
val exampleCounterFlow: Flow<Int> = context.dataStore.data
  .map { preferences ->
    // No type safety.
    preferences[EXAMPLE_COUNTER] ?: 0
}
```

### [Write to a Preferences DataStore](https://developer.android.com/topic/libraries/architecture/datastore#preferences-write)
Preferences DataStore provides an `edit()` function that transactionally updates the data in a `DataStore`. The function's `transform` parameter accepts a block of code where you can update the values as needed. All of the code in the transform block is treated as a single transaction.

```
suspend fun incrementCounter() {
  context.dataStore.edit { settings ->
    val currentCounterValue = settings[EXAMPLE_COUNTER] ?: 0
    settings[EXAMPLE_COUNTER] = currentCounterValue + 1
  }
}
```

## [Store typed objects with Proto DataStore](https://developer.android.com/topic/libraries/architecture/datastore#proto-datastore)
The Proto DataStore implementation uses DataStore and protocol buffers to persist typed objects to disk.

### [Define a schema](https://developer.android.com/topic/libraries/architecture/datastore#proto-schema)
Proto DataStore requires a predefined schema in a proto file in the `app/src/main/proto/` directory. This schema defines the type for the objects that you persist in your Proto DataStore.

```
syntax = "proto3";

option java_package = "com.example.application";
option java_multiple_files = true;

message Settings {
  int32 example_counter = 1;
}
```

### [Create a Proto DataStore](https://developer.android.com/topic/libraries/architecture/datastore#proto-create)
There are two steps involved in creating a Proto DataStore to store your typed objects:

1. Define a class that implements `Serializer<T>`, where `T` is the type defined in the proto file. This serializer class tells DataStore how to read and write your data type. Make sure you include a default value for the serializer to be used if there is no file created yet.

2. Use the property delegate created by `dataStore` to create an instance of `DataStore<T>`, where `T` is the type defined in the proto file. Call this once at the top level of your kotlin file and access it through this property delegate throughout the rest of your app. The `filename` parameter tells DataStore which file to use to store the data, and the `serializer` parameter tells DataStore the name of the serializer class defined in step 1.

```
object SettingsSerializer : Serializer<Settings> {
  override val defaultValue: Settings = Settings.getDefaultInstance()

  override suspend fun readFrom(input: InputStream): Settings {
    try {
      return Settings.parseFrom(input)
    } catch (exception: InvalidProtocolBufferException) {
      throw CorruptionException("Cannot read proto.", exception)
    }
  }

  override suspend fun writeTo(
    t: Settings,
    output: OutputStream) = t.writeTo(output)
}

val Context.settingsDataStore: DataStore<Settings> by dataStore(
  fileName = "settings.pb",
  serializer = SettingsSerializer
)
```

### [Read from a Proto DataStore](https://developer.android.com/topic/libraries/architecture/datastore#proto-read)
Use `DataStore.data` to expose a `Flow` of the appropriate property from your stored object.

```
val exampleCounterFlow: Flow<Int> = context.settingsDataStore.data
  .map { settings ->
    // The exampleCounter property is generated from the proto schema.
    settings.exampleCounter
  }
```

### [Write to a Proto DataStore](https://developer.android.com/topic/libraries/architecture/datastore#proto-write)
Proto DataStore provides an `updateData()` function that transactionally updates a stored object. `updateData()` gives you the current state of the data as an instance of your data type and updates the data transactionally in an atomic read-write-modify operation.

```
suspend fun incrementCounter() {
  context.settingsDataStore.updateData { currentSettings ->
    currentSettings.toBuilder()
      .setExampleCounter(currentSettings.exampleCounter + 1)
      .build()
    }
}
```

## [Use DataStore in synchronous code](https://developer.android.com/topic/libraries/architecture/datastore#synchronous)
One of the primary benefits of DataStore is the asynchronous API, but it may not always be feasible to change your surrounding code to be asynchronous. This might be the case if you're working with an existing codebase that uses synchronous disk I/O or if you have a dependency that doesn't provide an asynchronous API.

Kotlin coroutines provide the `runBlocking()` coroutine builder to help bridge the gap between synchronous and asynchronous code. You can use `runBlocking()` to read data from DataStore synchronously. The following code blocks the calling thread until DataStore returns data:

```
val exampleData = runBlocking { context.dataStore.data.first() }
```

Performing synchronous I/O operations on the UI thread can cause ANRs or UI jank. You can mitigate these issues by asynchronously preloading the data from DataStore:
```
override fun onCreate(savedInstanceState: Bundle?) {
    lifecycleScope.launch {
        context.dataStore.data.first()
        // You should also handle IOExceptions here.
    }
}
```
This way, DataStore asynchronously reads the data and caches it in memory. Later synchronous reads using `runBlocking()` may be faster or may avoid a disk I/O operation altogether if the initial read has completed.

# Links
[DataStore](https://developer.android.com/topic/libraries/architecture/datastore)

# Futher reading
[Prefer Storing Data with Jetpack DataStore](https://android-developers.googleblog.com/2020/09/prefer-storing-data-with-jetpack.html)  
[Working with Preferences DataStore](https://developer.android.com/codelabs/android-preferences-datastore#0)  
[Working with Proto DataStore](https://developer.android.com/codelabs/android-proto-datastore#0)
