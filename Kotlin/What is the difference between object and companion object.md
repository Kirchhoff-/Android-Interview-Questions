# `object` vs `companion object`

The `object` keyword is used to create a singleton object in Kotlin. It can also be used for object expressions and anonymous objects.

`companion object` is an object associated with a class rather than existing independently. It is commonly used for factory methods, constants, and functionality that conceptually belongs to the class itself.

| Aspect                                         | `object`                                            | `companion object`                                   |
| ---------------------------------------------- | --------------------------------------------------- | ---------------------------------------------------- |
| Scope                                          | Exists independently                                | Belongs to a specific class                          |
| Access                                         | Accessed directly by name                           | Accessed through the containing class                |
| Relationship to class                          | Not tied to any class                               | Associated with a class                              |
| Common use cases                               | Managers, registries, shared state, utility objects | Factory methods, constants, static-like APIs         |
| Can access private members of containing class | No                                                  | Yes                                                  |
| Can be declared at top level                   | Yes                                                 | No                                                   |
| Java interoperability                          | Accessed as a singleton instance                    | Can expose static methods via `@JvmStatic`           |
| Naming                                         | Named object                                        | Companion can be unnamed or named                    |
| Initialization                                 | Created on first access                             | Created when the containing class is loaded/accessed |
| Reflection                                     | Has its own Kotlin class                            | Appears as a member of the containing class          |
| Multiple declarations                          | Many `object` declarations can exist in a project   | Only one `companion object` per class                |
| Anonymous usage                                | Can be used for object expressions                  | Cannot be used for object expressions                |
| Example                                        | `object DateHelper`                                 | `User.create()` inside a `companion object`          |


## Related questions
[What do you know about object keyword?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20object%20keyword.md)

[What do you know about companion object?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Kotlin/What%20do%20you%20know%20about%20companion%20object.md)

# Further reading
[Kotlin-Difference b/w Object V/S Companion Object](https://medium.com/@guruprasadhegde4/kotlin-difference-b-w-object-v-s-companion-object-72044e72dd3e)
