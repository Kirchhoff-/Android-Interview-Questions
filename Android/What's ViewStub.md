# ViewStub

A `ViewStub` is an invisible, zero-sized View that can be used to lazily inflate layout resources at runtime. When a ViewStub is made visible, or when `inflate()` is invoked, the layout resource is inflated. The ViewStub then replaces itself in its parent with the inflated View or Views. Therefore, the ViewStub exists in the view hierarchy until `setVisibility(int)` or `inflate()` is invoked. The inflated View is added to the ViewStub's parent with the ViewStub's layout parameters. Similarly, you can define/override the inflate View's id by using the ViewStub's inflatedId property. For instance:

```xml
<ViewStub 
  android:id="@+id/stub"
  android:inflatedId="@+id/subTree"
  android:layout="@layout/mySubTree"
  android:layout_width="120dip"
  android:layout_height="40dip" />
```
The `ViewStub` thus defined can be found using the id "stub." After inflation of the layout resource "mySubTree," the `ViewStub` is removed from its parent. The `View` created by inflating the layout resource "mySubTree" can be found using the id "subTree," specified by the inflatedId property. The inflated `View` is finally assigned a width of 120dip and a height of 40dip. The preferred way to perform the inflation of the layout resource is the following:

`ViewStub stub = findViewById(R.id.stub);
 View inflated = stub.inflate();`

When `inflate()` is invoked, the `ViewStub` is replaced by the inflated View and the inflated View is returned. This lets applications get a reference to the inflated `View` without executing an extra `findViewById()`.

## Links
https://developer.android.com/reference/android/view/ViewStub    
https://stackoverflow.com/questions/11577777/how-to-use-view-stub-in-android    
https://proandroiddev.com/viewstub-on-demand-inflate-view-or-inflate-lazily-layout-resource-e56b8c39398b    
