# Lateinit

Normally, properties declared as having a non-null type must be initialized in the constructor. However, fairly often this is not convenient. For example, properties can be initialized through dependency injection, or in the setup method of a unit test. In this case, you cannot supply a non-null initializer in the constructor, but you still want to avoid null checks when referencing the property inside the body of a class.

To handle this case, you can mark the property with the `lateinit` modifier:

`lateinit var subject: TestSubject`

The modifier can be used on `var` properties declared inside the body of a class (not in the primary constructor, and only when the property does not have a custom getter or setter), for top-level properties and local variables. The type of the property or variable must be non-null, and it must not be a primitive type.

Accessing a `lateinit` property before it has been initialized throws a special exception that clearly identifies the property being accessed and the fact that it hasn't been initialized.

`if (foo::bar.isInitialized) {
    println(foo.bar)
}`

This check is only available for the properties that are lexically accessible, i.e. declared in the same type or in one of the outer types, or at top level in the same file.

## Links
https://kotlinlang.org/docs/reference/properties.html  
https://www.bignerdranch.com/blog/kotlin-when-to-use-lazy-or-lateinit/  
