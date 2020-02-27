# int vs Integer

`int` is a primitive type. Variables of type `int` store the actual binary value for the integer you want to represent. Integer is a class, no different from any other in the Java language. Variables of type `Integer` store references to Integer objects, just as with any other reference (object) type. `Integer.parseInt("1")` is a call to the static method `parseInt` from class `Integer`. To be more specific, `Integer` is a class with a single field of type int. This class is used where you need an int to be treated like any other object, such as in generic types or situations where you need nullability. The automatic conversion of primitive data types into its equivalent Wrapper type is known as autoboxing and opposite operation is known as unboxing.

## Autoboxing: 
Converting a primitive value into an object of the corresponding wrapper class is called autoboxing. For example, converting int to Integer class. The Java compiler applies autoboxing when a primitive value is:
- Passed as a parameter to a method that expects an object of the corresponding wrapper class.
- Assigned to a variable of the corresponding wrapper class.

## Unboxing: 
Converting an object of a wrapper type to its corresponding primitive value is called unboxing. For example conversion of Integer to int. The Java compiler applies unboxing when an object of a wrapper class is:
- Passed as a parameter to a method that expects a value of the corresponding primitive type.
- Assigned to a variable of the corresponding primitive type.

## Links
https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html
http://illegalargumentexception.blogspot.com/2008/08/java-int-versus-integer.html
https://www.geeksforgeeks.org/autoboxing-unboxing-java/
