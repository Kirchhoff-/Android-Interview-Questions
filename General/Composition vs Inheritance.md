# Composition vs Inheritance
Composition over inheritance (or composite reuse principle) in object-oriented programming (OOP) is the principle that classes should achieve polymorphic behavior and code reuse by their composition (by containing instances of other classes that implement the desired functionality) rather than inheritance from a base or parent class. This is an often-stated principle of OOP, such as in the influential book Design Patterns.

An implementation of composition over inheritance typically begins with the creation of various interfaces representing the behaviors that the system must exhibit. Interfaces enable polymorphic behavior. Classes implementing the identified interfaces are built and added to business domain classes as needed. Thus, system behaviors are realized without inheritance.

In fact, business domain classes may all be base classes without any inheritance at all. Alternative implementation of system behaviors is accomplished by providing another class that implements the desired behavior interface. A class that contains a reference to an interface can support implementations of the interface - a choice that can be delayed until run time.

## Composition over Inheritance
1. Inheritance is tightly coupled whereas composition is loosely coupled. Let’s assume we have below classes with inheritance
```
public class ClassA {
	public void foo(){	
	}
}

class ClassB extends ClassA{
	public void bar(){
	}
}
```
There could be many classes extending the superclass `ClassA`. Now let’s say `ClassA` implementation is changed like below, a new method `bar()` is added:
```
public class ClassA {

	public void foo(){	
	}
	
	public int bar(){
		return 0;
	}
}
```

As soon as you start using new `ClassA` implementation, you will get compile time error in `ClassB` as `The return type is incompatible with ClassA.bar()`. The solution would be to change either the superclass or the subclass `bar()` method to make them compatible.

If you would have used Composition over inheritance, you will never face this problem. A simple example of `ClassB` implementation using Composition can be as below.
```
class ClassB{
	ClassA classA = new ClassA();
	
	public void bar(){
		classA.foo();
		classA.bar();
	}
}
```

2. There is no access control in inheritance whereas access can be restricted in composition. We expose all the superclass methods to the other classes having access to subclass. So if a new method is introduced or there are security holes in the superclass, subclass becomes vulnerable. Since in composition we choose which methods to use, it’s more secure than inheritance. For example, we can provide `ClassA` `foo()` method exposure to other classes using below code in `ClassB`.

```
class ClassB {
	
	ClassA classA = new ClassA();
	
	public void foo(){
		classA.foo();
	}
	
	public void bar(){	
	}
	
}
```
This is one of the major advantage of composition over inheritance.

3. Composition provides flexibility in invocation of methods that is useful with multiple subclass scenario. For example, let’s say we have below inheritance scenario.
```
abstract class Abs {
	abstract void foo();
}

public class ClassA extends Abs{

	public void foo(){	
	}
	
}

class ClassB extends Abs{
		
	public void foo(){
	}
	
}

class Test {
	
	ClassA a = new ClassA();
	ClassB b = new ClassB();

	public void test(){
		a.foo();
		b.foo();
	}
}
```

So what if there are more subclasses, will composition make our code ugly by having one instance for each subclass? No, we can rewrite the Test class like below.

```
class Test {
	Abs obj = null;
	
	Test1(Abs o){
		this.obj = o;
	}
	
	public void foo(){
		this.obj.foo();
	}

}
```
This will give you the flexibility to use any subclass based on the object used in the constructor.

4. One more benefit of composition over inheritance is testing scope. Unit testing is easy in composition because we know what all methods we are using from another class. We can mock it up for testing whereas in inheritance we depend heavily on superclass and don’t know what all methods of superclass will be used. So we will have to test all the methods of the superclass. This is extra work and we need to do it unnecessarily because of inheritance.

## Conclusion
To favor composition over inheritance is a design principle that gives the design higher flexibility. It is more natural to build business-domain classes out of various components than trying to find commonality between them and creating a family tree. For example, an accelerator pedal and a steering wheel share very few common traits, yet both are vital components in a car. What they can do and how they can be used to benefit the car is easily defined. Composition also provides a more stable business domain in the long term as it is less prone to the quirks of the family members. In other words, it is better to compose what an object can do (HAS-A) than extend what it is (IS-A).

Initial design is simplified by identifying system object behaviors in separate interfaces instead of creating a hierarchical relationship to distribute behaviors among business-domain classes via inheritance. This approach more easily accommodates future requirements changes that would otherwise require a complete restructuring of business-domain classes in the inheritance model. Additionally, it avoids problems often associated with relatively minor changes to an inheritance-based model that includes several generations of classes.

| Inheritance | Composition |
|---|---|
| Define the class which we are inheriting(super class) and most importantly it cannot be changed at runtime  | Define a type which we want to use and which can hold its different implementation also it can change at runtime  |
| Can only extend one class, in other words more than one class can’t be extended as java do not support multiple inheritance  |  Allows to use functionality from different class  |
| Need parent class in order to test child class  | Allows to test the implementation of the classes we are using independent of parent or child class  |
| Cannot extend final class  | Allows code reuse even from final classes  |
| It is an is-a relationship  | While it is a has-a relationship  |

Inheritance should only be used when:
- Both classes are in the same logical domain
- The subclass is a proper subtype of the superclass
- The superclass’s implementation is necessary or appropriate for the subclass
- The enhancements made by the subclass are primarily additive.

There are times when all of these things converge:
- Higher-level domain modeling
- Frameworks and framework extensions
- Differential programming

## Links
https://en.wikipedia.org/wiki/Composition_over_inheritance  
https://www.geeksforgeeks.org/difference-between-inheritance-and-composition-in-java/  
https://www.journaldev.com/12086/composition-vs-inheritance  
https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose  
