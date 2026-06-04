# Guard conditions
Guard conditions allow you to include more than one condition to the branches of a `when` expression or statement, making complex control flow more explicit and concise. You can use guard conditions with `when` as long as it has a subject.<sup>[1](https://kotlinlang.org/docs/control-flow.html#guard-conditions-in-when-expressions:~:text=Guard%20conditions%20allow%20you%20to%20include%20more%20than%20one%20condition%20to%20the%20branches%20of%20a%20when%20expression%20or%20statement%2C%20making%20complex%20control%20flow%20more%20explicit%20and%20concise.%20You%20can%20use%20guard%20conditions%20with%20when%20as%20long%20as%20it%20has%20a%20subject.)</sup>

Place a guard condition after the primary condition in the same branch, separated by `if`:<sup>[2](https://kotlinlang.org/docs/control-flow.html#guard-conditions-in-when-expressions:~:text=Place%20a%20guard%20condition%20after%20the%20primary%20condition%20in%20the%20same%20branch%2C%20separated%20by%20if%3A)</sup>
```kotlin
fun feedAnimal(animal: Animal) {
    when (animal) {
        // Branch with only primary condition
        // Calls feedDog() when animal is Dog
        is Animal.Dog -> feedDog()
        // Branch with both primary and guard conditions
        // Calls feedCat() when animal is Cat and not mouseHunter
        is Animal.Cat if !animal.mouseHunter -> feedCat()
        // Prints "Unknown animal" if none of the above conditions match
        else -> println("Unknown animal")
    }
}

fun main() {
    val animals = listOf(
        Animal.Dog("Beagle"),
        Animal.Cat(mouseHunter = false),
        Animal.Cat(mouseHunter = true)
    )

    animals.forEach { feedAnimal(it) }
    // Feeding a dog
    // Feeding a cat
    // Unknown animal
}
```

You can't use guard conditions when you have multiple conditions separated by a comma. For example:
```
0, 1 -> print("x == 0 or x == 1")
```

In a single `when` expression or statement, you can combine branches with and without guard conditions. The code in a branch with a guard condition runs only if both the primary condition and the guard condition evaluate to `true`. If the primary condition doesn't match, the guard condition isn't evaluated.

Since `when` statements don't need to cover all cases, using guard conditions in `when` statements without an `else` branch means that if no conditions match, no code is run.<sup>[3](https://kotlinlang.org/docs/control-flow.html#guard-conditions-in-when-expressions:~:text=You%20can%27t%20use,code%20is%20run.)</sup>

Guard conditions also support `else if`:<sup>[4](https://kotlinlang.org/docs/control-flow.html#for-loops:~:text=Guard%20conditions%20also%20support%20else%20if%3A)</sup>
```kotlin
when (animal) {
    // Checks if `animal` is `Dog`
    is Animal.Dog -> feedDog()
    // Guard condition that checks if `animal` is `Cat` and not `mouseHunter`
    is Animal.Cat if !animal.mouseHunter -> feedCat()
    // Calls giveLettuce() if none of the above conditions match and animal.eatsPlants is true
    else if animal.eatsPlants -> giveLettuce()
    // Prints "Unknown animal" if none of the above conditions match
    else -> println("Unknown animal")
}
```

## Android example
Guard conditions can be especially useful when working with UI states, sealed classes, or other scenarios where multiple branches share the same type but require additional validation.

A common example is handling a successful state differently depending on whether the received data is empty or not.

Without guard conditions, this often requires additional nested `if` statements inside a `when` branch. With guard conditions, the additional check can be expressed directly as part of the branch itself.

```kotlin
when (state) {
    is UiState.Success if state.data.isNotEmpty() -> showContent(state.data)
    is UiState.Success -> showEmptyState()
    is UiState.Loading -> showLoading()
    is UiState.Error -> showError()
}
```

# Links
[Conditions and loops﻿](https://kotlinlang.org/docs/control-flow.html)

# Further reading
[Kotlin Guards Explained: Boost Code Clarity with `when` Statements in Kotlin 2.1](https://proandroiddev.com/kotlin-guards-explained-boost-code-clarity-with-when-statements-in-kotlin-2-1-776ec4c1b84a)
