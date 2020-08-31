
  

1. **Bundle savedInstanceState** is a reference to the Bundle object that is passed to the `onCreate()` in an Activity. Activities have capability to restore themselves to a previous state using the stored in the Bundle.

2. First time the `bundle` object is always `null`. If some data is stored in the bundle object during the Activity lifecycle and Activity is recreated due to rotation then the `bundle` object will be `non-null`.

	During the Activity Lifecycle save the data in `onSaveInstanceState()`

	```

	int scrolledPosition = 0;  
	  
	@Override  
	public void onSaveInstanceState(Bundle outState) {  
	    super.onSaveInstanceState(outState);  
	    outState.putInt("scrolled_position",scrolledPosition);  
	}

	```

	After the orientation when the Activity is recreated and `onCreate()` is called

	  ```
	  int scrolledPosition = 0;
	  
	  @Override  
	public void onCreate(Bundle savedInstanceState) {  
	    super.onCreate(savedInstanceState);  
	    if(savedInstanceState == null){  
	        scrolledPosition = 0;  
	    }else{  
	        scrolledPosition = savedInstanceState.getInt("scrolled_position",0);  
	    }  
	}
	```

	There is an alternative way of restoring state , `onRestoreInstanceState()` which system calls after `onStart()`. The system calls `onRestoreInstanceState()` only if there is a state to restore. So there is no need to check if `bundle` object is `null`.

		  int scrolledPosition = 0;
		  
	      public void onRestoreInstanceState(Bundle savedInstanceState) {  
		     // Always call the superclass so it can restore the view hierarchy    
		     super.onRestoreInstanceState(savedInstanceState);  
      
	        // Restore state members from saved instance    
		     scrolledPosition = savedInstanceState.getInt("scrolled_position",0);  
	    }

 


	  **Note:**
	Always call the superclass implementation of `onRestoreInstanceState()`so the default implementation can restore the state of the view hierarchy.


	**What kind of data should be stored in `bundle` object**

	  

	1. User selections

	2. Scroll positions

	3. User submitted data

  

	**What kind of data shouldn't be stored in `bundle` object**

	  

	1. Bitmap

	2. File

	3. Big Model classes(POJO)

	**Reference Link:**

	https://developer.android.com/guide/components/activities/activity-lifecycle#restore-activity-ui-state-using-saved-instance-state
