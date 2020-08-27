From the Google-android source code

![NoDPInAnyDPI](https://raw.githubusercontent.com/swayangjit/Android-Interview-Questions/master/Android/res/nodpi_anydpi_source.png)

## nodpi: Fallback

 - A drawable inside the `res/drawable-nodpi/`  folder is valid for any
   screen density.
   Eg.
   **drawable-nodpi/ic_icon.png**
   The above icon will will look small on `xxxhdpi` devices and big on `ldpi` devices.

	  **drawable-hdpi/ic_icon.png**
	  **drawable-nodpi-21/ic_icon.xml**
	   In android 21 hdpi devices `ic_icon.png` will be used and in android 21 xhdpi devices `ic_icon.xml` will be used.



## anydpi: Takeover

 - A drawable inside `drawable-anydpi`  is also valid for any screen
   density but anydpi variant has more priority over any density
   specific variant.

	Eg. 
	If we have **res/drawable-anydpi/ic_icon.xml** and  **res/drawable-xxxhdpi/ic_icon.png** then `ic_icon.xml` will be used even in xxxhdpi devices.

	 -Most of the times `-anydpi` used in conjunction with other qualifiers. A good example is  `anydpi-v21` as vector drawables were introduced in android 21 , so after that  we usually have `res/drawable-anydpi-v21/ic_icon.xml` (Vector drawable) and `res/drawable-xxhdpi/ic_icon.png`. The vector drawable (`ic_icon.xml`) will be used in all android 21+ devices and `ic_icon.png` will be used in xxhdpi devices runing in android 4.4 or older
 