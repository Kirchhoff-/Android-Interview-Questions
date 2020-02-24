# Companion objects

The companion object is a singleton, and its members can be accessed directly via the name of the containing class (although you can also insert the name of the companion object if you want to be explicit about accessing the companion object). A companion object is initialized when the class is loaded (typically the first time it's referenced by other code that is being executed), in a thread-safe manner. 

A class can only have one companion object, and companion objects can not be nested. Companion objects and their members can only be accessed via the containing class name, not via instances of the containing class. If you try to redeclare a companion object in a subclass, you'll just shadow the one from the base class. If you need an overridable "class-level" function, make it an ordinary open function in which you do not access any instance members - you can override it in subclasses, and when you call it via an object instance, the override in the object's class will be called. It is possible, but inconvenient, to call functions via a class reference in Kotlin, so we won't cover that here.

## Links
https://kotlinlang.org/docs/tutorials/kotlin-for-py/objects-and-companion-objects.html  
https://proandroiddev.com/a-true-companion-exploring-kotlins-companion-objects-dbd864c0f7f5  
https://blog.kotlin-academy.com/a-few-facts-about-companion-objects-37e18429b725  
