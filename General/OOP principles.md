# OOP Principles

Some sources claim that there are 3 main principles of object-oriented programming - **Inheritance**, **Polymorphism** and **Encapsulation**. Other sources added one more  principle - **Abstraction**. In this answer we will consider all these principles. From my point of view, **Abstraction** is not a principle of object-oriented programming.

## Inheritance
Inheritance is a mechanism that lets you describe a new class based on an existing (parent or super) class. In doing so, the new class borrows the properties and functionality of the parent class. Inheritance supports the concept of hierarchical classification, this allows classes to be arranged in a hierarchy that represents "is-a-type-of" relationships.

Important terminology:
- *Super Class*: The class whose features are inherited is known as superclass(or a base class or a parent class).
- *Sub Class*: The class that inherits the other class is known as subclass(or a derived class, extended class, or child class). The subclass can add its own fields and methods in addition to the superclass fields and methods.
- *Reusability*: Inheritance supports the concept of “reusability”, i.e. when we want to create a new class and there is already a class that includes some of the code that we want, we can derive our new class from the existing class. By doing this, we are reusing the fields and methods of the existing class.

Subclasses can override the methods defined by superclasses. Multiple inheritance is allowed in some languages, though this can make resolving overrides complicated. Some languages have special support for mixins, though in any language with multiple inheritance, a mixin is simply a class that does not represent an is-a-type-of relationship.

A real-world example of inheritance is a mother and child. The child may inherit attributes such as height, Voice patters, color. The mother can reproduce other children with the same attributes as well. 

In summary, **Inheritance** is concerned with the relationship between classes and method, which is like a parent and a child. A child can be born with some of the attributes of the parents. Inheritance ensures reusability of codes just the way multiple children can inherit the attributes of their parents.

## Polymorphism
Polymorphism is the ability to work with several types as if they were the same type. Moreover, the objects' behavior will be different depending on their type. 

An excellent example of Polymorphism in Object-oriented programing is a cursor behavior. A cursor may take different forms like an arrow, a line, cross, or other shapes depending on the behavior of the user or the program mode. With polymorphism, a method or subclass can define its behaviors and attributes while retaining some of the functionality of its parent class. This means you can have a class that displays date and time, and then you can create a method to inherit the class but should display a welcome message alongside the date and time. The goals of Polymorphism in Object-oriented programming is to enforce simplicity, making codes more extendable and easily maintaining applications.

Inheritance allows you to create class hierarchies, where a base class gives its behavior and attributes to a derived class. You are then free to modify or extend its functionality. Polymorphism ensures that the proper method will be executed based on the calling object’s type. 

Polymorphism could be static and dynamic both. Method Overloading is static polymorphism while, Method overriding is dynamic polymorphism.
- Overloading in simple words means more than one method having the same method name that behaves differently based on the arguments passed while calling the method. This called static because, which method to be invoked is decided at the time of compilation
- Overriding means a derived class is implementing a method of its super class. The call to overriden method is resolved at runtime, thus called runtime polymorphism

## Encapsulation

Encapsulation is defined as the wrapping up of data under a single unit. It is the mechanism that binds together code and the data it manipulates. Other way to think about encapsulation is, it is a protective shield that prevents the data from being accessed by the code outside this shield. Attributes and behaviors are defined by code inside the class template. Then, when an object is instantiated from the class, the data and methods are encapsulated in that object.

- Technically in encapsulation, the variables or data of a class is hidden from any other class and can be accessed only through any member function of own class in which they are declared.
- As in encapsulation, the data in a class is hidden from other classes using the data hiding concept which is achieved by making the members or methods of class as private and the class is exposed to the end user or the world without providing any details behind implementation using the abstraction concept, so it is also known as combination of data-hiding and abstraction.
- Encapsulation can be achieved by: Declaring all the variables in the class as private and writing public methods in the class to set and get the values of variables.

Encapsulation adds security. Attributes and methods can be set to private, so they can’t be accessed outside the class. To get information about data in an object, public methods & properties are used to access or update data. This adds a layer of security, where the developer chooses what data can be seen on an object by exposing that data through public methods in the class definition.

Benefits of encapsulation:
- Adds security
- Protects developers from common mistakes
- Protects IP
- Supportable
- Developer can change internal code in the class without alerting users
- Hides complexity

## Abstraction
Abstraction means that the user interacts with only selected attributes and methods of an object. Abstraction uses simplified, high level tools, to access a complex object.

Data Abstraction may also be defined as the process of identifying only the required characteristics of an object ignoring the irrelevant details.The properties and behaviors of an object differentiate it from other objects of similar type and also help in classifying/grouping the objects.

Consider a real-life example of a man driving a car. The man only knows that pressing the accelerators will increase the speed of car or applying brakes will stop the car but he does not know about how on pressing the accelerator the speed is actually increasing, he does not know about the inner mechanism of the car or the implementation of accelerator, brakes etc in the car. This is what abstraction is.

Benefits of abstraction:
- Simple, high level user interfaces
- Complex code is hidden
- Security. Only chosen parts of object are accessible
- Easier software maintenance. Code updates rarely change abstraction

## Links
https://en.wikipedia.org/wiki/Object-oriented_programming  
https://codegym.cc/groups/posts/155-principles-of-object-oriented-programming  
https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/  
https://docs.oracle.com/javase/tutorial/java/concepts/index.html  
https://beginnersbook.com/2013/03/oops-in-java-encapsulation-inheritance-polymorphism-abstraction/  
https://www.nerd.vision/post/polymorphism-encapsulation-data-abstraction-and-inheritance-in-object-oriented-programming  
https://www.geeksforgeeks.org/encapsulation-in-java/  
https://www.geeksforgeeks.org/abstraction-in-java-2/
