# Data classes

We frequently create classes whose main purpose is to hold data. In such a class some standard functionality and utility functions are often mechanically derivable from the data. In Kotlin, this is called a data class and is marked as `data:`

```
data class User(val name: String, val age: Int)
```

The compiler automatically derives the following members from all properties declared in the primary constructor:
- `equals()`/`hashCode()` pair;
- `toString()` of the form `User(name=John, age=42)`;
- componentN() functions corresponding to the properties in their order of declaration;
- `copy()` function.

To ensure consistency and meaningful behavior of the generated code, data classes have to fulfill the following requirements:
- The primary constructor needs to have at least one parameter;
- All primary constructor parameters need to be marked as `val` or `var`;
- Data classes cannot be abstract, open, sealed or inner;

Additionally, the members generation follows these rules with regard to the members inheritance:

- If there are explicit implementations of `equals()`, `hashCode()` or `toString()` in the data class body or final implementations in a superclass, then these functions are not generated, and the existing implementations are used;
- If a supertype has the `componentN()` functions that are open and return compatible types, the corresponding functions are generated for the data class and override those of the supertype. If the functions of the supertype cannot be overridden due to incompatible signatures or being final, an error is reported;
- Deriving a data class from a type that already has a `copy(...)` function with a matching signature is deprecated in Kotlin 1.2 and is prohibited in Kotlin 1.3.
- Providing explicit implementations for the `componentN()` and `copy()` functions is not allowed.

On the JVM, if the generated class needs to have a parameterless constructor, default values for all properties have to be specified:

```
data class User(val name: String = "", val age: Int = 0)
```

## Links
https://kotlinlang.org/docs/reference/data-classes.html
