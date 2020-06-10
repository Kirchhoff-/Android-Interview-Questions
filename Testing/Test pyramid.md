# Test pyramid

A test pyramid is a pyramid of where all the different types of tests fits. Mike Cohn came up with this concept in his book *Succeeding with Agile*. It's a great visual metaphor telling you to think about different layers of testing. It also tells you how much testing to do on each layer. Agile testing relies more on automation. It requires a much greater contribution from developers. And it has a different basic philosophy – to prevent bugs.

![](./res/test_pyramid.png "test_pyramid")

Original test pyramid consists of three layers:
- Unit Test
- Service/Integration Test
- UI/GUI Test

**Unit Test** 

Unit testing is a software testing method by which individual units of source code, sets of one or more computer program modules together with associated control data, usage procedures, and operating procedures, are tested to determine whether they are fit for use. In object-oriented programming, a unit is often an entire interface, such as a class, but could be an individual method. By writing tests first for the smallest testable units, then the compound behaviors between those, one can build up comprehensive tests for complex applications.

The goal of unit testing is to isolate each part of the program and show that the individual parts are correct. A unit test provides a strict, written contract that the piece of code must satisfy. Unit testing allows the programmer to refactor code or upgrade system libraries at a later date, and make sure the module still works correctly (e.g., in regression testing). The procedure is to write test cases for all functions and methods so that whenever a change causes a fault, it can be quickly identified. Unit tests detect changes which may break a design contract.

**Integration Test**

Integration testing is the phase in software testing  in which individual software modules are combined and tested as a group. Integration testing is conducted to evaluate the compliance of a system or component with specified functional requirements. It occurs after unit testing and before validation testing. Integration testing takes as its input modules that have been unit tested, groups them in larger aggregates, applies tests defined in an integration test plan to those aggregates, and delivers as its output the integrated system ready for system testing.

**GUI Test**

GUI testing is a software testing type that checks the Graphical User Interface of the Application Under Test. GUI testing involves checking the screens with the controls like menus, buttons, icons, and all types of bars - toolbar, menu bar, dialog boxes, and windows, etc. The purpose of Graphical User Interface (GUI) Testing is to ensure UI functionality works as per the specification.

## Links
https://martinfowler.com/articles/practical-test-pyramid.html  
https://medium.com/@Colin_But/define-testing-strategy-using-the-testing-pyramid-1dabee37e823  
https://jamescrisp.org/2011/05/30/automated-testing-and-the-test-pyramid/  
https://www.agilecoachjournal.com/2014-01-28/the-agile-testing-pyramid  
https://en.wikipedia.org/wiki/Integration_testing  
https://en.wikipedia.org/wiki/Unit_testing  
https://www.guru99.com/gui-testing.html  
