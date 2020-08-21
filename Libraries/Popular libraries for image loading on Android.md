# Image loading libraries

[Glide](https://github.com/bumptech/glide) is a fast and efficient open source media management and image loading framework for Android that wraps media decoding, memory and disk caching, and resource pooling into a simple and easy to use interface. 

Glide supports fetching, decoding, and displaying video stills, images, and animated GIFs. Glide includes a flexible API that allows developers to plug in to almost any network stack. By default Glide uses a custom `HttpUrlConnection` based stack, but also includes utility libraries plug in to Google's Volley project or Square's OkHttp library instead.

Glide's primary focus is on making scrolling any kind of a list of images as smooth and fast as possible, but Glide is also effective for almost any case where you need to fetch, resize, and display a remote image.

[Picasso](https://github.com/square/picasso) allows for hassle-free image loading in your application—often in one line of code! Many common pitfalls of image loading on Android are handled automatically by Picasso:
- Handling ImageView recycling and download cancelation in an adapter.
- Complex image transformations with minimal memory use.
- Automatic memory and disk caching.

[Fresco](https://github.com/facebook/fresco) is a powerful system for displaying images in Android applications.

Fresco takes care of image loading and display, so you don't have to. It will load images from the network, local storage, or local resources, and display a placeholder until the image has arrived. It has two levels of cache; one in memory and another in internal storage.

In Android 4.x and lower, Fresco puts images in a special region of Android memory. This lets your application run faster - and suffer the dreaded `OutOfMemoryError` much less often.

Fresco also supports:
- streaming of progressive JPEGs
- display of animated GIFs and WebPs
- extensive customization of image loading and display

[Coil](https://github.com/coil-kt/coil) image loading library for Android backed by Kotlin Coroutines. Coil is:
- **Fast**: Coil performs a number of optimizations including memory and disk caching, downsampling the image in memory, re-using Bitmaps, automatically pausing/cancelling requests, and more.
- **Lightweight**: Coil adds ~2000 methods to your APK (for apps that already use OkHttp and Coroutines), which is comparable to Picasso and significantly less than Glide and Fresco.
- **Easy to use**: Coil's API leverages Kotlin's language features for simplicity and minimal boilerplate.
- **Modern**: Coil is Kotlin-first and uses modern libraries including Coroutines, OkHttp, Okio, and AndroidX Lifecycles.

## Links
https://proandroiddev.com/coil-vs-picasso-vs-glide-get-ready-go-774add8cfd40  
https://github.com/bumptech/glide  
https://github.com/square/picasso  
https://github.com/facebook/fresco  
https://github.com/coil-kt/coil  
https://square.github.io/picasso/
