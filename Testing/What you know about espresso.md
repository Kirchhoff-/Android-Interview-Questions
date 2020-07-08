# Espresso
Espresso is an open source android user interface (UI) testing framework developed by Google. Espresso is a simple, efficient and flexible testing framework. Google released the Espresso framework in Oct. 2013. Since its 2.0 release Espresso is part of the Android Support Repository. Espresso tests can be written in both Java and Kotlin, a modern programming language to develop android application.

Espresso automatically synchronizes your test actions with the user interface of your application. The framework also ensures that your activity is started before the tests run. It also let the test wait until all observed background activities have finished. The Espresso API is simple and easy to learn. You can easily perform Android UI tests without the complexity of multi-threaded testing. Google Drive, Maps and some other applications are currently using Espresso.

Espresso Testing works basically in three blocks:

- `ViewMatchers` – allows you to find an item in the view
- `ViewActions` – allows to execute actions on the elements
- `ViewAssertions` – validate a view state

Base Espresso Test
```
onView(ViewMatcher)   //1     
 .perform(ViewAction) // 2    
   .check(ViewAssertion);  //3
```
1. Finds the view
2. Performs an action on the view
3. Validates a assertioin

Features of Espresso:
- Very simple API and so, easy to learn.
- Highly scalable and flexible.
- Provides separate module to test Android WebView component.
- Provides separate module to validate as well as mock Android Intents.
- Provides automatic synchronization between your application and tests.

Advantages of Espresso:
- Backward compatibility
- Easy to setup.
- Highly stable test cycle.
- Supports testing activities outside application as well.
- Supports JUnit4
- UI automation suitable for writing black box tests.

## Links
https://developer.android.com/training/testing/espresso  
https://www.vogella.com/tutorials/AndroidTestingEspresso/article.html  
https://www.tutorialspoint.com/espresso_testing/index.htm  
https://apiumhub.com/tech-blog-barcelona/introduction-espresso-testing/  
