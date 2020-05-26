# installLocation tag

`installLocation` is the tag in AndroidManifest that configure default install location for the app. The following keyword strings are accepted:

| Value | Description |
|---|---|
| internalOnly | The app must be installed on the internal device storage only. If this is set, the app will never be installed on the external storage. If the internal storage is full, then the system will not install the app. This is also the default behavior if you do not define `android:installLocation`. |
| auto | The app may be installed on the external storage, but the system will install the app on the internal storage by default. If the internal storage is full, then the system will install it on the external storage. Once installed, the user can move the app to either internal or external storage through the system settings. |
| preferExternal | 	The app prefers to be installed on the external storage (SD card). There is no guarantee that the system will honor this request. The app might be installed on internal storage if the external media is unavailable or full. Once installed, the user can move the app to either internal or external storage through the system settings. |

When an app is installed on the external storage:
- The `.apk` file is saved to the external storage, but any app data (such as databases) is still saved on the internal device memory.
- The container in which the `.apk` file is saved is encrypted with a key that allows the app to operate only on the device that installed it. (A user cannot transfer the SD card to another device and use apps installed on the card.) Though, multiple SD cards can be used with the same device.
- At the user's request, the app can be moved to the internal storage.

The user may also request to move an app from the internal storage to the external storage. However, the system will not allow the user to move the app to external storage if this attribute is set to `internalOnly`, which is the default setting.

## Links   
https://developer.android.com/guide/topics/manifest/manifest-element  
https://developer.android.com/guide/topics/data/install-location  
