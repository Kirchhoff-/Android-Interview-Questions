# Chrome custom tabs
App developers face a choice when a user taps a URL to either launch a browser, or build their own in-app browser using WebViews.

Both options present challenges - launching the browser is a heavy context switch for users that isn't customizable, while WebViews don't support all features of the web platform, don't share state with the browser and add maintenance overhead.

Custom Tabs is a browser feature, introduced by Chrome, that is now supported by most major browsers on Android. It give apps more control over their web experience, and make transitions between native and web content more seamless without having to resort to a `WebView`.

Custom Tabs allow an app to customize how the browser looks and feels. An app can change things like:
- Toolbar color;
- Enter and exit animations;
- Add custom actions to the browser toolbar, overflow menu and bottom toolbar.

Custom Tabs also allow the developer to pre-start the browser and pre-fetch content for faster loading. Custom Tabs is a feature supported by browsers on the Android platform. It was originally introduced by Chrome, on version 45. Currentely, the protocol is supported by most Android browsers.

## [When should I use Custom Tabs vs WebView?](https://developers.google.com/web/android/custom-tabs#when_should_i_use_custom_tabs_vs_webview)
The `WebView` is good solution if you are hosting your own content inside your app. If your app directs people to URLs outside your domain, we recommend that you use Custom Tabs for these reasons:
- Support for the same web platform features and capabilities as the browsers;
- Simple to implement. No need to build code to manage requests, permission grants or cookie stores;
- UI customization:
  - Toolbar color
  - Action button
  - Custom menu items
  - Custom in/out animations
  - Bottom toolbar
- Navigation awareness: the browser delivers a callback to the application upon an external navigation;
- Security: the browser uses Google's Safe Browsing to protect the user and the device from dangerous sites;
- Performance optimization:
  - Pre-warming of the Browser in the background, while avoiding stealing resources from the application.
  - Providing a likely URL in advance to the browser, which may perform speculative work, speeding up page load time.
- Lifecycle management: the browser prevents the application from being evicted by the system while on top of it, by raising its importance to the "foreground" level;
- Shared cookie jar and permissions model so users don't have to sign-in to sites they are already connected to, or re-grant permissions they have already granted;
- If the user has turned on Data Saver, they will still benefit from it;
- Synchronized AutoComplete across devices for better form completion;
- Simple customization model;
- Quickly return to app with a single tap;
- You want to use the latest browser implementations on devices pre-Lollipop (auto updating `WebView`) instead of older `WebViews`.

# Links
https://developers.google.com/web/android/custom-tabs  
https://developers.google.com/web/android/custom-tabs/implementation-guide
