# Constructors invocation order
Suppose we have next code:
```
open class Parent {

    private val a = println("Parent.a")

    constructor(arg: String = "Default arg for parent") {
        println("Parent primary constructor")
        println("Parent arg value = $arg")
    }

    init {
        println("Parent.init")
    }

    private val b = println("Parent.b")
}

class Child: Parent {

    private val a = println("Child.a")

    init {
        println("Child.init 1")
    }

    constructor(arg: String = "Default arg for child"): super(arg) {
        println("Child primary constructor")
    }

    constructor(arg: Int): this(arg.toString()) {
        println("Child secondary constructor")
    }

    init {
        println("Child.init 2")
    }
}
```

What will be displayed in the console in the following cases:
- `Parent()`
- `Parent("Value")`
- `Child()`
- `Child("Value")`
- `Child(IntValue)`

<details>
<summary>Parent()</summary>
  
```
Parent.a
Parent.init
Parent.b
Parent primary constructor
Parent arg value = Default arg for parent
```
  
</details>

<details>
<summary>Parent(Value)</summary>
  
```
Parent.a
Parent.init
Parent.b
Parent primary constructor
Parent arg value = Value
```
  
</details>

<details>
<summary>Child()</summary>
  
```
Parent.a
Parent.init
Parent.b
Parent primary constructor
Parent arg value = Default arg for child
Child.a
Child.init 1
Child.init 2
Child primary constructor
```
  
</details>

<details>
<summary>Child(Value)</summary>
  
```
Parent.a
Parent.init
Parent.b
Parent primary constructor
Parent arg value = Value
Child.a
Child.init 1
Child.init 2
Child primary constructor
```
  
</details>

<details>
<summary>Child(IntValue)</summary>
  
```
Parent.a
Parent.init
Parent.b
Parent primary constructor
Parent arg value = 2
Child.a
Child.init 1
Child.init 2
Child primary constructor
Child secondary constructor
```
  
</details>

**Tips**: Code in initializer blocks effectively becomes part of the primary constructor. Delegation to the primary constructor happens as the first statement of a secondary constructor, so the code in all initializer blocks and property initializers is executed before the body of the secondary constructor.
 
# Links
[Classes](https://kotlinlang.org/docs/classes.html#creating-instances-of-classes)

# Futher reading
[Kotlin Constructors](https://www.programiz.com/kotlin-programming/constructors)

[An in-depth look at Kotlin’s initializers](https://medium.com/keepsafe-engineering/an-in-depth-look-at-kotlins-initializers-a0420fcbf546)
