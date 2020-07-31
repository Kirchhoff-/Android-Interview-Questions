# Unit testing vs Functional testing

Unit testing is a software testing method by which individual units of source code, sets of one or more computer program modules together with associated control data, usage procedures, and operating procedures, are tested to determine whether they are fit for use. In object-oriented programming, a unit is often an entire interface, such as a class, but could be an individual method. By writing tests first for the smallest testable units, then the compound behaviors between those, one can build up comprehensive tests for complex applications.

The goal of unit testing is to isolate each part of the program and show that the individual parts are correct. A unit test provides a strict, written contract that the piece of code must satisfy. Unit testing allows the programmer to refactor code or upgrade system libraries at a later date, and make sure the module still works correctly (e.g., in regression testing). The procedure is to write test cases for all functions and methods so that whenever a change causes a fault, it can be quickly identified. Unit tests detect changes which may break a design contract.

Functional testing is a quality assurance (QA) process and a type of black-box testing that bases its test cases on the specifications of the software component under test. Functions are tested by feeding them input and examining the output, and internal program structure is rarely considered (unlike white-box testing). Functional testing is conducted to evaluate the compliance of a system or component with specified functional requirements. Functional testing usually describes what the system does.

Functional testing does not imply that you are testing a function (method) of your module or class. Functional testing tests a slice of functionality of the whole system.

| Unit Testing  |  Functional Testing  |
|---|---|
| It tests the Structure.  |  It tests the Functionality.  |
| This is performed by the developer while he codes.  | This is performed by Functional tester in line with User/business documents.  |
| Test happens while coding, one code at a time, ensuring the code correctness.  | Test happens after the development is complete and the software reaches the Acceptance testing phase.  |
| The cost to quality is low as it impacts just one code/function.  | Cost to correct is much higher as it impacts a set of codes that are interdependent.  |
| Testing happens within System Structure.  |  No System Structure Assumptions.  |
| White Box Testing Method.  | Black Box testing Method.  |
| Third-party tool or created within the development group.  | Test conditions created from Business requirements.  |
| Helps in Debugging-process simplification by isolation the code that’s bad.  | It helps in eliminating Functional errors.  |
| Mostly, this is Automation testing.  | It’s a Manual Testing process.  |

Unit testing is fast and helps writing clean code, but doesn’t provide confidence that the system will work as expected. Basically, it tells us where is the problem in the code.

Functional testing is slow and complex but it ensures that the system will work according to the requirements. Basically, it tells us that what is the problem in the functionality.

## Links
https://en.wikipedia.org/wiki/Unit_testing  
https://en.wikipedia.org/wiki/Functional_testing  
https://www.simform.com/unit-testing-vs-functional-testing/  
https://www.educba.com/unit-test-vs-functional-test/
