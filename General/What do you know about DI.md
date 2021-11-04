# What is Dependency injection
**Dependency injection** is a technique in which an object receives other objects that it depends on, called dependencies. Typically, the receiving object is called a client and the passed-in ('injected') object is called a service. The code that passes the service to the client is called the injector. Instead of the client specifying which service it will use, the injector tells the client what service to use. The 'injection' refers to the passing of a dependency (a service) into the client that uses it.

The service is made part of the client's state. Passing the service to the client, rather than allowing the client to build or find the service, is the fundamental requirement of the pattern.

The intent behind dependency injection is to achieve separation of concerns of construction and use of objects. This can increase readability and code reuse.

Dependency injection is one form of the broader technique of inversion of control. A client who wants to call some services should not have to know how to construct those services. Instead, the client delegates to external code (the injector). The client is not aware of the injector. The injector passes the services, which might exist or be constructed by the injector itself, to the client. The client then uses the services.

This means the client does not need to know about the injector, how to construct the services, or even which services it is actually using. The client only needs to know the interfaces of the services, because these define how the client may use the services. This separates the responsibility of 'use' from the responsibility of 'construction'.

## Overview
Dependency injection solves the following problems:
- How can a class be independent of how the objects on which it depends are created?
- How can the way objects are created be specified in separate configuration files?
- How can an application support different configurations?

Creating objects directly within the class commits the class to particular implementations. This makes it difficult to change the instantiation at runtime, especially in compiled languages where changing the underlying objects can require re-compiling the source code.

Dependency injection separates the creation of a client's dependencies from the client's behavior, which promotes loosely coupled programs and the dependency inversion and single responsibility principles. Fundamentally, dependency injection is based on passing parameters to a method.

Dependency injection is an example of the more general concept of inversion of control.

An example of inversion of control without dependency injection is the template method pattern, where polymorphism is achieved through subclassing. Dependency injection implements inversion of control through composition, and is often similar to the strategy pattern. While the strategy pattern is intended for dependencies that are interchangeable throughout an object's lifetime, in dependency injection it may be that only a single instance of a dependency is used. This still achieves polymorphism, but through delegation and composition.

Dependency injection directly contrasts with the service locator pattern, which allows clients to know about the system they use to find dependencies.

## Types of dependency injection
There are three ways a client can receive a reference to an external module:
- **Constructor injection**: In the constructor injection, the injector supplies the service (dependency) through the client class constructor;
- **Property Injection**: In the property injection (aka the Setter Injection), the injector supplies the dependency through a public property of the client class;
- **Method Injection**: In this type of injection, the client class implements an interface which declares the method(s) to supply the dependency and the injector uses this interface to supply the dependency to the client class.

### Examples

In all examples we will have two classes - `User` and `Service`. `User` will get injection and `Service` will be injected. 

`Service` class:
```
class Service {
    fun doWork() {
        //Do some work here
    }
}
```

#### Constructor injection example
```
class User(private val service: Service) {

    fun start() {
        service.doWork()
    }
}

fun main(args: Array) {
    val service = Service()
    val user = User(service)
    user.start()
}
```

#### Property injection example
```
class User {
    lateinit var service: Service

    fun start() {
        service.doWork()
    }
}

fun main(args: Array) {
    val service = Service()
    val user = User()
    user.service = service
    user.start()
}
```

#### Method injection example
```
class User {

    fun start(service: Service) {
        service.doWork()
    }
}

fun main(args: Array) {
    val service = Service()
    val user = User()
    user.start(service)
}
```

## Conclusion

### Advantages
There are several benefits from using dependency injection containers rather than having components satisfy their own dependencies. Some of these benefits are:
- Reduced Dependencies;
- Reduced Dependency Carrying;
- More Reusable Code;
- More Testable Code;
- More Readable Code.

### Disadvantages
Criticisms of dependency injection argue that it:
- Creates clients that demand configuration details, which can be onerous when obvious defaults are available;
- Make code difficult to trace because it separates behavior from construction;
- Is typically implemented with reflection or dynamic programming. This can hinder IDE automation;
- Typically requires more upfront development effort;
- Forces complexity out of classes and into the links between classes which might be harder to manage;
- Encourage dependence on a framework.

# Links
[Dependency injection](https://en.wikipedia.org/wiki/Dependency_injection)

[Dependency Injection](https://www.tutorialsteacher.com/ioc/dependency-injection)

[Dependency Injection Benefits](http://tutorials.jenkov.com/dependency-injection/dependency-injection-benefits.html)


# Next questions
[Template method pattern](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Patterns/Template%20method%20pattern.md)

[Strategy pattern](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Patterns/Strategy%20pattern.md)

# Futher reading
[Dependency Injection](http://tutorials.jenkov.com/dependency-injection/index.html)

[Dependency injection in Android](https://developer.android.com/training/dependency-injection)
