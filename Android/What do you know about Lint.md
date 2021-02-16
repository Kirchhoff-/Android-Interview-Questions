# Lint
Android Studio provides a code scanning tool called lint that can help you to identify and correct problems with the structural quality of your code without your having to execute the app or write test cases. Each problem detected by the tool is reported with a description message and a severity level, so that you can quickly prioritize the critical improvements that need to be made. Also, you can lower the severity level of a problem to ignore issues that are not relevant to your project, or raise the severity level to highlight specific problems. The lint tool helps find poorly structured code that can impact the reliability and efficiency of your Android apps and make your code harder to maintain.

The lint tool checks your Android project source files for potential bugs and optimization improvements for correctness, security, performance, usability, accessibility, and internationalization. When using Android Studio, configured lint and IDE inspections run whenever you build your app.

For example, if your XML resource files contain unused namespaces, this takes up space and incurs unnecessary processing. Other structural issues, such as use of deprecated elements or API calls that are not supported by the target API versions, might lead to code failing to run correctly. Lint can help you clean up these issues.

In the image below we can see how lint tool processes the application source files:

![](./res/lint_tool.png "Code scanning workflow with the lint tool")

- **Application source files**. The source files consist of files that make up your Android project, including Java, Kotlin, and XML files, icons, and ProGuard configuration files.
- **The `lint.xml` file**. A configuration file that you can use to specify any lint checks that you want to exclude and to customize problem severity levels.
- **The lint tool**. A static code scanning tool that you can run on your Android project either from the command line or in Android Studio (see [Manually run inspections](https://developer.android.com/studio/write/lint#manuallyRunInspections)). The lint tool checks for structural code problems that could affect the quality and performance of your Android application. It is strongly recommended that you correct any errors that lint detects before publishing your application.
- **Results of lint checking**. You can view the results from lint either in the console or in the Inspection Results window in Android Studio.

## [Configure lint to suppress warnings](https://developer.android.com/studio/write/lint#config)
By default when you run a lint scan, the tool checks for all issues that lint supports. You can also restrict the issues for lint to check and assign the severity level for those issues. For example, you can suppress lint checking for specific issues that are not relevant to your project and, you can configure lint to report non-critical issues at a lower severity level.

You can configure lint checking for different levels:
- Globally (entire project);
- Project module;
- Production module;
- Test module;
- Open files;
- Class hierarchy;
- Version Control System (VCS) scopes.

## [Configure the lint file](https://developer.android.com/studio/write/lint#pref)
You can specify your lint checking preferences in the `lint.xml` file. If you are creating this file manually, place it in the root directory of your Android project.

The `lint.xml` file consists of an enclosing `<lint>` parent tag that contains one or more children `<issue>` elements. Lint defines a unique id attribute value for each `<issue>`.

```
<?xml version="1.0" encoding="UTF-8"?>
    <lint>
        <!-- list of issues to configure -->
</lint>
```

You can change an issue's severity level or disable lint checking for the issue by setting the severity attribute in the `<issue>` tag.

The following example shows the contents of a lint.xml file.
```
<?xml version="1.0" encoding="UTF-8"?>
<lint>
    <!-- Disable the given check in this project -->
    <issue id="IconMissingDensityFolder" severity="ignore" />

    <!-- Ignore the ObsoleteLayoutParam issue in the specified files -->
    <issue id="ObsoleteLayoutParam">
        <ignore path="res/layout/activation.xml" />
        <ignore path="res/layout-xlarge/activation.xml" />
    </issue>

    <!-- Ignore the UselessLeaf issue in the specified file -->
    <issue id="UselessLeaf">
        <ignore path="res/layout/main.xml" />
    </issue>

    <!-- Change the severity of hardcoded strings to "error" -->
    <issue id="HardcodedText" severity="error" />
</lint>
```

## [Configure lint options with Gradle](https://developer.android.com/studio/write/lint#gradle)
The Android plugin for Gradle allows you to configure certain lint options, such as which checks to run or ignore, using the `lintOptions {}` block in your module-level `build.gradle` file. The following code snippet shows you some of the properties you can configure:

```
android {
  ...
  lintOptions {
    // Turns off checks for the issue IDs you specify.
    disable 'TypographyFractions','TypographyQuotes'
    // Turns on checks for the issue IDs you specify. These checks are in
    // addition to the default lint checks.
    enable 'RtlHardcoded','RtlCompat', 'RtlEnabled'
    // To enable checks for only a subset of issue IDs and ignore all others,
    // list the issue IDs with the 'check' property instead. This property overrides
    // any issue IDs you enable or disable using the properties above.
    check 'NewApi', 'InlinedApi'
    // If set to true, turns off analysis progress reporting by lint.
    quiet true
    // if set to true (default), stops the build if errors are found.
    abortOnError false
    // if true, only report errors.
    ignoreWarnings true
  }
}
...
```

# Links
[Improve your code with lint checks](https://developer.android.com/studio/write/lint)
