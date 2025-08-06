# Interpreter pattern
The Interpreter design pattern is a behavioral design pattern that defines a way to interpret and evaluate language grammar or expressions. It provides a mechanism to evaluate sentences in a language by representing their grammar as a set of classes. Each class represents a rule or expression in the grammar, and the pattern allows these classes to be composed hierarchically to interpret complex expressions.<sup>[1](https://www.geeksforgeeks.org/java/interpreter-design-pattern-in-java/#:~:text=Pattern%20in%20Java%3F-,The%20Interpreter%20design%20pattern%20in%20Java%20is%20a%20behavioral%20design%20pattern,allows%20these%20classes%20to%20be%20composed%20hierarchically%20to%20interpret%20complex%20expressions.,-The%20pattern%20involves)</sup>

What problems can the Interpreter design pattern solve?<sup>[2](https://en.wikipedia.org/wiki/Interpreter_pattern#:~:text=What%20problems%20can%20the%20Interpreter%20design%20pattern%20solve%3F%5B,so%20that%20sentences%20in%20the%20language%20can%20be%20interpreted.)</sup>
- A grammar for a simple language should be defined;
- so that sentences in the language can be interpreted.

When a problem occurs very often, it could be considered to represent it as a sentence in a simple language (Domain Specific Languages) so that an interpreter can solve the problem by interpreting the sentence.

For example, when many different or complex search expressions must be specified. Implementing (hard-wiring) them directly into a class is inflexible because it commits the class to particular expressions and makes it impossible to specify new expressions or change existing ones independently from (without having to change) the class.<sup>[3](https://en.wikipedia.org/wiki/Interpreter_pattern#:~:text=When%20a%20problem,change\)%20the%20class.)</sup>

What solution does the Interpreter design pattern describe?<sup>[4](https://en.wikipedia.org/wiki/Interpreter_pattern#:~:text=What%20solution%20does%20the%20Interpreter%20design%20pattern%20describe%3F%5B,Interpret%20a%20sentence%20by%20calling%20interpret()%20on%20the%20AST.)</sup>
- Define a grammar for a simple language by defining an `Expression` class hierarchy and implementing an `interpret()` operation;
- Represent a sentence in the language by an abstract syntax tree (AST) made up of `Expression` instances;
- Interpret a sentence by calling `interpret()` on the AST.

When Should We Use It?<sup>[5](https://medium.com/softaai-blogs/understanding-the-interpreter-design-pattern-in-kotlin-a-comprehensive-guide-28b8dba98bb9#:~:text=When%20Should%20We,a%20structured%20approach.)</sup>

The Interpreter pattern comes in handy when:
- We need to evaluate a series of expressions that follow some grammar or rules;
- We’re dealing with complex expressions that can be broken down into smaller components;
- The language we’re working with is relatively simple but needs a structured approach.

## [Example](https://www.javaguides.net/2023/10/interpreter-design-pattern-in-kotlin.html#:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation Steps:<sup>[6](https://www.javaguides.net/2023/10/interpreter-design-pattern-in-kotlin.html#:~:text=Implementation%20Steps,the%20specific%20expressions.)</sup>
- Define an abstract expression that declares an interpret operation;
- For every rule in the grammar, create a concrete expression class;
- The client creates instances of these concrete expression classes to interpret the specific expressions.

```
// Step 1: Define the Abstract Expression
interface Expression {
    fun interpret(context: String): Boolean
}
// Step 2: Concrete Expressions
class TerminalExpression(private val data: String) : Expression {
    override fun interpret(context: String): Boolean {
        return context.contains(data)
    }
}
class OrExpression(private val expr1: Expression, private val expr2: Expression) : Expression {
    override fun interpret(context: String): Boolean {
        return expr1.interpret(context) || expr2.interpret(context)
    }
}
class AndExpression(private val expr1: Expression, private val expr2: Expression) : Expression {
    override fun interpret(context: String): Boolean {
        return expr1.interpret(context) && expr2.interpret(context)
    }
}
// Client code to interpret expressions
fun getMaleExpression(): Expression {
    val john = TerminalExpression("John")
    val robert = TerminalExpression("Robert")
    return OrExpression(john, robert)
}
fun getMarriedWomanExpression(): Expression {
    val julie = TerminalExpression("Julie")
    val married = TerminalExpression("Married")
    return AndExpression(julie, married)
}
fun main() {
    val isMale = getMaleExpression()
    val isMarriedWoman = getMarriedWomanExpression()
    println("John is male? ${isMale.interpret("John")}")
    println("Julie is a married woman? ${isMarriedWoman.interpret("Married Julie")}")
}
```

Output:
```
John is male? true
Julie is a married woman? true
```

Explanation:<sup>[7](https://www.javaguides.net/2023/10/interpreter-design-pattern-in-kotlin.html#:~:text=Explanation%3A,and%20is%20%22Married%22.)</sup>
- We define an `Expression` interface with the `interpret` method;
- `TerminalExpression`, `OrExpression`, and `AndExpression` are concrete implementations that interpret specific expressions;
- In the client code, we build up a more complex expression by combining the simple terminal expressions. For instance, the `getMarriedWomanExpression` checks if a woman is named "Julie" and is "Married".

## Pros and Cons
Pros:<sup>[8](https://medium.com/softaai-blogs/understanding-the-interpreter-design-pattern-in-kotlin-a-comprehensive-guide-28b8dba98bb9#:~:text=Advantages%20of%20Using,easier%20to%20extend.)</sup>
- **Extensibility**. It’s easy to add more expressions or operators without affecting the existing code;
- **Maintainability**. The expression logic is separated into individual components, making the code cleaner and easier to maintain;
- **Readability**. With the use of well-named classes (like AddExpression, NumberExpression), the code becomes more understandable and easier to extend.

Cons:<sup>[9](https://medium.com/softaai-blogs/understanding-the-interpreter-design-pattern-in-kotlin-a-comprehensive-guide-28b8dba98bb9#:~:text=Disadvantages,very%20large%20grammars.)</sup>
- **Complexity**. For simple scenarios, the Interpreter pattern might introduce unnecessary complexity. If the problem doesn’t require a structured approach, a simpler solution might be more appropriate;
- **Performance**. In cases with large and complex expression trees, the recursive nature of the Interpreter pattern could lead to performance issues. It might not be the best choice for very large grammars.

# Links
[Interpreter Design Pattern in Java](https://www.geeksforgeeks.org/java/interpreter-design-pattern-in-java/)

[Interpreter pattern](https://en.wikipedia.org/wiki/Interpreter_pattern)

[Understanding the Interpreter Design Pattern in Kotlin: A Comprehensive Guide](https://medium.com/softaai-blogs/understanding-the-interpreter-design-pattern-in-kotlin-a-comprehensive-guide-28b8dba98bb9)

[Interpreter Design Pattern in Kotlin](https://www.javaguides.net/2023/10/interpreter-design-pattern-in-kotlin.html)

# Further reading
[Interpreter Design Pattern](https://sourcemaking.com/design_patterns/interpreter)
