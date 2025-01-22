# `data class`
A `data class` in Kotlin is a specialized class that is primarily designed to hold and represent data. It's a concise way to create classes that are focused on storing properties and providing common functionalities, without the need for boilerplate code. 

Kotlin data classes are particularly useful when you need to model simple, immutable data structures.<sup>[1](https://www.tutorialsfreak.com/kotlin-tutorial/kotlin-data-class#:~:text=A%20data%20class,immutable%20data%20structures.)</sup> For each `data class`, the compiler automatically generates additional member functions that allow you to print an instance to readable output, compare instances, copy instances, and more. Data classes are marked with `data`:<sup>[2](https://kotlinlang.org/docs/data-classes.html#:~:text=For%20each%20data%20class%2C%20the%20compiler%20automatically%20generates%20additional%20member%20functions%20that%20allow%20you%20to%20print%20an%20instance%20to%20readable%20output%2C%20compare%20instances%2C%20copy%20instances%2C%20and%20more.%20Data%20classes%20are%20marked%20with%20data%3A)</sup>
```
data class User(val name: String, val age: Int)
```

The compiler automatically derives the following members from all properties declared in the primary constructor:
- `equals()`/`hashCode()` pair;
- `toString()` of the form `"User(name=John, age=42)"`;
- `componentN()` functions corresponding to the properties in their order of declaration;
- `copy()` function.<sup>[3](https://kotlinlang.org/docs/data-classes.html#:~:text=The%20compiler%20automatically%20derives,copy()%20function%20(see%20below).)</sup> 

Data classes may extend other classes and implement the interfaces, but also there are some limitations:
- The primary constructor must have at least one parameter;
- All primary constructor parameters must be marked as `val` or `var`;
- Data classes can't be `abstract`, `open`, `sealed`, or `inner`.<sup>[4](https://kotlinlang.org/docs/data-classes.html#:~:text=The%20primary%20constructor%20must,open%2C%20sealed%2C%20or%20inner.)</sup>

## [Properties declared in the class body﻿](https://kotlinlang.org/docs/data-classes.html#properties-declared-in-the-class-body)
The compiler only uses the properties defined inside the primary constructor for the automatically generated functions. To exclude a property from the generated implementations, declare it inside the class body:
```
data class Person(val name: String) {
    var age: Int = 0
}
```

In the example below, only the `name` property is used by default inside the `toString()`, `equals()`, `hashCode()`, and `copy()` implementations, and there is only one component function, `component1()`. The `age` property is declared inside the class body and is excluded. Therefore, two `Person` objects with the same `name` but different `age` values are considered equal since `equals()` only evaluates properties from the primary constructor:
```
val person1 = Person("John")
val person2 = Person("John")
person1.age = 10
person2.age = 20

println("person1 == person2: ${person1 == person2}")
// person1 == person2: true

println("person1 with age ${person1.age}: ${person1}")
// person1 with age 10: Person(name=John)

println("person2 with age ${person2.age}: ${person2}")
// person2 with age 20: Person(name=John)
```

## [Copying﻿](https://kotlinlang.org/docs/data-classes.html#copying)
Use the `copy()` function to copy an object, allowing you to alter **some** of its properties while keeping the rest unchanged. The implementation of this function for the `User` class above would be as follows:
```
fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```

You can then write the following:
```
val jack = User(name = "Jack", age = 1)
val olderJack = jack.copy(age = 2)
```

## [Data classes and destructuring declarations﻿](https://kotlinlang.org/docs/data-classes.html#data-classes-and-destructuring-declarations)
**Component functions** generated for data classes make it possible to use them in destructuring declarations:
```
val jane = User("Jane", 35)
val (name, age) = jane
println("$name, $age years of age")
// Jane, 35 years of age
```

## [What is the Use of Data Class in Kotlin?](https://www.tutorialsfreak.com/kotlin-tutorial/kotlin-data-class#:~:text=properties%20in%20Kotlin.-,What%20is%20the%20Use%20of%20Data%20Class%20in%20Kotlin%3F,-Data%20classes%20in)
- **Reducing Boilerplate Code**. Data classes automatically generate useful methods such as `equals()`, `hashCode()`, `toString()`, and `copy()`. This eliminates the need to write repetitive and error-prone boilerplate code for these functions;
- **Immutability**. By default, properties in data classes are declared as `val` (read-only), making instances of data classes immutable. Immutability can help in preventing unintended side effects and simplifying code;
- **Structural Equality**. Data classes automatically implement structural equality (comparison of property values), making it easy to compare instances based on their content;
- **Ease of Use with Collections**. Data classes work seamlessly with collections (e.g., lists, sets, maps), making it convenient to store and manipulate data in data structures;
- **Copy Operations**. The `copy()` method allows you to create new instances based on existing ones with modified property values. This is useful for creating modified copies of objects while keeping the original intact;
- **JSON Serialization/Deserialization**. Data classes are commonly used to represent JSON data and facilitate easy serialization and deserialization using libraries like Gson, Jackson, or Moshi.

# Links
[Data classes﻿](https://kotlinlang.org/docs/data-classes.html)

[Data Class in Kotlin (Detailed Guide With Examples)](https://www.tutorialsfreak.com/kotlin-tutorial/kotlin-data-class)

# Further Reading
[Data Classes and Destructuring](https://typealias.com/start/kotlin-data-classes-and-destructuring/)

[Kotlin Data Class](https://www.digitalocean.com/community/tutorials/kotlin-data-class)

# Next Questions
[What do you know about data objects?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20data%20objects.md)
