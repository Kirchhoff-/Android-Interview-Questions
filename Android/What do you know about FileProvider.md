# FileProvider
`FileProvider` is a special subclass of `ContentProvider` that facilitates secure sharing of files associated with an app by creating a `content://` `Uri` for a file instead of a `file:///` `Uri`.

A content URI allows you to grant read and write access using temporary access permissions. When you create an `Intent` containing a content URI, in order to send the content URI to a client app, you can also call `Intent.setFlags()` to add permissions. These permissions are available to the client app for as long as the stack for a receiving `Activity` is active. For an `Intent` going to a `Service`, the permissions are available as long as the `Service` is running.

In comparison, to control access to a `file:///` `Uri` you have to modify the file system permissions of the underlying file. The permissions you provide become available to *any* app, and remain in effect until you change them. This level of access is fundamentally insecure.

The increased level of file access security offered by a content URI makes `FileProvider` a key part of Android's security infrastructure.

## [Defining a FileProvider](https://developer.android.com/reference/androidx/core/content/FileProvider#ProviderDefinition)
Since the default functionality of `FileProvider` includes content URI generation for files, you don't need to define a subclass in code. Instead, you can include a `FileProvider` in your app by specifying it entirely in XML. To specify the `FileProvider` component itself, add a `<provider>` element to your app manifest. Set the `android:name` attribute to `androidx.core.content.FileProvider`. Set the `android:authorities` attribute to a URI authority based on a domain you control; for example, if you control the domain `mydomain.com` you should use the authority `com.mydomain.fileprovider`.  Set the `android:exported` attribute to `false`; the `FileProvider` does not need to be public. Set the `android:grantUriPermissions` attribute to `true`, to allow you to grant temporary access to files. For example:

```
<manifest>
    ...
    <application>
        ...
        <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="com.mydomain.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            ...
        </provider>
        ...
    </application>
</manifest>
```

## [Specifying Available Files](https://developer.android.com/reference/androidx/core/content/FileProvider#SpecifyFiles)
A `FileProvider` can only generate a content `URI` for files in directories that you specify beforehand. To specify a directory, specify its storage area and path in XML, using child elements of the `<paths>` element. For example, the following `paths` element tells FileProvider that you intend to request content URIs for the `images/` subdirectory of your private file area.

```
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <files-path name="my_images" path="images/"/>
    ...
</paths>
```

The `<paths>` element must contain one or more of the following child elements:
```
<files-path name="name" path="path" />
```

These child elements all use the same attributes:

`name="name"`. A URI path segment. To enforce security, this value hides the name of the subdirectory you're sharing. The subdirectory name for this value is contained in the `path` attribute.

`path="path"`. The subdirectory you're sharing. While the `name` attribute is a URI path segment, the `path` value is an actual subdirectory name. Notice that the value refers to a **subdirectory**, not an individual file or files. You can't share a single file by its file name, nor can you specify a subset of files using wildcards. 

You must specify a child element of `<paths>` for each directory that contains files for which you want content URIs. For example, these XML elements specify two directories:
```
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <files-path name="my_images" path="images/"/>
    <files-path name="my_docs" path="docs/"/>
</paths>
```

## [Generating the Content URI for a File](https://developer.android.com/reference/androidx/core/content/FileProvider#GetUri)
To share a file with another app using a content URI, your app has to generate the content URI. To generate the content URI, create a new `File` for the file, then pass the `File` to `getUriForFile()`. You can send the content URI returned by `getUriForFile()` to another app in an `Intent`. The client app that receives the content URI can open the file and access its contents by calling `ContentResolver.openFileDescriptor` to get a `ParcelFileDescriptor`.

For example, suppose your app is offering files to other apps with a `FileProvider` that has the authority `com.mydomain.fileprovider`. To get a content URI for the file default_image.jpg in the images/ subdirectory of your internal storage add the following code:

```
File imagePath = new File(Context.getFilesDir(), "images");
File newFile = new File(imagePath, "default_image.jpg");
Uri contentUri = getUriForFile(getContext(), "com.mydomain.fileprovider", newFile);
```

As a result of the previous snippet, `getUriForFile()` returns the content URI `content://com.mydomain.fileprovider/my_images/default_image.jpg`.

## [Granting Temporary Permissions to a URI](https://developer.android.com/reference/androidx/core/content/FileProvider#Permissions)
To grant an access permission to a content URI returned from `getUriForFile()`, you can either grant the permission to a specific package or include the permission in an intent, as shown in the following sections.

### [Grant Permission to a Specific Package](https://developer.android.com/reference/androidx/core/content/FileProvider#grant-permission-to-a-specific-package)
Call the method `Context.grantUriPermission(package, Uri, mode_flags)` for the `content://` `Uri`, using the desired mode flags. This grants temporary access permission for the content URI to the specified package, according to the value of the the `mode_flags` parameter, which you can set to `Intent.FLAG_GRANT_READ_URI_PERMISSION`, `Intent.FLAG_GRANT_WRITE_URI_PERMISSION` or both. The permission remains in effect until you revoke it by calling `revokeUriPermission()` or until the device reboots.

### [Include the Permission in an Intent](https://developer.android.com/reference/androidx/core/content/FileProvider#include-the-permission-in-an-intent)
To allow the user to choose which app receives the intent, and the permission to access the content, do the following:
- Put the content URI in an `Intent` by calling `setData()`.
- Call the method `Intent.setFlags()` with either `Intent.FLAG_GRANT_READ_URI_PERMISSION` or `Intent.FLAG_GRANT_WRITE_URI_PERMISSION` or both.
- Send the `Intent` to another app. Most often, you do this by calling `setResult()`.

Permissions granted in an `Intent` remain in effect while the stack of the receiving `Activity` is active. When the stack finishes, the permissions are automatically removed. Permissions granted to one `Activity` in a client app are automatically extended to other components of that app.

# Links
https://developer.android.com/reference/androidx/core/content/FileProvider

# Next questions
[What is ContentProvider?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What's%20ContentProvider.md)
