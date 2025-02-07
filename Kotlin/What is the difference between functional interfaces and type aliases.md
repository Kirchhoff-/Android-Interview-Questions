# Functional interfaces vs. type aliases
An interface with only one abstract method is called a **functional interface**, or a **Single Abstract Method (SAM) interface**. The functional interface can have several non-abstract members but only one abstract member.

To declare a functional interface in Kotlin, use the `fun` modifier.<sup>[1](https://kotlinlang.org/docs/fun-interfaces.html#:~:text=An%20interface%20with,the%20fun%20modifier.)</sup>

```
fun interface KRunnable {
   fun invoke()
}
```

Type aliases provide alternative names for existing types. If the type name is too long you can introduce a different shorter name and use the new one instead.

It's useful to shorten long generic types. For instance, it's often tempting to shrink collection types:
```
typealias NodeSet = Set<Network.Node>

typealias FileTable<K> = MutableMap<K, MutableList<File>>
```

You can provide different aliases for function types<sup>[2](https://kotlinlang.org/docs/type-aliases.html#:~:text=Type%20aliases%20provide,\)%20%2D%3E%20Boolean)</sup>:
```
typealias MyHandler = (Int, String, Any) -> Unit

typealias Predicate<T> = (T) -> Boolean
```

Functional interfaces and type aliases serve different purposes. Type aliases are just names for existing types – they don't create a new type, while functional interfaces do. You can provide extensions that are specific to a particular functional interface to be inapplicable for plain functions or their type aliases.

Type aliases can have only one member, while functional interfaces can have multiple non-abstract member functions and one abstract member function. Functional interfaces can also implement and extend other interfaces.

Functional interfaces are more flexible and provide more capabilities than type aliases, but they can be more costly both syntactically and at runtime because they can require conversions to a specific interface. When you choose which one to use in your code, consider your needs:
- If your API needs to accept a function (any function) with some specific parameter and return types – use a simple functional type or define a type alias to give a shorter name to the corresponding functional type;
- If your API accepts a more complex entity than a function – for example, it has non-trivial contracts and/or operations on it that can't be expressed in a functional type's signature – declare a separate functional interface for it.<sup>[3](https://kotlinlang.org/docs/fun-interfaces.html#functional-interfaces-vs-type-aliases:~:text=functional%20interfaces%20and,interface%20for%20it.)</sup>

# Links
[Functional (SAM) interfaces﻿](https://kotlinlang.org/docs/fun-interfaces.html)

[Type aliases﻿](https://kotlinlang.org/docs/type-aliases.html)

# Next questions
[What do you know about functional interfaces?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20functional%20interfaces.md)

[What do you know about type aliases?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20type%20aliases.md)
