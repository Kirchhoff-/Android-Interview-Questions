# `reified` keyword

The keyword `reified` enables access to information about type at runtime in inline functions. This allows for more flexibility and type-safe operations at runtime and improves the code's readability. 

In Java, generic types suffer from type erasure, which means that the information about actual types used in the generic class is not available at runtime. This limitation restricts the ability to perform certain operations like checking the type of a generic parameter at runtime, leading to the need for explicit casting and potential runtime errors.<sup>[1](https://javanexus.com/blog/kotlin-reified-type-parameters#:~:text=In%20Java%2C%20generic,potential%20runtime%20errors.)</sup>

Kotlin introduces the `reified` modifier to address the limitations imposed by type erasure. When a type parameter is marked as `reified`, it allows us to access the actual type at runtime within inline functions. This effectively provides the ability to perform type checks, type casts, and create instances of the type parameter at runtime.

By using `reified` type parameters, we can write more concise and type-safe code within inline functions, eliminating the need for explicit casts and improving the overall reliability of our code.<sup>[2](https://javanexus.com/blog/kotlin-reified-type-parameters#:~:text=Kotlin%20introduces%20the,of%20our%20code.)</sup>

Let's take a look at an example:
```
fun <T> TreeNode.findParentOfType(clazz: Class<T>): T? {
    var p = parent
    while (p != null && !clazz.isInstance(p)) {
        p = p.parent
    }
    @Suppress("UNCHECKED_CAST")
    return p as T?
}
```

Here, you walk up a tree and use reflection to check whether a node has a certain type. It's all fine, but the call site is not very pretty:
```
treeNode.findParentOfType(MyTreeNode::class.java)
```

A better solution would be to simply pass a type to this function. You can call it as follows:
```
treeNode.findParentOfType<MyTreeNode>()
```

To enable this, inline functions support **reified type parameters**, so you can write something like this:
```
inline fun <reified T> TreeNode.findParentOfType(): T? {
    var p = parent
    while (p != null && p !is T) {
        p = p.parent
    }
    return p as T?
}
```

The code above qualifies the type parameter with the `reified` modifier to make it accessible inside the function, almost as if it were a normal class. Since the function is inlined, no reflection is needed and normal operators like `!is` and `as` are now available for you to use. Also, you can call the function as shown above: `myTree.findParentOfType<MyTreeNodeType>()`.<sup>[3](https://kotlinlang.org/docs/inline-functions.html#reified-type-parameters:~:text=fun%20%3CT%3E%20TreeNode,above%3A%20myTree.findParentOfType%3CMyTreeNodeType%3E().)</sup>

Real word example is navigation in Android. In Java, when we call `startActivity`, we need to specify the destination class as a parameter. In Kotlin, we can simplify it by adding the type to the function:
```
inline fun <reified T : Activity> Activity.startActivity() {
    startActivity(Intent(this, T::class.java))
}
```

Navigating to an Activity is now as easy as this:<sup>[4](https://antonioleiva.com/reified-types-kotlin#:~:text=In%20Java%2C%20when,%3CDetailActivity%3E())</sup>
```
startActivity<DetailActivity>()
```

## [Benefits of Reified Type Parameters](https://javanexus.com/blog/kotlin-reified-type-parameters#:~:text=Benefits%20of%20Reified%20Type%20Parameters)
The use of reified type parameters brings several benefits to Kotlin developers:
- **Type-safe Operations**. Reified type parameters enable type-safe operations within inline functions, eliminating the need for explicit casts and reducing the likelihood of runtime errors;
- **Improved Code Readability**. By leveraging reified type parameters, the code becomes more concise and readable as the type information is available at runtime, simplifying conditional logic and type-based operations;
- **Enhanced API Design**. APIs utilizing reified type parameters can offer more intuitive and streamlined interfaces, as the type information can be leveraged directly within the function body.

## [Limitations and Considerations](https://javanexus.com/blog/kotlin-reified-type-parameters#:~:text=the%20function%20body.-,Limitations%20and%20Considerations,-While%20reified%20type)
While reified type parameters provide significant advantages, there are some limitations and considerations to keep in mind:
- **Only for Inline Functions**. Reified type parameters can only be used within inline functions, which restricts their applicability to certain contexts;
- **Memory Overhead**. Using reified type parameters within inline functions may lead to increased memory overhead, as the bytecode of the inline function is duplicated at each call site;
- **Reflective Operations**. While reified type parameters allow for type checks and casts, they do not support general reflective operations, such as obtaining class references or working with reflection APIs.

# Links
[Understanding Kotlin's Reified Type Parameters](https://javanexus.com/blog/kotlin-reified-type-parameters)

[Inline functions﻿](https://kotlinlang.org/docs/inline-functions.html)

[Reified Types in Kotlin: how to use the type within a function (KAD 14)](https://antonioleiva.com/reified-types-kotlin)

# Further reading
[Kotlin Generics: Reified Type Parameter with Inline Function](https://www.dhiwise.com/post/kotlin-generics-with-inline-function)

[How Reified Type makes Kotlin so much better](https://medium.com/mobile-app-development-publication/how-reified-type-makes-kotlin-so-much-better-7ae539ed0304)

[Introduction to Kotlin Generics: Reified Generic Parameters](https://medium.com/kotlin-thursdays/introduction-to-kotlin-generics-reified-generic-parameters-7643f53ba513)

# Next questions
[What do you know about `inline` keyword?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/Inline%20keyword.md)
