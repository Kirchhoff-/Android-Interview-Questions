# Sticky Intent

Sticky Intent - Sticks with Android, for future broadcast listeners. For example if *BATTERY_LOW* event occurs then that intent will stick with Android so that any future requests for *BATTERY_LOW*, will return the intent. Sticky Intent is also a type of Intent which allows communication between a function and a service sendStickyBroadcast(), performs a sendBroadcast(Intent), the Intent you are sending stays around after the broadcast is complete, so that others can quickly retrieve that data through the return value of registerReceiver(BroadcastReceiver, IntentFilter). In all other ways, this behaves the same as sendBroadcast(Intent).

## Links
https://stackoverflow.com/questions/26038839/knowing-about-sticky-intent-in-android  
