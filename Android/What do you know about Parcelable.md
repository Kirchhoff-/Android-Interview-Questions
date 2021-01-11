# Parcelable

`Parcelable` used for pass data between components. `Parcelable` is the analog of Java `Serializable` interface with optimization for mobile devices. It assumes a certain structure and way of processing it.

Classes implementing the `Parcelable` interface must also have a non-null static field called `CREATOR` of a type that implements the `Parcelable.Creator` interface.

A typical implementation of `Parcelable` is:

```
 public class MyParcelable implements Parcelable {
     private int mData;

     public int describeContents() {
         return 0;
     }

     public void writeToParcel(Parcel out, int flags) {
         out.writeInt(mData);
     }

     public static final Parcelable.Creator<MyParcelable> CREATOR
             = new Parcelable.Creator<MyParcelable>() {
         public MyParcelable createFromParcel(Parcel in) {
             return new MyParcelable(in);
         }

         public MyParcelable[] newArray(int size) {
             return new MyParcelable[size];
         }
     };

     private MyParcelable(Parcel in) {
         mData = in.readInt();
     }
 }
 ```
 
We can now pass the parcelable data between activities within an intent:
```
MyParcelable info = new MyParcelable();
Intent i = new Intent(this, NewActivity.class);
i.putExtra("parcelableKey", info);
startActivity(i);
```

For obtaining information in `NewActivity` should do next:
```
public class NewActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        MyParcelable info = (MyParcelable) getIntent().getParcelableExtra("parcelableKey");
    }
}
```

Resources for simplified generate the boilerplate code for creating `Parcelables`:
- [parcelabler](http://www.parcelabler.com/)
- [Android studio plugin](https://github.com/mcharmas/android-parcelable-intellij-plugin)
- [Kotlin `@Parcelize`](https://kotlinlang.org/docs/reference/compiler-plugins.html#parcelable-implementations-generator)

## Links
https://developer.android.com/reference/android/os/Parcelable  
https://guides.codepath.com/android/using-parcelable  
https://www.vogella.com/tutorials/AndroidParcelable/article.html
