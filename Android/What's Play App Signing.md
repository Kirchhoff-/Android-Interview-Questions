# Play App Signing
With Play App Signing, Google manages and protects your app's signing key for you and uses it to sign optimized, distribution APKs that are generated from your app bundles. Play App Signing stores your app signing key on Google’s secure infrastructure and offers upgrade options to increase security.

Play App Signing uses two keys: the app *signing key* and the *upload key*. You keep the upload key and use it to sign your app for upload to the Google Play Store. Google uses the upload certificate to verify your identity, and signs your APK(s) with your app signing key for distribution as shown in figure below. By using a separate upload key you can [request an upload key reset](https://support.google.com/googleplay/android-developer/answer/9842756) if your key is ever lost or compromised.

![](./res/app_signing.png "App Signing")

## [Keystores, keys, and certificates](https://developer.android.com/studio/publish/app-signing#certificates-keystores)
Java Keystores (.jks or .keystore) are binary files that serve as repositories of certificates and private keys.

A **public key certificate** (`.der` or `.pem` files), also known as a digital certificate or an identity certificate, contains the public key of a public/private key pair, as well as some other metadata identifying the owner (for example, name and location) who holds the corresponding private key.

The following are the different types of keys you should understand:
- **App signing key**: The key that is used to sign APKs that are installed on a user's device. As part of Android’s secure update model, the signing key never changes during the lifetime of your app. The app signing key is private and must be kept secret. You can, however, share the certificate that is generated using your app signing key.
- **Upload key**: The key you use to sign the app bundle or APK before you upload it for [app signing with Google Play](https://developer.android.com/studio/publish/app-signing#app-signing-google-play). You must keep the upload key secret. However, you can share the certificate that is generated using your upload key. You may generate an upload key in one of the following ways:
  - If you choose for Google to generate the app signing key for you when you opt in, then the key you use to [sign your app for release](https://developer.android.com/studio/publish/app-signing#sign-apk) is designated as your upload key;
  - If you provide the app signing key to Google when opting in your new or existing app, then you have the option to generate a new upload key during or after opting in for increased security;
  - If you do not generate a new upload key, you continue to use your app signing key as your upload key to sign each release.

## Best practices
- If you also distribute your app outside of Google Play or plan to later and want to use the same signing key, you have two options: 
  - Either let Google generate the key (recommended) and then download a signed, universal APK from the from [App bundle explorer](https://play.google.com/console/u/0/signup) to distribute outside of Google Play;
  - Or you can generate the app signing key you want to use for all app stores, and then transfer a copy of it to Google when you configure Play App Signing.
- To protect your account, turn on [2-Step Verification](https://safety.google/authentication/) for accounts with access to Play Console;
- After publishing an app bundle to a release track, you can visit the [App bundle explorer](https://play.google.com/console/u/0/signup) to access installable APKs that Google generates from your app bundle. You can:
  - Copy and share an [internal app sharing](https://support.google.com/googleplay/android-developer/answer/9844679) link that allows you to test, in a single tap, what Google Play would install from your app bundle on different devices;
  - Download a signed, universal APK. This single APK is signed with the app signing key that Google holds and is installable on any device that your app supports;
  - Download a ZIP archive with all of the APKs for a specific device. These APKs are signed with the app signing key that Google holds., Yand you can install the APKs in the ZIP archive on a device using the `adb install-multiple *.apk` command.
- For increased security, generate a new upload key that’s different from your app signing key;
- If you're using any Google API, you may want to register the upload key and app signing key certificates in the [Google Cloud Console](https://console.cloud.google.com/apis/dashboard?pli=1) for your app;
- If you're using [Android App Links](https://developer.android.com/training/app-links#android-app-links), make sure to update keys in the corresponding [Digital Asset Links JSON file](https://developer.android.com/training/app-links/verify-android-applinks) on your website.

## Conclusion
Advantages:
- Use the Android App Bundle and support Google Play’s advanced delivery modes. The Android App Bundle makes your app much smaller, your releases simpler, and makes it possible to use feature modules and offer instant experiences;
- Increase the security of your signing key, and make it possible to use a separate upload key to sign the app bundle you upload to Google Play;
- One time key upgrade for new installs lets you change your app signing key in case your existing one is compromised or if you need to migrate to a cryptographically stronger key.

# Links
[Play App Signing](https://developer.android.com/studio/publish/app-signing#app-signing-google-play)

[Use Play App Signing](https://support.google.com/googleplay/android-developer/answer/9842756) 

# Further Reading
[Answers to common questions about Play App Signing](https://medium.com/androiddevelopers/answers-to-common-questions-about-app-signing-by-google-play-b28fef836af0)
