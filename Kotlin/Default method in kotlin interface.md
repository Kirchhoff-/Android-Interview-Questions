


  ##  Default method in kotlin interface
Before getting into Kotlin default method in interface ,lets go through the `default`  interface mthod  in Java which was introduced in Java 8 and `@JvmDefault` annotaion in Kotlin

## `default` interface in Java
### Why default method is introduced in Java 8
 - In a typical abstraction based design where an interface has one or more implementation if any new method is to be added to the interface then all the implementations will be forced to implement it. 
 - `default`  interface method is introduced to deal with this issue which allow us to add new methods to the interface that are automatically available to implementations. In this way backward compatibility is neatly preserved without having to refactor the implementers.
 - The most typical use of `default` methods in interfaces is **to incrementally provide additional functionality to a given type without breaking down the implementing classes.**

## `@JvmDefault` annotation in Kotlin
- `@JvmDefault` is an annotation + compiler flag in Kotlin to enable using Java 8 `default` interface methods.

- `@JvmDefault` was introduced in Kotlin 1.2 which will be deprecated in nfavor of generating all the method bodies in interfaces directly when the code is compiled in a special mode.

 A basic interface in Kotlin looks like following

     interface Car {  
        fun getNoOfWheels()  
    }
The java equvalent of the above Kotlin code is as follows

    interface Car {  
        void getNoOfWheels();  
    }
Now lets add default implementation to the Kotlin code

    interface Car {  
        fun getNoOfWheels() {}  
    }
   The java equivalent code will be as following
 

      interface Car {  
        void getNoOfWheels();  
      
        final class DefaultImpls {  
            public static void getNoOfWheels(Car instance) { }  
        }  
    }
- `DefaultImpls` is a compiler-generated construct for holding default implementations.In this case - the default `getNoOfWheels()` implementation. It's implemented as a static method by the same name, return type, an `instance` parameter (`this` references in the function body are modified to refer to the `instance` parameter), and finally any other parameters.
- This is how Kotlin supports `default` methods even on Java 6 or 7.

- Now lets consider adding `@JvmDefault` by adding configuration  `-Xjvm-default=enable`

      interface Car {  
            @JvmDefault fun getNoOfWheels() {}  
        }
Now in bytecode, It will use the native  `default`  interface method support and no longer uses  `DefaultImpls`.

    public interface Car {  
        @JvmDefault default void getNoOfWheels() { }  
    }
If we use `-Xjvm-default=compatibility`, it will use both the native `default` support while also still generating the `DefaultImpls` API. This is important for backward binary compatibility.

    interface Car {  
        @JvmDefault default void getNoOfWheels() { }  
      
        final class DefaultImpls {  
            @JvmDefault public static void load(Car instance) {  
                instance.getNoOfWheels();  
            }  
        }  
    }
