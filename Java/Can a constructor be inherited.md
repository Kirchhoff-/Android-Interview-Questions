# Constructor inherited

Short answer - no, a constructor cannot be inherited. Reasons:
 - Constructors are special and have same name as class name. So if constructors were inherited in child class then child class would contain a parent class constructor which is against the constraint that constructor should have same name as class name
 - A constructor cannot be called as a method. It is called when object of the class is created so it does not make sense of creating child class object using parent class constructor notation. i.e. `Child c = new Parent()`;
 - If constructors can be inherited then it will be impossible to achieving encapsulation. Because by using a super class’s constructor we can access/initialize private members of a class.

## Links
https://www.geeksforgeeks.org/constructors-not-inherited-java/  
https://stackoverflow.com/questions/18147768/why-constructors-can-not-be-inherited-in-java
