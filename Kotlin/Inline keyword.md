# Inline keyword

## Inline functions 

Using higher-order functions imposes certain runtime penalties: each function is an object, and it captures a closure, i.e. those variables that are accessed in the body of the function. Memory allocations (both for function objects and classes) and virtual calls introduce runtime overhead.

But it appears that in many cases this kind of overhead can be eliminated by inlining the lambda expressions. Inlining is basically requesting the compiler to copy the (inlined) code at the calling place.

Consider the following case:

`lock(l) { foo() }`

Instead of creating a function object for the parameter and generating a call, the compiler could emit the following code:
```
l.lock()
try {
    foo()
}
finally {
    l.unlock()
}
```

To make the compiler do this, we need to mark the lock() function with the inline modifier:

`inline fun <T> lock(lock: Lock, body: () -> T): T { ... }`

The `inline` modifier affects both the function itself and the lambdas passed to it: all of those will be inlined into the call site.

Let's look on one more example. Let's create file with one function:
```
fun doubleAction(
  action1: () -> Unit,
  action2: () -> Unit
) {
  action1()
  action2()
}
```

and after this, let's create class `InlineFunctionExample` that will be use this function: 
```
class InlineFunctionExample {
  fun run() {
    doubleAction(
      { println("First function") },
      { println("Second function") }
    )
  }
}
```

After decompile code of class `InlineFunctionExample` we will see next: 
```
public final class InlineFunctionExample {
   public final void run() {
      InlineFunctionKt.doubleAction((Function0)null.INSTANCE, (Function0)null.INSTANCE);
   }
}
```

Sometimes kotlin decompiler works strange (this code will not create `NullPointerException`), but we should note for ourself that our two lambdas are now instances of `Function0` object.

After adding `inline` keyword to `doubleAction` function:

```
inline fun doubleAction(
  action1: () -> Unit,
  action2: () -> Unit
) {
  action1()
  action2()
}
```

we will get next decompiled code:
```
public final class InlineFunctionKt {
   public static final void doubleAction(@NotNull Function0 action1, @NotNull Function0 action2) {
      int $i$f$doubleAction = 0;
      Intrinsics.checkNotNullParameter(action1, "action1");
      Intrinsics.checkNotNullParameter(action2, "action2");
      action1.invoke();
      action2.invoke();
   }
}
```

as we can see there are no object creation here. 

Inlining may cause the generated code to grow; however, if we do it in a reasonable way (i.e. avoiding inlining large functions), it will pay off in performance, especially at "megamorphic" call-sites inside loops.

## Inline properties

The `inline` modifier can be used on accessors of properties that don't have a backing field. You can annotate individual property accessors:

```
val foo: Foo
    inline get() = Foo()

var bar: Bar
    get() = ...
    inline set(v) { ... }
```
    
You can also annotate an entire property, which marks both of its accessors as inline:

```
inline var bar: Bar
    get() = ...
    set(v) { ... }
```

At the call site, inline accessors are inlined as regular inline functions.

Let's look on one more example. Let's create file with one property:
```
var complexProperty: Int
  get() {
    val x = 2
    val y = 4
    return x + y
  }
  set(value) {
    val adjustedValue = value + 10
    println("Setting adjusted value $adjustedValue")
  }
```

And the class `InlinePropertyExample` that will be use `complexProperty`:
```
class InlinePropertyExample {
  fun run() {
    complexProperty = 4
    val calculatedProperty = complexProperty
    println("The property is $calculatedProperty")
  }
}
```

after decompiling it, we will see next:
```
public final class InlinePropertyExample {
   public final void run() {
      InlineFunctionKt.setComplexProperty(4);
      int calculatedProperty = InlineFunctionKt.getComplexProperty();
      String var2 = "The property is " + calculatedProperty;
      System.out.println(var2);
   }
}
```

Let's add `inline` keyword to `compleProperty`:
```
inline var complexProperty: Int
  get() {
    val x = 2
    val y = 4
    return x + y
  }
  set(value) {
    val adjustedValue = value + 10
    println("Setting adjusted value $adjustedValue")
  }
```

we will get next decompiled code:
```
public final class InlinePropertyExample {
   public final void run() {
      //Setter  
      int value$iv = 4;
      int adjustedValue$iv = value$iv + 10;
      String var4 = "Setting adjusted value " + adjustedValue$iv;
      System.out.println(var4);
      
      //Getter
      int x$iv = 2;
      int y$iv = 4;
      int calculatedProperty = x$iv + y$iv;
      String var6 = "The property is " + calculatedProperty;
      System.out.println(var6);
   }
}
```

# Links
[Inline functions](https://kotlinlang.org/docs/inline-functions.html)

[A Practical Guide to Kotlin's inline Modifier](https://maxkim.eu/a-practical-guide-to-kotlins-inline-modifier)

# Futher reading
[Inline function : Kotlin](https://agrawalsuneet.medium.com/inline-function-kotlin-3f05d2ea1b59)

[Kotlin Inline Functions](https://www.geeksforgeeks.org/kotlin-inline-functions/)
