# Lambda expressions

One issue with anonymous classes is that if the implementation of your anonymous class is very simple, such as an interface that contains only one method, then the syntax of anonymous classes may seem unwieldy and unclear. In these cases, you're usually trying to pass functionality as an argument to another method, such as what action should be taken when someone clicks a button. Lambda expressions enable you to do this, to treat functionality as method argument, or code as data.

Lambda expressions basically express instances of functional interfaces. Lambda expressions implement the only abstract function and therefore implement functional interfaces. Lambda expressions are added in Java 8 and provide next functionalities:
- Enable to treat functionality as a method argument, or code as data.
- A function that can be created without belonging to any class.
- A lambda expression can be passed around as if it was an object and executed on demand.

A lambda expression is characterized by the following syntax:
```
parameter -> expression body
```
where lambda operator can be:
- Zero parameter: `() -> System.out.println("Zero parameter lambda");`
- One parameter: `(p) -> System.out.println("One parameter: " + p);`
- Multiple parameters: `(p1, p2) -> System.out.println("Multiple parameters: " + p1 + ", " + p2);`

The body of a lambda expression, and thus the body of the function / method it represents, is specified to the right of the -> in the lambda declaration: Here is an example:

```
(oldState, newState) -> System.out.println("State changed")
```

If your lambda expression needs to consist of multiple lines, you can enclose the lambda function body inside the { } bracket which Java also requires when declaring methods elsewhere. Here is an example:

```
(oldState, newState) -> {
    System.out.println("Old state: " + oldState);
    System.out.println("New state: " + newState);
  }
```

You can return values from Java lambda expressions, just like you can from a method. You just add a return statement to the lambda function body, like this:

```
(param) -> {
    System.out.println("param: " + param);
    return "return value";
  }
```

In case all your lambda expression is doing is to calculate a return value and return it, you can specify the return value in a shorter way. Instead of this:

```
(a1, a2) -> { return a1 > a2; }
```

You can write:
```
(a1, a2) -> a1 > a2;
```

The compiler then figures out that the expression a1 > a2 is the return value of the lambda expression (hence the name lambda expressions - as expressions return a value of some kind).

Following are the important characteristics of a lambda expression:
- Optional type declaration − No need to declare the type of a parameter. The compiler can inference the same from the value of the parameter.
- Optional parenthesis around parameter − No need to declare a single parameter in parenthesis. For multiple parameters, parentheses are required.
- Optional curly braces − No need to use curly braces in expression body if the body contains a single statement.
- Optional return keyword − The compiler automatically returns the value if the body has a single expression to return the value. Curly braces are required to indicate that expression returns a value.

## Links
https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html  
http://tutorials.jenkov.com/java/lambda-expressions.html  
https://www.geeksforgeeks.org/lambda-expressions-java-8/  
https://www.tutorialspoint.com/java8/java8_lambda_expressions.htm
