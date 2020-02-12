# Difference between == and ===

In Kotlin there are two types of equality - referential and structural

## === (Referential Equality)
For referential equality, we use the `===` symbol which allows us to evaluate the reference of an object,  it will only be true if both the objects or variables pointing to the same object. This is an equivalent of `==` operator in Java.

## == (Structural Equality)
For structural equality `==` operator is used. It will be true if data of both objects or variables a equal. This is an equivalent of `equals()` method in Java.

## Links
https://kotlinlang.org/docs/reference/equality.html  
https://www.baeldung.com/kotlin-equality-operators  
https://medium.com/@agrawalsuneet/equality-in-kotlin-and-equals-d8373ef529f1
