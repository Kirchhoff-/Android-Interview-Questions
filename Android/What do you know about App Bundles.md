# App Bundles
An *Android App Bundle* is a publishing format that includes all your app’s compiled code and resources, and defers APK generation and signing to Google Play.

Google Play uses your app bundle to generate and serve optimized APKs for each device configuration, so only the code and resources that are needed for a specific device are downloaded to run your app. You no longer have to build, sign, and manage multiple APKs to optimize support for different devices, and users get smaller, more-optimized downloads.

When you use the app bundle format to publish your app, you can also optionally take advantage of [Play Feature Delivery](https://developer.android.com/guide/playcore/feature-delivery), which allows you to add *feature modules* to your app project. hese modules contain features and resources that are only included with your app based on conditions that you specify, or are available later at runtime for download [Using the Play Core Library](https://developer.android.com/guide/playcore).

## [What's the difference between AABs and APKs?](https://developer.android.com/guide/app-bundle/faq#whats_the_difference_between_aabs_and_apks)
App bundles are only for publishing and cannot be installed on Android devices. The Android package (APK) is Android's installable, executable format for apps. App bundles must be processed by a distributor into APKs so that they can be installed on devices.

| APK | AAB |
|---|---|
| Upload to Google Play and install directly on devices	| Upload to Google Play only |
| No reduction in download size via dynamic delivery		| Supports dynamic delivery |
| Supports developer or Google signing	| Google signing only |

## [Compressed download size restriction](https://developer.android.com/guide/app-bundle#size_restrictions)
Publishing with Android App Bundles helps your users to install your app with the smallest downloads possible and increases the **compressed download size limit to 150 MB**. hat is, when a user downloads your app, the total size of the compressed APKs required to install your app (for example, the base APK + configuration APKs) must be no more than 150 MB. Any subsequent downloads, such as downloading a feature module (and its configuration APKs) on demand, must also meet this compressed download size restriction. Asset packs do not contribute to this size limit, but they do have other [size restrictions](https://developer.android.com/guide/playcore/asset-delivery#size-limits).

# Links  
[About Android App Bundles](https://developer.android.com/guide/app-bundle)

[Android App Bundle frequently asked questions](https://developer.android.com/guide/app-bundle/faq)

[Android App Bundle (AAB)](https://gonative.io/docs/android-app-bundle)

# Further Reading
[Top 7 takeaways for Android App Bundles](https://www.youtube.com/watch?v=st9VZuJNIbw)

[What a new publishing format means for the future of Android](https://medium.com/googleplaydev/what-a-new-publishing-format-means-for-the-future-of-android-2e34981793a)

[Building your first app bundle](https://medium.com/androiddevelopers/building-your-first-app-bundle-bbcd228bf631)
