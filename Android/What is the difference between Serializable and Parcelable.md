# Serializable vs Parcelable

## Serializable 
Serializable is a standard Java interface. For the serializable class just implement the Serializable interface and add override methods. This approach is a slow process. Its create a lot of temporary objects and a bit of garbage collection but the serializable interface is easier to implement. Java will automatically serialize it in certain situations.

## Parcelable
Parcelable is another interface. Despite its rival (Serializable in case you forgot), it is a part of the Android SDK. Now, Parcelable was specifically designed in such a way that there is no reflection when using it. That is because we are being really explicit for the serialization process.

## Conclusion
- **Parcelable** is faster than **Serializable** interface
- **Parcelable** interface takes more time to implement compared to **Serializable** interface (with Kotlin this is not true)
- **Serializable** interface is easier to implement
- **Serializable** interface creates a lot of temporary objects and causes quite a bit of garbage collection

## Links
https://docs.oracle.com/javase/7/docs/api/java/io/Serializable.html
https://developer.android.com/reference/android/os/Parcelable.html  
https://android.jlelse.eu/parcelable-vs-serializable-6a2556d51538
