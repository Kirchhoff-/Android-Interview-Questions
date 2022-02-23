# Dependency injection vs Service locator
**Dependency injection** is a technique in which an object receives other objects that it depends on, called dependencies. Typically, the receiving object is called a client and the passed-in ('injected') object is called a service. The code that passes the service to the client is called the injector. Instead of the client specifying which service it will use, the injector tells the client what service to use. The 'injection' refers to the passing of a dependency (a service) into the client that uses it.

The **service locator** pattern is a design pattern used to encapsulate the processes involved in obtaining a service with a strong abstraction layer. This pattern uses a central registry known as the "service locator", which on request returns the information necessary to perform a certain task.

Both Dependency Injection and the Service Locator pattern are implementations of the Inversion of Control concept.

Main difference between **DI** and **Service locator**:
- In **DI** dependencies located outside the class that is used them, in other words, class is not responded of creating it's dependencies. It neither knows, nor cares where they come from. With this approach is much easer to test such class, cause it is easy to mock all required dependencies. 

- In **Service locator** class **pulls** all required dependencies from some storage. With this approach, during the testing, we have to go to the class to see all it's dependencies, and only after that, we can mock the storage in which dependencies are located. 

# Links 
[Dependency injection](https://en.wikipedia.org/wiki/Dependency_injection)

[Service locator pattern](https://en.wikipedia.org/wiki/Service_locator_pattern)

# Next questions
[SOLID Principles](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/SOLID%20principles.md)

# Further reading
[Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html)

[Dependency Injection or Service Locator](https://medium.com/mobile-app-development-publication/dependency-injection-and-service-locator-4dbe4559a3ba)