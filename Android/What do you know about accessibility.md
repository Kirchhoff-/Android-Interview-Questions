# Accessibility
Try to make your Android app usable for everyone, including people with accessibility needs.

People with impaired vision, color blindness, impaired hearing, impaired dexterity, cognitive disabilities, and many other disabilities use Android devices. When you develop apps with accessibility in mind, you make the user experience better for people with accessibility needs.<sup>[1](https://developer.android.com/guide/topics/ui/accessibility/apps#:~:text=Try%20to%20make%20your%20Android,for%20people%20with%20accessibility%20needs.)</sup>

The main benefits of the accessible apps are: 
- **Increase your app’s reach**. According to the World Bank, 15% of the world’s population has some type of disability. People with disabilities depend on apps and services that support accessibility to communicate, learn, and work. By making your app accessible, you can reach these users;
- **Improve your app’s versatility**. Accessibility features benefit all users. For example, if someone is using your app while they cook, they can use voice commands instead of touch gestures to navigate. Low vision features make apps useful in bright sunshine;
- **Meet government and institution requirements**. Many countries now require all products used by government employees to be accessible. Companies are following suit.

To assist users with accessibility needs, the Android framework lets you create an accessibility service that can present content from apps to users and also operate apps on their behalf.<sup>[2](https://developer.android.com/guide/topics/ui/accessibility/principles#:~:text=To%20assist%20users%20with%20accessibility%20needs%2C%20the%20Android%20framework%20lets%20you%20create%20an%20accessibility%20service%20that%20can%20present%20content%20from%20apps%20to%20users%20and%20also%20operate%20apps%20on%20their%20behalf.)</sup>

Android provides several system accessibility services, including the following:
- [**TalkBack**](https://support.google.com/accessibility/android/answer/6283677): helps people who have low vision or are blind. It announces content through a synthesized voice and performs actions on an app in response to user gestures;
- [**Switch Access**](https://support.google.com/accessibility/android/answer/6122836): helps people who have motor disabilities. It highlights interactive elements and performs actions in response to the user pressing a button. It allows for controlling the device using only one or two buttons.
 
## [How to Make Your Application Accessible](https://android-developers.googleblog.com/2012/04/accessibility-are-you-serving-all-your.html#:~:text=How%20to%20Make%20Your%20Application%20Accessible)
It would be great to be able to give you a standard recipe for accessibility, but the truth of the matter is that the right answer depends on the design and functionality of your application. Here are some key steps for ensuring that your application is accessible:
- **Task flows**. Design well-defined, clear task flows with minimal navigation steps, especially for major user tasks, and make sure those tasks are navigable via focus controls (see item 4);
- **Action target size**. Make sure buttons and selectable areas are of sufficient size for users to easily touch them, especially for critical actions. How big? We recommend that touch targets be 48dp (roughly 9mm) or larger;
- **Label user interface controls**. Label user interface components that do not have visible text, especially `ImageButton`, `ImageView`, and `EditText` components. Use the `android:contentDescription` XML layout attribute or `setContentDescription()` to provide this information for accessibility services;
- **Enable focus-based navigation**. Make sure users can navigate your screen layouts using hardware-based or software directional controls (D-pads, trackballs and keyboards). In a few cases, you may need to make UI components focusable or change the focus order to be more logical;
- **Use framework-provided controls**: Use Android's built-in user interface controls whenever possible, as these components provide accessibility support by default;
- **Custom view controls**: If you build custom interface controls for your application, implement accessibility interfaces for your custom views and provide text labels for the controls;
- **Test**. Checking off the items on this list doesn’t guarantee your app is accessible. Test accessibility by attempting to navigate your application using directional controls, and also try eyes free navigation with the TalkBack service enabled.

## [Make media content more accessible](https://developer.android.com/guide/topics/ui/accessibility/principles#media-content)
If you're developing an app that includes media content, such as a video clip or an audio recording, try to support users with different types of accessibility needs in understanding this material. In particular, we encourage you to do the following:
- Include controls that allow users to pause or stop the media, change the volume, and toggle subtitles (captions);
- If a video presents information that is vital to completing a workflow, provide the same content in an alternate format, such as a transcript.

## [Describe each UI element](https://developer.android.com/guide/topics/ui/accessibility/apps#describe-ui-element)
For each UI element in your app, include a description that describes the element's purpose. In most cases, you include this description in the element's `contentDescription` attribute, as shown in the following code snippet:
```
<!-- Use string resources for easier localization. -->
<!-- The en-US value for the following string is "Inspect". -->
<ImageView
    ...
    android:contentDescription="@string/inspect" />
```

When adding descriptions to your app's UI elements, keep the following best practices in mind:
- Don't include the type of UI element in the content description. Screen readers automatically announce both the element's type and description. For example, if selecting a button causes a "submit" action to occur in your app, make the button's description "**Submit**", not "**Submit button**";
- Each description must be unique. That way, when screen reader users encounter a repeated element description, they correctly recognize that the focus is on an element that already had focus earlier. In particular, each item within a view group such as `RecyclerView` must have a different description. Each description must reflect the content that's unique to a given item, such as the name of a city in a list of locations;
- If your app's `minSdkVersion` is `16` or higher, you can set the `android:importantForAccessibility` attribute to `"no"` for graphical elements that are only used for decorative effect.

**Note**: Don't provide descriptions for `TextView` elements. Android accessibility services automatically announce the text itself as the description.

## [Accessibility Scanner](https://developer.android.com/guide/topics/ui/accessibility/testing#accessibility-scanner)
The [Accessibility Scanner](https://play.google.com/store/apps/details?id=com.google.android.apps.accessibility.auditor) app scans your screen and suggests ways to improve the accessibility of your app. Accessibility Scanner uses the [Accessibility Test Framework](https://github.com/google/Accessibility-Test-Framework-for-Android) and provides specific suggestions after looking at content labels, clickable items, contrast, and more.

The Android Accessibility Test Framework is integrated in Android Studio to help you find accessibility issues in your layouts. To launch the panel, click the error report button `!` in the Layout Editor.

**Note**: Keep in mind that the Android Accessibility Test Framework in Android Studio can't detect issues that occur when the app is running on a device.

# Links
[Build accessible apps](https://developer.android.com/guide/topics/ui/accessibility)

[Make apps more accessible](https://developer.android.com/guide/topics/ui/accessibility/apps)

[Accessibility: Are You Serving All Your Users?](https://android-developers.googleblog.com/2012/04/accessibility-are-you-serving-all-your.html)

# Further reading
[Make your Android app more accessible](https://developer.android.com/courses/pathways/make-your-android-app-accessible)

[Get started with Accessibility Scanner](https://support.google.com/accessibility/android/answer/6376570)

[Accessibility Scanner results](https://support.google.com/accessibility/android/answer/6376559)
