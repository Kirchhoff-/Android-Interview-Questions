# Raw folder vs Assets folder

The files in both directories will be stored intact in the APK package after being packaged and will not be compiled into binary systems.

**The differences between res/raw and assets:**

Since `raw` is a subfolder of Resources (`res`), Android will automatically generate an ID for any file located inside it. This ID is then stored an the `R` class that will act as a reference to a file, meaning it can be easily accessed from other Android classes and methods and even in Android XML files. Using the automatically generated ID is the fastest way to have access to a file in Android.

The Assets folder is an “appendix” directory. The `R` class does not generate IDs for the files placed there, so its less compatible with some Android classes and methods. Also, it’s much slower to access a file inside it, since you will need to get a handle to it based on a String. However some operations are more easily done by placing files in this folder, like copying a database file to the system’s memory. There’s no (easy) way to create an Android XML reference to files inside the Assets folder.

## Links
https://developer.android.com/guide/topics/resources/providing-resources  
https://stackoverflow.com/questions/9563373/when-should-i-use-the-assets-as-opposed-to-raw-resources-in-android   
https://topic.alibabacloud.com/a/the-difference-between-the-asset-folder-and-the-raw-folder-in-android-deep-parsing-_android_1_21_20133424.html    
