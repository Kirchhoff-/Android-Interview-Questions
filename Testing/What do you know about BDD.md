# BDD
**Behavior Driven Development** (BDD) is a branch of Test Driven Development (TDD). BDD uses human-readable descriptions of software user requirements as the basis for software tests. Like Domain Driven Design (DDD), an early step in BDD is the definition of a shared vocabulary between stakeholders, domain experts, and engineers. This process involves the definition of entities, events, and outputs that the users care about, and giving them names that everybody can agree on.

BDD practitioners then use that vocabulary to create a domain specific language they can use to encode system tests such as User Acceptance Tests (UAT).

Although BDD is principally an idea about how software development should be managed by both business interests and technical insight, the practice of BDD does assume the use of specialized software tools to support the development process. Although these tools are often developed specifically for use in BDD projects, they can be seen as specialized forms of the tooling that supports test-driven development. The tools serve to add automation to the ubiquitous language that is a central theme of BDD.

BDD is largely facilitated through the use of a simple domain-specific language (DSL) using natural-language constructs (e.g., English-like sentences) that can express the behaviour and the expected outcomes. Test scripts have long been a popular application of DSLs with varying degrees of sophistication. BDD is considered an effective technical practice especially when the "problem space" of the business problem to solve is complex.

## Principles of BDD
Test-driven development is a software-development methodology which essentially states that for each unit of software, a software developer must:
- define a test set for the unit *first*;
- make the tests fail;
- then implement the unit;
- finally verify that the implementation of the unit makes the tests succeed.

This definition is rather non-specific in that it allows tests in terms of high-level software requirements, low-level technical details or anything in between. One way of looking at BDD therefore, is that it is a continued development of TDD which makes more specific choices than TDD.

Behavior-driven development specifies that tests of any unit of software should be specified in terms of the desired behavior of the unit. Borrowing from agile software development the "desired behavior" in this case consists of the requirements set by the business — that is, the desired behavior that has business value for whatever entity commissioned the software unit under construction. Within BDD practice, this is referred to as BDD being an "outside-in" activity.

## How it works
BDD is characterized by two main features:
- **Behavior**. Software development is based on the behavior the product is supposed to show. However, a test case is only available to the developer. It defines the technical requirements of a software in such a way that technically trained personnel can understand it. But contractors, sponsors, and users of the software, will only be partially able to think with it. Focusing on the behavior of the software, harmonizes the problems of technically demanding implementation of test cases, product requirements, and migration to other systems (development, test, and production environment). Test cases, requirements, and domain-specific problems are generally considered to be the behavior of the software. Instead of defining test methods, the behavior of the software is described, so that all participants can immediately understand the meaning and purpose of individual test cases and user stories. The focus on the behavior of the software also benefits the business value, which a software has for a company or a group of clients. If the software or web application does exactly what it is supposed to, the business objectives are at least partially achieved. Moreover, the BDD approach facilitates the creation of documentation, specifications and APIs.
- **Language**. The focus on behavior leads directly to the content features of the BDD. A developer knows what the software is supposed to do and keeps it in requirement catalogs. However, the developer has privileged access to it, while other project participants such as project management and quality assurance do not. The language used with BDD remedies this. A technical documentation of test cases is translated directly into natural-language elements, so it is also clear which requirements the software must meet, depending on the complex professional contexts of the domain on which the product is created. Thanks to the ubiquitous language, the work on the software can be understood to the last detail by all project participants and the architecture of the software (mostly layer architecture) is directly related to the domain for which the software is created. The domain-driven design is independent of certain programming paradigms, tools, and frameworks, but is usually used in the context of object-oriented programming.

## Expected Benefits
Teams already using TDD or ATDD may want to consider BDD for several reasons:
- BDD offers more precise guidance on organizing the conversation between developers, testers and domain experts;
- Notations originating in the BDD approach, in particular the given-when-then canvas, are closer to everyday language and have a shallower learning curve compared to those of tools such as Fit/FitNesse;
- Tools targeting a BDD approach generally afford the automatic generation of technical and end user documentation from BDD “specifications”.

# Links
[Behavior Driven Development (BDD) and Functional Testing](https://medium.com/javascript-scene/behavior-driven-development-bdd-and-functional-testing-62084ad7f1f2)

[Behavior-driven development](https://en.wikipedia.org/wiki/Behavior-driven_development)

[Behavior Driven Development (BDD)](https://www.agilealliance.org/glossary/bdd)

[Behavior Driven Development](https://en.ryte.com/wiki/Behavior_Driven_Development)

# Next questions
[Describe Test Driven Development](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Testing/Describe%20Test%20Driven%20Development.md)