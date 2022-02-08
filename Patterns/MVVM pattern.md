# MVVM pattern
**Model–view–viewmodel (MVVM)** is a software architectural pattern that facilitates the separation of the development of the UI - rom the development of the business logic or back-end logic (the model) so that the view is not dependent on any specific model platform. 

The separate code layers of MVVM are:
- **Model**: This layer is responsible for the abstraction of the data sources. Model and ViewModel work together to get and save the data.
- **View**: The purpose of this layer is to inform the ViewModel about the user’s action. This layer observes the ViewModel and does not contain any kind of application logic.
- **ViewModel**: It exposes those data streams which are relevant to the View. Moreover, it servers as a link between the Model and the View.

![](./res/mvvm_android.png "MVVM")

It is important to note that in MVVM, the view doesn’t maintain state information; instead, it is synchronized with the ViewModel. In MVVM, the ViewModel isolates the presentation layer and offers methods and commands for managing the state of a view and manipulating the model.

The view and the Viewmodel in the MVVM design pattern communicate using methods, properties, and events. The view and the Viewmodel have bi-directional data binding, or two-way data binding, which guarantees that the Viewmodel’s models and properties are in sync with the view.

## Examples
[MVVM Architecture - Android Tutorial for Beginners - Step by Step Guide](https://blog.mindorks.com/mvvm-architecture-android-tutorial-for-beginners-step-by-step-guide)

[Guide to app architecture](https://developer.android.com/jetpack/guide)

[Pokedex](https://github.com/skydoves/Pokedex)

[Foodium](https://github.com/PatilShreyas/Foodium)

[MVVM-Kotlin-Android-Architecture](https://github.com/ahmedeltaher/MVVM-Kotlin-Android-Architecture)


## Conclusion
Advantages:
- **Maintainability** – Can remain agile and keep releasing successive versions quickly;
- **Extensibility** – Have the ability to replace or add new pieces of code;
- **Testability** – Easier to write unit tests against a core logic;
- **Transparent Communication** – The view model provides a transparent interface to the view controller, which it uses to populate the view layer and interact with the model layer, which results in a transparent communication between the layers of your application.

Disadvantages: 
- Some people think that for simple UIs, MVVM can be overkill;
- In bigger cases, it can be hard to design the ViewModel;
- Debugging would be a bit difficult when we have complex data bindings.

# Links
[Model–view–viewmodel](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel)

[MVVM (Model View ViewModel) Architecture Pattern in Android](https://www.geeksforgeeks.org/mvvm-model-view-viewmodel-architecture-pattern-in-android/)

[Introduction to Model View View Model (MVVM)](https://www.geeksforgeeks.org/introduction-to-model-view-view-model-mvvm/)

[Comparing the MVC MVP and MVVM Design Patterns](https://www.developer.com/design/mvc-vs-mvp-vs-mvvm-design-patterns/)

[Android Architecture Patterns Part 3: Model-View-ViewModel](https://medium.com/upday-devs/android-architecture-patterns-part-3-model-view-viewmodel-e7eeee76b73b)

# Futher reading
[MVVM and DataBinding: Android Design Patterns](https://www.raywenderlich.com/636803-mvvm-and-databinding-android-design-patterns)
