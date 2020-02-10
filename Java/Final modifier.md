The `final` is a modifier in Java, which can be applied to a variable, a method or a class.

- Apply this modifier to a class-level object, it means that the class can never have subclasses. This is one way to protect your class from being subclassed and often sensitive classes are made final due to security reason.
- Apply this modifier to a method, the method can never be overridden, which means you cannot override the logic of the method in the subclass. This is done to protect the original logic of method.
- Apply this modifier to a variable, the value of the variable remains constant value cannot be changed once assigned.
