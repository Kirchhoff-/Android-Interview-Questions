# References types

There are four type of references in java:
- Strong
- Weak
- Soft
- Phantom

## Strong reference
Strong reference are the ordinary references in Java. Anytime we create a new object, a strong reference is by default created. Any object which has an active strong reference are not eligible for garbage collection. The object is garbage collected only when the variable which was strongly referenced points to null.

`MyClass obj = new MyClass();` 

## Weak  reference

Weak reference is a reference not strong enough to keep the object in memory. The object is garbage collected whenever a weak reference is found, the object will be recycled regardless of whether the system heap space is sufficient.

`WeakReference<MyClass> obj = new MyClass();` 

## Soft reference

Think of a SoftReference as a stronger WeakReference. An object holding a SoftReference won't be reclaimed quickly by the JVM. JVM will determine when to recycle based on the current heap usage. Only when the heap usage is near the threshold, the SoftReferenced object will be collected. Therefore, SoftReference can be used to implement a memory-sensitive cache.

`SoftReference<MyClass> obj = new MyClass();` 

## Phantom reference

PhantomReference is the weakest of all types. An object holding a PhantomReference is almost identical to having no reference and can be recycled by the garbage collector at any time. But, before removing them from the memory, JVM puts them in a queue called ‘reference queue’ . They are put in a reference queue after calling `finalize()` method on them.

`PhantomReference<MyClass> obj = new MyClass();`

## Links
https://docs.oracle.com/javase/8/docs/api/java/lang/ref/package-summary.html  
https://medium.com/google-developer-experts/finally-understanding-how-references-work-in-android-and-java-26a0d9c92f83   
https://www.tutorialdocs.com/article/java-4-referance-types.html  
