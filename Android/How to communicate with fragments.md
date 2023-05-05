# Communicating with fragments
To reuse fragments, build them as completely self-contained components that define their own layout and behavior. Once you define these reusable fragments, you can associate them with an activity and connect them with the application logic to realize the overall composite UI.

To properly react to user events and to share state information, you often need to have channels of communication between an activity and its fragments or between two or more fragments. To keep fragments self-contained, *don't* have fragments communicate directly with other fragments or with their host activity.

The `Fragment` library provides two options for communication: a shared `ViewModel` and the Fragment Result API. The recommended option depends on the use case. To share persistent data with custom APIs, use a `ViewModel`. For a one-time result with data that can be placed in a `Bundle`, use the Fragment Result API.<sup>[1](https://developer.android.com/guide/fragments/communicate#:~:text=To%20reuse%20fragments%2C%20build%20them,use%20the%20Fragment%20Result%20API.)</sup>

## [Share data using a ViewModel](https://developer.android.com/guide/fragments/communicate#viewmodel)
`ViewModel` is an ideal choice when you need to share data between multiple fragments or between fragments and their host activity. `ViewModel` objects store and manage UI data.

### [Share data with the host activity](https://developer.android.com/guide/fragments/communicate#host-activity)
In some cases, you might need to share data between fragments and their host activity. For example, you might want to toggle a global UI component based on an interaction within a fragment.

Consider the following `ItemViewModel`:
```
class ItemViewModel : ViewModel() {
    private val mutableSelectedItem = MutableLiveData<Item>()
    val selectedItem: LiveData<Item> get() = mutableSelectedItem

    fun selectItem(item: Item) {
        mutableSelectedItem.value = item
    }
}
```

Both your fragment and its host activity can retrieve a shared instance of a `ViewModel` with activity scope by passing the activity into the `ViewModelProvider` constructor. The `ViewModelProvider` handles instantiating the `ViewModel` or retrieving it if it already exists. Both components can observe and modify this data.
```
class MainActivity : AppCompatActivity() {
    // Using the viewModels() Kotlin property delegate from the activity-ktx
    // artifact to retrieve the ViewModel in the activity scope.
    private val viewModel: ItemViewModel by viewModels()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        viewModel.selectedItem.observe(this, Observer { item ->
            // Perform an action with the latest item data.
        })
    }
}

class ListFragment : Fragment() {
    // Using the activityViewModels() Kotlin property delegate from the
    // fragment-ktx artifact to retrieve the ViewModel in the activity scope.
    private val viewModel: ItemViewModel by activityViewModels()

    // Called when the item is clicked.
    fun onItemClicked(item: Item) {
        // Set a new item.
        viewModel.selectItem(item)
    }
}
```

### [Share data between fragments](https://developer.android.com/guide/fragments/communicate#fragments)
Two or more fragments in the same activity often need to communicate with each other. For example, imagine one fragment that displays a list and another that lets the user apply various filters to the list. Implementing this case isn't trivial without the fragments communicating directly, but then they are no longer self-contained. Additionally, both fragments must handle the scenario where the other fragment is not yet created or visible.

These fragments can share a `ViewModel` using their activity scope to handle this communication. By sharing the `ViewModel` in this way, the fragments don't need to know about each other, and the activity doesn't need to do anything to facilitate the communication.

The following example shows how two fragments can use a shared `ViewModel` to communicate:
```
class ListViewModel : ViewModel() {
    val filters = MutableLiveData<Set<Filter>>()

    private val originalList: LiveData<List<Item>>() = ...
    val filteredList: LiveData<List<Item>> = ...

    fun addFilter(filter: Filter) { ... }

    fun removeFilter(filter: Filter) { ... }
}

class ListFragment : Fragment() {
    // Using the activityViewModels() Kotlin property delegate from the
    // fragment-ktx artifact to retrieve the ViewModel in the activity scope.
    private val viewModel: ListViewModel by activityViewModels()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        viewModel.filteredList.observe(viewLifecycleOwner, Observer { list ->
            // Update the list UI.
        }
    }
}

class FilterFragment : Fragment() {
    private val viewModel: ListViewModel by activityViewModels()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        viewModel.filters.observe(viewLifecycleOwner, Observer { set ->
            // Update the selected filters UI.
        }
    }

    fun onFilterSelected(filter: Filter) = viewModel.addFilter(filter)

    fun onFilterDeselected(filter: Filter) = viewModel.removeFilter(filter)
}
```

Both fragments use their host activity as the scope for the `ViewModelProvider`. Because the fragments use the same scope, they receive the same instance of the `ViewModel`, which enables them to communicate back and forth.

### [Share data between a parent and child fragment](https://developer.android.com/guide/fragments/communicate#share_data_between_a_parent_and_child_fragment)
When working with child fragments, your parent fragment and its child fragments might need to share data with each other. To share data between these fragments, use the parent fragment as the `ViewModel` scope, as shown in the following example:
```
class ListFragment: Fragment() {
    // Using the viewModels() Kotlin property delegate from the fragment-ktx
    // artifact to retrieve the ViewModel.
    private val viewModel: ListViewModel by viewModels()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        viewModel.filteredList.observe(viewLifecycleOwner, Observer { list ->
            // Update the list UI.
        }
    }
}

class ChildFragment: Fragment() {
    // Using the viewModels() Kotlin property delegate from the fragment-ktx
    // artifact to retrieve the ViewModel using the parent fragment's scope
    private val viewModel: ListViewModel by viewModels({requireParentFragment()})
    ...
}
```

### [Scope a ViewModel to the Navigation Graph](https://developer.android.com/guide/fragments/communicate#scope_a_viewmodel_to_the_navigation_graph)
If you're using the Navigation library, you can also scope a `ViewModel` to the lifecycle of a destination's `NavBackStackEntry`. For example, the `ViewModel` can be scoped to the `NavBackStackEntry` for the `ListFragment`:
```
class ListFragment: Fragment() {
    // Using the navGraphViewModels() Kotlin property delegate from the fragment-ktx
    // artifact to retrieve the ViewModel using the NavBackStackEntry scope.
    // R.id.list_fragment == the destination id of the ListFragment destination
    private val viewModel: ListViewModel by navGraphViewModels(R.id.list_fragment)

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        viewModel.filteredList.observe(viewLifecycleOwner, Observer { item ->
            // Update the list UI.
        }
    }
}
```

## [Get results using the Fragment Result API](https://developer.android.com/guide/fragments/communicate#fragment-result)
In some cases, you might want to pass a one-time value between two fragments or between a fragment and its host activity. For example, you might have a fragment that reads QR codes, passing the data back to a previous fragment.

In Fragment version 1.3.0 and higher, each `FragmentManager` implements `FragmentResultOwner`. This means that a `FragmentManager` can act as a central store for fragment results. This change lets components communicate with each other by setting fragment results and listening for those results, without requiring those components to have direct references to each other. <sup>[2](https://developer.android.com/guide/fragments/communicate#kotlin:~:text=In%20some%20cases%2C%20you%20might%20want,have%20direct%20references%20to%20each%20other.)</sup>

### [Pass results between fragments](https://developer.android.com/guide/fragments/communicate#pass-between-fragments)
To pass data back to fragment A from fragment B, first set a result listener on fragment A, the fragment that receives the result. Call `setFragmentResultListener()` on fragment A's `FragmentManager`, as shown in the following example:
```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // Use the Kotlin extension in the fragment-ktx artifact.
    setFragmentResultListener("requestKey") { requestKey, bundle ->
        // We use a String here, but any type that can be put in a Bundle is supported.
        val result = bundle.getString("bundleKey")
        // Do something with the result.
    }
}
```

![](./res/fragment-a-to-b.png "Fragment B sends data to fragment A using a `FragmentManager`")

In fragment B, the fragment producing the result, set the result on the same `FragmentManager` by using the same `requestKey`. You can do so by using the `setFragmentResult()` API:
```
button.setOnClickListener {
    val result = "result"
    // Use the Kotlin extension in the fragment-ktx artifact.
    setFragmentResult("requestKey", bundleOf("bundleKey" to result))
}
```

Fragment A then receives the result and executes the listener callback once the fragment is `STARTED`.

You can have only a single listener and result for a given key. If you call `setFragmentResult()` more than once for the same key, and if the listener is not `STARTED`, the system replaces any pending results with your updated result.

If you set a result without a corresponding listener to receive it, the result is stored in the `FragmentManager` until you set a listener with the same key. Once a listener receives a result and fires the `onFragmentResult()` callback, the result is cleared. This behavior has two major implications:

- Fragments on the back stack do not receive results until they have been popped and are `STARTED`;
- If a fragment listening for a result is `STARTED` when the result is set, the listener's callback then fires immediately.

### [Pass results between parent and child fragments](https://developer.android.com/guide/fragments/communicate#pass-parent-child)
To pass a result from a child fragment to a parent, use `getChildFragmentManager()` from the parent fragment instead of `getParentFragmentManager()` when calling `setFragmentResultListener()`.
```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // Set the listener on the child fragmentManager.
    childFragmentManager.setFragmentResultListener("requestKey") { key, bundle ->
        val result = bundle.getString("bundleKey")
        // Do something with the result.
    }
}
```

![](./res/pass-parent-child.png "A child fragment can use `FragmentManager` to send a result to its parent.")

The child fragment sets the result on its `FragmentManager`. The parent then receives the result once the fragment is `STARTED`:
```
button.setOnClickListener {
    val result = "result"
    // Use the Kotlin extension in the fragment-ktx artifact.
    setFragmentResult("requestKey", bundleOf("bundleKey" to result))
}
```

### [Receive results in the host activity](https://developer.android.com/guide/fragments/communicate#receive-host-activity)
To receive a fragment result in the host activity, set a result listener on the fragment manager using `getSupportFragmentManager()`.
```
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        supportFragmentManager
                .setFragmentResultListener("requestKey", this) { requestKey, bundle ->
            // We use a String here, but any type that can be put in a Bundle is supported.
            val result = bundle.getString("bundleKey")
            // Do something with the result.
        }
    }
}
```

# Links
[Communicate with fragments](https://developer.android.com/guide/fragments/communicate)
