## How to create empty constructor for data class?
<details>
  <summary>Click to expand!</summary>
  If you give default value to all the fields - empty constructor is generated automatically by Kotlin:
  
  ```
  data class Information(
    var title: String = "",
    var text: String = "",
  )
  ```
  
  or you can declare a secondary constructor that nas no parameter:
  ```
  data class Information(
    var title: String,
    var text: String,
  ) {
    constructor(): this("", "")
  }
  ```
</details>


## What will be result of the following code execurion?
```
val innerProperty by lazy { 
    println("Calculating the value")
    "Result"
}

fun main(args: Array<String>) {
  println(innerProperty)
  println(innerProperty)
}
```
<details>
  <summary>Click to expand!</summary>
For lazy the first time you access the Lazy property, the initialisation (lazy() function invocation) takes place. The second time, this value is remembered and returned: 
  
  ```
Calculating the value
Result
Result
  ```
</details>
