# Object keyword
Kotlin has an `object` keyword, which combines both declaring a class and creating an instance of it in one action. It can be used in three different situations and has three different meanings

- *Object declaration*. Defines a singleton class;
- *Companion object*. Defines a nested class that can hold members related to the outer containing class;
- *Object expression*. Creates an instance of the object on the fly, the same as Java's anonymous inner classes.

## Object declaration
In Kotlin, as in almost all JVM languages, there's the concept of a class as the core of the Object-Oriented Programming model. Kotlin introduces the concept of an object on top of that.

Whereas a class describes structures that can be instantiated as and when desired and allows for as many instances as needed, an object instead represents a single static instance, and can never have any more or any less than this one instance. This pattern is known as a singleton. In other languages, you would have to implement this pattern manually, making sure that only one instance of your type gets created. But, Kotlin has language support for creating singletons with the object keyword. 
Example:

```
object TestSingleton {
  val testValue = 42
  
  fun testFunc(name:String) = "Hello, $name!"
}
```

## Companion object
Every class can implement a companion object, which is an object that is common to all instances of that class. It’d come to be similar to static fields in Java.

```
class MyClass {
    companion object Factory {
        fun create(): MyClass = MyClass()
    }
}
```

Companion objects allow their members to be accessed from inside the companion class without specifying the name. At the same time, visible members can be accessed from outside the class when prefixed by the class name:

```
class TestVisibilityClass {
    companion object {
        private val secret = "Secret" //Not visible from outside
        val public = "Public" //Visible from outside
    }
}
```

## Object expression
Objects can also be used to create anonymous class implementations. With them, you create objects on the fly.

```
window.addMouseListener(object : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) { /*...*/ }

    override fun mouseEntered(e: MouseEvent) { /*...*/ }
})
```

Note that anonymous objects can be used as types only in local and private declarations. If you use an anonymous object as a return type of a public function or the type of a public property, the actual type of that function or property will be the declared supertype of the anonymous object, or Any if you didn't declare any supertype. Members added in the anonymous object will not be accessible.

```
class C {
    // Private function, so the return type is the anonymous object type
    private fun foo() = object {
        val x: String = "x"
    }

    // Public function, so the return type is Any
    fun publicFoo() = object {
        val x: String = "x"
    }

    fun bar() {
        val x1 = foo().x        // Works
        val x2 = publicFoo().x  // ERROR: Unresolved reference 'x'
    }
}
```

## Semantic difference between object expressions and declarations
There is one important semantic difference between object expressions and object declarations:
- object expressions are executed (and initialized) **immediately**, where they are used;
- object declarations are initialized **lazily**, when accessed for the first time;
- a companion object is initialized when the corresponding class is loaded (resolved), matching the semantics of a Java static initializer.

## Links
https://kotlinlang.org/docs/reference/object-declarations.html  
https://en.proft.me/2019/08/26/object-keyword-kotlin/  
https://www.baeldung.com/kotlin-objects  
https://antonioleiva.com/objects-kotlin/  
