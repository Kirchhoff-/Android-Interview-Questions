# JDK & JRE & JVM

## JDK

**Java Development Kit (JDK)** is Kit which provides the environment to develop and execute(run) the Java program. JDK is a kit(or package) which includes two things:
- Development Tools(to provide an environment to develop your java programs)
- JRE (to execute your java program).

Features of JDK:

- JDK includes all features that JRE has.
- It contains development tools such as a compiler, debugger, etc.
- JDK provides the environment to develop and execute Java source code.
- It can be installed on Windows, Unix, and Mac operating systems.

![](./res/jdk.png "Java Development Kit (JDK)")

## JRE

**Java Runtime Environment (JRE)** is an installation package which provides environment to only run(not develop) the java program(or application)onto your machine. JRE is only used by them who only wants to run the Java Programs i.e. end users of your system.

Features of JDE:

- JRE contains class libraries, JVM, and other supporting files. It does not contain any tool for Java development like a debugger, compiler, etc.
- It uses important package classes like math, swingetc, util, lang, awt, and runtime libraries.
- If you have to run Java applets, then JRE must be installed in your system.

![](./res/jre.png "Java Runtime Environment (JRE)")

## JVM

**Java Virtual machine(JVM)** is an abstract machine. It is called a virtual machine because it doesn't physically exist. It is a specification that provides a runtime environment in which Java bytecode can be executed. It can also run those programs which are written in other languages and compiled to Java bytecode.

Features of JVM:

- JVM provides a platform-independent way of executing Java source code.
- It has numerous libraries, tools, and frameworks.
- Once you run Java program, you can run on any platform and save lots of time.
- JVM comes with JIT(Just-in-Time) compiler that converts Java source code into low-level machine language. Hence, it runs more faster as a regular application.

## Conclusion

| JDK | JRE | JVM |
|---|---|---|
| The full form of JDK is Java Development Kit. | The full form of JRE is Java Runtime Environment. | The full form of JVM is Java Virtual Machine. |
| JDK is a software development kit to develop applications in Java. | It is a software bundle which provides Java class libraries with necessary components to run Java code. | JVM executes Java byte code and provides an environment for executing it. |
| JDK is platform dependent. | JRE is also platform dependent. | JVM is platform-independent. |
| It contains tools for developing, debugging, and monitoring java code. | It contains class libraries and other supporting files that JVM requires to execute the program. | Software development tools are not included in JVM. |
| It is the superset of JRE | It is the subset of JDK. | JVM is a subset of JRE. |
| The JDK enables developers to create Java programs that can be executed and run by the JRE and JVM. | The JRE is the part of Java that creates the JVM. | It is the Java platform component that executes source code. |
| JDK comes with the installer. | JRE only contain environment to execute source code. | JVM bundled in both software JDK and JRE. |

## Links
https://www.javatpoint.com/difference-between-jdk-jre-and-jvm   
https://www.geeksforgeeks.org/differences-jdk-jre-jvm/  
https://www.guru99.com/difference-between-jdk-jre-jvm.html  
