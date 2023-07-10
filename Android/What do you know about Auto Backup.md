# What do you know about Auto Backup?
*Auto Backup for Apps* automatically backs up a user's data from apps that target and run on Android 6.0 (API level 23) or higher. Android preserves app data by uploading it to the user's Google Drive, where it's protected by the user's Google account credentials. The backup is end-to-end encrypted on devices running Android 9 or higher using the device's PIN, pattern, or password. 

Backup data is stored in a private folder in the user's Google Drive account, limited to 25 MB per app. There's no charge for storing backup data. The saved data does not count toward the user's personal Google Drive quota. Only the most recent backup is stored. When a backup is made, any previous backup is deleted. The backup data can't be read by the user or other apps on the device. Your app can customize the backup process or opt out by disabling backups.

From Android 12 (API level 31) Android backup and restore has two forms:
- **Cloud backups**: User data is stored in a user’s Google Drive so that it can later be restored on that device or a new device;
- **Device-to-device (D2D)** transfers: User data is sent directly to the user's new device from their older device, such as by using a cable.

## [Files that are backed up](https://developer.android.com/guide/topics/data/autobackup#Files)
By default, Auto Backup includes files in most of the directories that are assigned to your app by the system:
- Shared preferences files;
- Files saved to your app's internal storage and accessed by `getFilesDir()` or `getDir(String, int)`;
- Files in the directory returned by `getDatabasePath(String)`, which also includes files created with the `SQLiteOpenHelper` class;
- Files on external storage in the directory returned by `getExternalFilesDir(String)`.

Auto Backup excludes files in directories returned by `getCacheDir()`, `getCodeCacheDir()`, and `getNoBackupFilesDir()`. The files saved in these locations are needed only temporarily and are intentionally excluded from backup operations.

## [Enable and disable backup](https://developer.android.com/guide/topics/data/autobackup#EnablingAutoBackup)
Apps that target Android 6.0 (API level 23) or higher automatically participate in Auto Backup. In your app manifest file, set the boolean value `android:allowBackup` to enable or disable backup. The default value is `true`, but we recommend explicitly setting the attribute in your manifest, as shown below:
```
<manifest ... >
    ...
    <application android:allowBackup="true" ... >
        ...
    </application>
</manifest>
```

You can disable backups by setting `android:allowBackup` to `false`. You might want to do this if your app can recreate its state through some other mechanism or if your app deals with sensitive information.

**Note**. For apps running on and targeting Android 12 (API level 31) or higher, specifying `android:allowBackup="false"` disables backups to Google Drive but doesn’t disable device-to-device transfers for the app.

## [Control backup on Android 11 and lower](https://developer.android.com/guide/topics/data/autobackup#include-exclude-android-11)
In your `AndroidManifest.xml` file, add the `android:fullBackupContent` attribute to the `<application>` element, as shown in the following example. This attribute points to an XML file that contains backup rules.
```
<application ...
 android:fullBackupContent="@xml/backup_rules">
</application>
```

Create an XML file called `@xml/backup_rules` in the `res/xml/ directory`. In this file, add rules with the `<include>` and `<exclude>` elements. The following sample backs up all shared preferences except `device.xml`:
```
<?xml version="1.0" encoding="utf-8"?>
<full-backup-content>
 <include domain="sharedpref" path="."/>
 <exclude domain="sharedpref" path="device.xml"/>
</full-backup-content>
```

## [Control backup on Android 12 or higher](https://developer.android.com/guide/topics/data/autobackup#include-exclude-android-12)
In your `AndroidManifest.xml` file, add the `android:dataExtractionRules` attribute to the `<application>` element, as shown in the following example. This attribute points to an XML file that contains backup rules.
```
<application ...
 android:dataExtractionRules="backup_rules.xml">
</application>
```

Create an XML file called `@xml/backup_rules` in the `res/xml/ directory`. In this file, add rules with the `<include>` and `<exclude>` elements. The following sample backs up all shared preferences except `device.xml`:
```
<?xml version="1.0" encoding="utf-8"?>
<data-extraction-rules>
 <cloud-backup [disableIfNoEncryptionCapabilities="true|false"]>
   <include domain="sharedpref" path="."/>
   <exclude domain="sharedpref" path="device.xml"/>
 </cloud-backup>
</data-extraction-rules>
```

## [Implement BackupAgent](https://developer.android.com/guide/topics/data/autobackup#ImplementingBackupAgent)
Apps that implement Auto Backup don't need to implement a [`BackupAgent`](https://developer.android.com/reference/android/app/backup/BackupAgent). However, you can optionally implement a custom `BackupAgent`. Typically, there are two reasons for doing this:
- You want to receive notification of backup events, such as `onRestoreFinished()` and `onQuotaExceeded(long, long)`. These callback methods are executed even if the app is not running.
- You can't easily express the set of files you want to back up with XML rules. In these rare cases, you can implement a `BackupAgent` that overrides `onFullBackup(FullBackupDataOutput)` to store what you want. To retain the system's default implementation, call the corresponding method on the superclass with `super.onFullBackup()`.

# Links
[Back up user data with Auto Backup](https://developer.android.com/guide/topics/data/autobackup)

# Further Reading
[Android’s Attribute `android:allowBackup` Demystified](https://betterprogramming.pub/androids-attribute-android-allowbackup-demystified-114b88087e3b)
