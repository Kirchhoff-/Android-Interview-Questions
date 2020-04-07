# Inline keyword

## Inline functions 

Using higher-order functions imposes certain runtime penalties: each function is an object, and it captures a closure, i.e. those variables that are accessed in the body of the function. Memory allocations (both for function objects and classes) and virtual calls introduce runtime overhead.

But it appears that in many cases this kind of overhead can be eliminated by inlining the lambda expressions. Inlining is basically requesting the compiler to copy the (inlined) code at the calling place.

Consider the following case:

`lock(l) { foo() }`

Instead of creating a function object for the parameter and generating a call, the compiler could emit the following code:

`l.lock()
try {
    foo()
}
finally {
    l.unlock()
}`

To make the compiler do this, we need to mark the lock() function with the inline modifier:

`inline fun <T> lock(lock: Lock, body: () -> T): T { ... }`

The `inline` modifier affects both the function itself and the lambdas passed to it: all of those will be inlined into the call site.

Inlining may cause the generated code to grow; however, if we do it in a reasonable way (i.e. avoiding inlining large functions), it will pay off in performance, especially at "megamorphic" call-sites inside loops.

## Inline properties

The `inline` modifier can be used on accessors of properties that don't have a backing field. You can annotate individual property accessors:

`val foo: Foo
    inline get() = Foo()

var bar: Bar
    get() = ...
    inline set(v) { ... }`
    
You can also annotate an entire property, which marks both of its accessors as inline:

`inline var bar: Bar
    get() = ...
    set(v) { ... }`
    
At the call site, inline accessors are inlined as regular inline functions.

## Links
https://kotlinlang.org/docs/reference/inline-functions.html    
https://medium.com/@agrawalsuneet/inline-function-kotlin-3f05d2ea1b59    
https://www.geeksforgeeks.org/kotlin-inline-functions/    
