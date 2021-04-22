# JvmOverloads annotation
Instructs the Kotlin compiler to generate overloads for this function that substitute default parameter values.

If a method has *N* parameters and *M* of which have default values, *M* overloads are generated: the first one takes *N-1* parameters (all but the last one that takes a default value), the second takes *N-2* parameters, and so on.

Functions with parameters having a default value must use `@JvmOverloads`. Without this annotation it is impossible to invoke the Kotlin function using any default values from Java.

When using `@JvmOverloads`, inspect the generated methods to ensure they each make sense. If they do not, perform one or both of the following refactorings until satisfied:
- Change the parameter order to prefer those with defaults being towards the end.
- Move the defaults into manual function overloads.

*Incorrect*: No `@JvmOverloads`
```
class Greeting {
    fun sayHello(prefix: String = "Mr.", name: String) {
        println("Hello, $prefix $name")
    }
}
```
```
public class JavaClass {
    public static void main(String... args) {
        Greeting greeting = new Greeting();
        greeting.sayHello("Mr.", "Bob");
    }
}
```

*Correct*: `@JvmOverloads` annotation.
```
class Greeting {
    @JvmOverloads
    fun sayHello(prefix: String = "Mr.", name: String) {
        println("Hello, $prefix $name")
    }
}
```
```
public class JavaClass {
    public static void main(String... args) {
        Greeting greeting = new Greeting();
        greeting.sayHello("Bob");
    }
}
```

# Links
[JvmOverloads](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.jvm/-jvm-overloads/#jvmoverloads)  
[Kotlin-Java interop guide](https://developer.android.com/kotlin/interop)

# Futher reading
[Misconception about Kotlin @JvmOverloads for Android View Creation](https://proandroiddev.com/misconception-about-kotlin-jvmoverloads-for-android-view-creation-cb88f432e1fe)  
[@JvmOverloads for Android Views](https://zsmb.co/jvmoverloads-for-android-views/)
