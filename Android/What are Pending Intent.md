## PendingIntent

A **PendingIntent** specifies an action to take in the future. It lets you pass a future Intent to another application and allow that application to execute that Intent as if it had the same permissions as your application, whether or not your application is still around when the Intent is eventually invoked. 

By giving a **PendingIntent** to another application, you are granting it the right to perform the operation you have specified as if the other application was yourself (with the same permissions and identity). As such, you should be careful about how you build the **PendingIntent**: often, for example, the base Intent you supply will have the component name explicitly set to one of your own components, to ensure it is ultimately sent there and nowhere else.

It is an Intent action that you want to perform but at a later time. The reason it’s needed is because an Intent must be created and launched from a valid context in your application, but there are certain cases where one is not available at the time you want to run the action because you are technically outside the application’s context. 

## Links
https://developer.android.com/reference/android/app/PendingIntent  
https://medium.com/programming-lite/pendingintent-in-android-application-development-61818c90458b  
https://www.bradcypert.com/android-pending-intents/
