# Ovverriding vs Overloading

**Overloading** - occurs when two or more methods in one class have the same method name but different parameters.
**Overriding** - means having two methods with the same method name and parameters (i.e., method signature). One of the methods is in the parent class and the other is in the child class. Overriding allows a child class to provide a specific implementation of a method that is already provided its parent class.

- `Overloading` happens at compile-time while `Overriding` happens at runtime: The binding of overloaded method call to its definition has happens at compile-time however binding of overridden method call to its definition happens at runtime.
- Static methods can be `overloaded` which means a class can have more than one static method of same name. Static methods cannot be `overridden`, even if you declare a same static method in child class it has nothing to do with the same method of parent class.
- The most basic difference is that `overloading` is being done in the same class while for `overriding` base and child classes are required. `Overriding` is all about giving a specific implementation to the inherited method of parent class.
- Static binding is being used for `overloaded` methods and dynamic binding is being used for `overridden/overriding` methods.
- Performance: `Overloading` gives better performance compared to overriding. The reason is that the binding of `overridden` methods is being done at runtime.
- Private and final methods can be `overloaded` but they cannot be `overridden`. It means a class can have more than one private/final methods of same name but a child class cannot `override` the private/final methods of their base class.
- Return type of method does not matter in case of method `overloading`, it can be same or different. However, in case of method `overriding` the `overriding` method can have more specific return type.
- Argument list should be different while doing method `overloading`. Argument list should be same in method `overriding`.

