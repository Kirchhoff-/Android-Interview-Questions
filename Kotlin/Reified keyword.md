# Reified keyword
`reified` keyword allows get type of generic variable in inline function. 

Sometimes we need to access a type passed to us as a parameter:
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

Here, we walk up a tree and use reflection to check if a node has a certain type. It’s all fine, but the call site is not very pretty:

`treeNode.findParentOfType(MyTreeNode::class.java)`

What we actually want is simply pass a type to this function, i.e. call it like this:

`treeNode.findParentOfType<MyTreeNode>()`

To enable this, inline functions support *reified type* parameters, so we can write something like this:
```
inline fun <reified T> TreeNode.findParentOfType(): T? {
    var p = parent
    while (p != null && p !is T) {
        p = p.parent
    }
    return p as T?
}
```

We qualified the type parameter with the `reified` modifier, now it’s accessible inside the function, almost as if it were a normal class. Since the function is inlined, no reflection is needed, normal operators like `!is` and `as` are working now. Also, we can call it as mentioned above: `myTree.findParentOfType<MyTreeNodeType>()`.

Though reflection may not be needed in many cases, we can still use it with a reified type parameter:
```
inline fun <reified T> membersOf() = T::class.members

fun main(s: Array<String>) {
    println(membersOf<StringBuilder>().joinToString("\n"))
}
```

Normal functions (not marked as inline) cannot have reified parameters. A type that does not have a run-time representation (e.g. a non-reified type parameter or a fictitious type like `Nothing`) cannot be used as an argument for a reified type parameter.

## Links
https://kotlinlang.org/docs/reference/inline-functions.html  
https://proandroiddev.com/how-reified-type-makes-kotlin-so-much-better-7ae539ed0304  
https://kotlinexpertise.com/kotlin-reified-types/  
https://medium.com/kotlin-thursdays/introduction-to-kotlin-generics-reified-generic-parameters-7643f53ba513  
