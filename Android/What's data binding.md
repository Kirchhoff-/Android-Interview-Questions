# Data binding

Data binding is a general technique that binds data sources from the provider and consumer together and synchronizes them. In a data binding process, each data change is reflected automatically by the elements that are bound to the data. The term data binding is also used in cases where an outer representation of data in an element changes, and the underlying data is automatically updated to reflect this change.

The Data Binding Library is a support library that allows you to bind UI components in your layouts to data sources in your app using a declarative format rather than programmatically.

Layouts are often defined in activities with code that calls UI framework methods. For example, the code below calls `findViewById()` to find a `TextView` widget and bind it to the `userName` property of the `viewModel` variable:
```
findViewById<TextView>(R.id.sample_text).apply {
    text = viewModel.userName
}
```

With helping of data binding library we can simplify code showed above by moving such logic to XML:

```
<TextView
    android:text="@{viewmodel.userName}" />
```

Binding components in the layout file lets you remove many UI framework calls in your activities, making them simpler and easier to maintain. This can also improve your app's performance and help prevent memory leaks and null pointer exceptions.

## Layouts and binding expressions
The expression language allows you to write expressions that connect variables to the views in the layout. The Data Binding Library automatically generates the classes required to bind the views in the layout with your data objects. The library provides features such as imports, variables, and includes that you can use in your layouts.

These features of the library coexist seamlessly with your existing layouts. For example, the binding variables that can be used in expressions are defined inside a `data` element that is a sibling of the UI layout's root element. Both elements are wrapped in a `layout` tag, as shown in the following example:
```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <variable
            name="viewmodel"
            type="com.myapp.data.ViewModel" />
    </data>
    <ConstraintLayout... /> <!-- UI layout's root element -->
</layout>
```

## Work with observable data objects
The Data Binding Library provides classes and methods to easily observe data for changes. You don't have to worry about refreshing the UI when the underlying data source changes. You can make your variables or their properties observable. The library allows you to make objects, fields, or collections observable.

## Binding adapters
For every layout expression, there is a binding adapter that makes the framework calls required to set the corresponding properties or listeners. For example, the binding adapter can take care of calling the `setText()` method to set the text property or call the `setOnClickListener()` method to add a listener to the click event. The most common binding adapters, such as the adapters for the `android:text` property used in the examples in this page, are available for you to use in the `android.databinding.adapters` package. You can also create custom adapters, as shown in the following example:

```
@BindingAdapter("app:goneUnless")
fun goneUnless(view: View, visible: Boolean) {
    view.visibility = if (visible) View.VISIBLE else View.GONE
}
```

## Two-way data binding
The Data Binding Library supports two-way data binding. The notation used for this type of binding supports the ability to receive data changes to a property and listen to user updates to that property at the same time.

## Links  
https://developer.android.com/topic/libraries/data-binding  
https://en.wikipedia.org/wiki/Data_binding  
https://androidwave.com/data-binding-in-android-tutorial/  
https://android.jlelse.eu/android-data-binding-8d0eb34b9bad
