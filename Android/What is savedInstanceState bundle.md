
 1. **Bundle savedInstanceState**  is a reference to the Bundle object that is passed to the `onCreate()` in an Activity. Activities have capability to restore themselves to a previous state using the stored in the Bundle.
 2. First time the `bundle` object is always null. If some data is stored in the bundle object during the Activity lifecycle and Activity is recreated due to rotation then the `bundle` object will be non null.
 
 
During the  Activity Lifecycle save the data in `onSaveInstanceState()`
```
int scrolledPosition  = 0;
 @Override
 public void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    outState.putInt("scrolled_position",scrolledPosition);
 }
```
After the orientation when the Activity is recreated and onCreate() is called


    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    
        if(savedInstanceState == null){
            scrolledPosition = 0;
        }else{
            scrolledPosition = savedInstanceState.getInt("scrolled_position",0);
        }
    }

**What kind of data should be stored in `bundle` object**

 3. User selections
 4. Scroll positions
 5. UserSubmitted data

**What kind of data shouldn't be stored in `bundle` object**

 1. Bitmap
 2. File
 3. Big Model classes(POJO)
