# Template method
Template Method Design Pattern is a behavioral pattern that defines the skeleton of an algorithm in a base method while allowing subclasses to override specific steps without altering its overall structure. It’s like a recipe: the main steps remain fixed, but details can be customized for variation.<sup>[1](https://www.geeksforgeeks.org/system-design/template-method-design-pattern/#:~:text=Template%20Method%20Design%20Pattern%20is%20a%20behavioral%20pattern%20that%20defines%20the%20skeleton%20of%20an%20algorithm%20in%20a%20base%20method%20while%20allowing%20subclasses%20to%20override%20specific%20steps%20without%20altering%20its%20overall%20structure.%20It%E2%80%99s%20like%20a%20recipe%3A%20the%20main%20steps%20remain%20fixed%2C%20but%20details%20can%20be%20customized%20for%20variation.)</sup>

This pattern has two main parts:<sup>[2](https://en.wikipedia.org/wiki/Template_method_pattern#:~:text=This%20pattern%20has,template%20method%20itself.)</sup>
- The "template method" is implemented as a method in a **base class** (usually an **abstract class**). This method contains code for the parts of the overall algorithm that are invariant. The template ensures that the overarching algorithm is always followed. In the template method, portions of the algorithm that may vary are implemented by sending self messages that request the execution of additional helper methods. In the base class, these helper methods are given a default implementation, or none at all (that is, they may be abstract methods);
- Subclasses of the base class "fill in" the empty or "variant" parts of the "template" with specific algorithms that vary from one subclass to another. It is important that subclasses do not override the template method itself.

At run-time, the algorithm represented by the template method is executed by sending the template message to an instance of one of the concrete subclasses. Through inheritance, the template method in the base class starts to execute. When the template method sends a message to self requesting one of the helper methods, the message will be received by the concrete sub-instance. If the helper method has been overridden, the overriding implementation in the sub-instance will execute; if it has not been overridden, the inherited implementation in the base class will execute. This mechanism ensures that the overall algorithm follows the same steps every time while allowing the details of some steps to depend on which instance received the original request to execute the algorithm.<sup>[3](https://en.wikipedia.org/wiki/Template_method_pattern#:~:text=At%20run%2Dtime,execute%20the%20algorithm.)</sup>

Use of the template method design pattern:<sup>[4](https://www.geeksforgeeks.org/system-design/template-method-design-pattern/#:~:text=Use%20of%20the,and%20organized%20codebase.)</sup>
- **Common Algorithm with Variations**. When an algorithm has a common structure but differs in some steps or implementations, the Template Method pattern can help subclasses customize specific parts while encapsulating the common phases in a superclass;
- **Code Reusability**. By specifying the common steps in one place, the Template Method design encourages code reuse when you have similar tasks or processes that must be executed in several contexts;
- **Enforcing Structure**. It's useful when you want to provide some elements of an algorithm flexibility while maintaining a particular structure or set of steps;
- **Reducing Duplication**. By centralizing common behavior in the abstract class and avoiding duplication of code in subclasses, the Template Method pattern helps in maintaining a clean and organized codebase.

When not to use the Template Method Design Pattern?<sup>[5](https://www.geeksforgeeks.org/system-design/template-method-design-pattern/#:~:text=When%20not%20to,structure%20at%20runtime.)</sup>
- **When Algorithms are Highly Variable**. Using the Template Method pattern might not be appropriate if the algorithms you're dealing with have a lot of differences in their steps and structure and little in common. This is because it could result in inappropriate abstraction or excessive complexity;
- **Tight Coupling Between Steps**. The Template Method pattern might not offer enough flexibility if there is a close coupling between the algorithm's parts, so modifications to one step require modifications to other steps;
- **Inflexibility with Runtime Changes**. Because the Template Method pattern depends on predetermined structure and behavior, it might not be the best option if you expect frequent changes to the algorithm's stages or structure at runtime.

## [Example](https://www.javaguides.net/2023/10/template-method-design-pattern-in-kotlin.html#:~:text=6.%20Implementation%20in%20Kotlin%20Programming)
Implementation steps:<sup>[6](https://www.javaguides.net/2023/10/template-method-design-pattern-in-kotlin.html#:~:text=5.%20Implementation%20Steps,implement%20specific%20behaviors.)</sup>
- Create an abstract class with the template method;
- Within this class, provide default implementations or abstract definitions for the variable steps;
- Derive subclasses from the abstract class to implement specific behaviors.

```
// Step 1: Abstract Class with Template Method
abstract class DataMiner {
    fun mineData() {
        extractData()
        analyzeData()
        generateReport()
    }
    abstract fun extractData()
    abstract fun analyzeData()
    fun generateReport() {
        println("Generating report based on analysis.")
    }
}
// Step 2: Concrete Implementations
class CSVDataMiner : DataMiner() {
    override fun extractData() {
        println("Extracting data from CSV file.")
    }
    override fun analyzeData() {
        println("Analyzing CSV data.")
    }
}
class XMLDataMiner : DataMiner() {
    override fun extractData() {
        println("Extracting data from XML file.")
    }
    override fun analyzeData() {
        println("Analyzing XML data.")
    }
}
// Client Code
fun main() {
    val csvMiner = CSVDataMiner()
    csvMiner.mineData()
    val xmlMiner = XMLDataMiner()
    xmlMiner.mineData()
}
```

Output:
```
Extracting data from CSV file.
Analyzing CSV data.
Generating report based on analysis.
Extracting data from XML file.
Analyzing XML data.
Generating report based on analysis.
```

Explanation:<sup>[7](https://www.javaguides.net/2023/10/template-method-design-pattern-in-kotlin.html#:~:text=Explanation%3A,by%20respective%20subclasses.)</sup> 
- `DataMiner` is the abstract class containing the template method `mineData`, which outlines the algorithm's structure;
- Specific steps, like `extractData` and `analyzeData`, are abstract and must be implemented by subclasses;
- `generateReport` has a default implementation which can be overridden by subclasses if needed;
- Concrete classes, `CSVDataMiner` and `XMLDataMiner`, provide specific implementations for extracting and analyzing data;
- The client code demonstrates the algorithm's consistency, with specifics handled by respective subclasses.

# Pros and Cons
Pros:<sup>[8](https://dev.to/mspilari/template-method-pattern-in-java-5f27#:~:text=Advantages%20of%20the,across%20multiple%20implementations.)</sup> 
- **Code Reusability**. Reduces code duplication by defining a common structure for algorithms;
- **Encapsulation**. Hides the specific steps of an algorithm, exposing only the necessary ones to subclasses;
- **Enforces Consistency**. Ensures that the algorithm follows a standard structure across multiple implementations.

Cons:<sup>[9](https://dev.to/mspilari/template-method-pattern-in-java-5f27#:~:text=Disadvantages%20of%20the,code%20more%20complex.)</sup>
- **Limited Flexibility**. The overall algorithm structure is fixed, which can make it hard to adapt to changes;
- **Increased Maintenance Effort**. Modifications to the template method can impact all subclasses;
- **Difficult to Understand**. If there are too many abstract methods or hooks, the pattern can make the code more complex.

# Links
[Template Method Design Pattern](https://www.geeksforgeeks.org/system-design/template-method-design-pattern/)

[Template method pattern](https://en.wikipedia.org/wiki/Template_method_pattern)

[Template Method Design Pattern in Kotlin](https://www.javaguides.net/2023/10/template-method-design-pattern-in-kotlin.html)

[Template Method Pattern in Java](https://dev.to/mspilari/template-method-pattern-in-java-5f27)

# Further reading
[Template Method Design Pattern](https://sourcemaking.com/design_patterns/template_method)

[Template Method](https://refactoring.guru/design-patterns/template-method)

[Template Method Software Pattern Kotlin Examples](https://softwarepatterns.com/kotlin/template-method-software-pattern-kotlin-example)

[Template Method Pattern: Define the Flow, Customize the Steps](https://maxim-gorin.medium.com/template-method-pattern-define-the-flow-customize-the-steps-027d5c3cfcc6)
