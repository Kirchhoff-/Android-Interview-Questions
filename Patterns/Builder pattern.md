# Builder pattern
Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.<sup>[1](https://refactoring.guru/design-patterns/builder#:~:text=Builder%20is%20a%20creational%20design%20pattern%20that%20lets%20you%20construct%20complex%20objects%20step%20by%20step.%20The%20pattern%20allows%20you%20to%20produce%20different%20types%20and%20representations%20of%20an%20object%20using%20the%20same%20construction%C2%A0code.)</sup>

The builder design pattern solves problems like:
- How can a class (the same construction process) create different representations of a complex object?
- How can a class that includes creating a complex object be simplified?

Creating and assembling the parts of a complex object directly within a class is inflexible. It commits the class to creating a particular representation of the complex object and makes it impossible to change the representation later independently from (without having to change) the class.

The builder design pattern describes how to solve such problems:
- Encapsulate creating and assembling the parts of a complex object in a separate `Builder` object;
- A class delegates object creation to a `Builder` object instead of creating the objects directly.

A class (the same construction process) can delegate to different `Builder` objects to create different representations of a complex object.<sup>[2](https://en.wikipedia.org/wiki/Builder_pattern#:~:text=The%20builder%20design,a%20complex%20object.)</sup>

Also builder solves problem of telescopic constructors, when many variants of constructor are created with increasing number of arguments:
```
constructor(firstName: String): this(firstName, "", 0)
constructor(firstName: String, lastName: String): this(firstName, lastName, 0)
constructor(firstName: String, lastName: String, age: Int): this(firstName, lastName, age)
```

Technically they allow you to use the constructor with just enough arguments you want to set, but in practice adding a new field to the class forces you to modify each constructor.<sup>[3](https://swiderski.tech/kotlin-builder-pattern/#:~:text=Builder%20solves%20problem,modify%20each%20constructor.)</sup>

## Pros and Cons
[Advantages](https://en.wikipedia.org/wiki/Builder_pattern#:~:text=%5B1%5D-,Advantages,-%5Bedit%5D):
- Allows you to vary a product's internal representation;
- Encapsulates code for construction and representation;
- Provides control over the steps of the construction process;

[Disadvantages](https://en.wikipedia.org/wiki/Builder_pattern#:~:text=the%20construction%20process.-,Disadvantages,-%5Bedit%5D):
- Builder classes must be mutable;
- May hamper/complicate dependency injection.

## [Example usage](https://swiderski.tech/kotlin-builder-pattern/#:~:text=the%20need%20comes.-,Example%20usage,-Because%20of%20my)
[`NotificationBuilder`](https://developer.android.com/reference/android/app/Notification.Builder):
```
val notificationBuilder = Notification.Builder(this, "channelId")
notificationBuilder.setContentTitle("Title")
notificationBuilder.setContentText("Content")
notificationBuilder.setSmallIcon(R.mipmap.ic_launcher)
val notification = notificationBuilder.build()
```

Most traditional usage of the Builder Pattern. Builders Constructor takes 2 arguments necessary for proper object creation, other fields are getting values through setter methods called on the Builder instance. Finally the `build()` method is called that returns desired notification object.

[`AlertDialogBuilder`](https://developer.android.com/reference/kotlin/androidx/appcompat/app/AlertDialog.Builder?hl=en):
```
val dialog = AlertDialog.Builder(this)
        .apply {
            setTitle("Title")
            setIcon(R.mipmap.ic_launcher)
        }.show()
```

Very Kotlin style with utilizing the apply. Interestingly enough there is no `build()` method but `show()` that is not only returning dialog object but also displays it. Sounds like a bad idea for a method to do more than one thing, but in this case I believe it was done on purpose to avoid a common mistake of creating a dialog but forgetting to display it with a separate method.

## [Example](https://www.baeldung.com/kotlin/builder-pattern#kotlin-style)
```
class FoodOrder private constructor(
    val bread: String = "Flat bread",
    val condiments: String?,
    val meat: String?,
    val fish: String?) {

    data class Builder(
        var bread: String? = null,
        var condiments: String? = null,
        var meat: String? = null,
        var fish: String? = null) {

        fun bread(bread: String) = apply { this.bread = bread }
        fun condiments(condiments: String) = apply { this.condiments = condiments }
        fun meat(meat: String) = apply { this.meat = meat }
        fun fish(fish: String) = apply { this.fish = fish }
        fun build() = FoodOrder(bread, condiments, meat, fish)
        fun randomBuild() = bread(bread ?: "dry")
          .condiments(condiments ?: "pepper")
          .meat(meat ?: "beef")
          .fish(fish?: "Tilapia")
          .build()
    }
}

val foodOrder = FoodOrder.Builder()
  .bread("white bread")
  .meat("bacon")
  .condiments("olive oil")
  .build()
```

Kotlin comes with named and default parameters that help to minimize the number of overloads and improve the readability of the function invocation.

# Links
[Builder](https://refactoring.guru/design-patterns/builder)

[Builder pattern](https://en.wikipedia.org/wiki/Builder_pattern)

[Kotlin Builder Pattern](https://swiderski.tech/kotlin-builder-pattern/)

# Further reading
[Builder Design Pattern in Kotlin](https://medium.com/@ssvaghasiya61/builder-design-pattern-in-kotlin-50d6c669675c)
