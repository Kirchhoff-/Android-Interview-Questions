# `Activity` vs `Fragment`
`Activity` is a crucial component of an application that provides a user interface for interacting with the app. Each `Activity` represents a single screen with which users can engage, similar to a window in a desktop application. `Activity` can serve as a host for fragments and `@Composable` functions, and it can also have its own UI.

A `Fragment` represents a reusable portion of your app's UI. A fragment defines and manages its own layout, has its own lifecycle, and can handle its own input events. Fragments can't live on their own. They must be *hosted* by an activity or another fragment. The fragment’s view hierarchy becomes part of, or *attaches* to, the host’s view hierarchy.
<sup>[1](https://developer.android.com/guide/fragments#samples:~:text=A%20Fragment%20represents,host%E2%80%99s%20view%20hierarchy.)</sup>

| `Activity`  | `Fragment` |
|---|---|
| A standalone component that doesn’t depend on anything else  |  A reusable UI component that must be hosted within an `Activity` or another `Fragment` |
| Has its own independent lifecycle managed by the OS  | Has a lifecycle tied to the hosting `Activity` but with additional lifecycle states  |
| Must be registered in the `AndroidManifest.xml` file  | Does not need to be registered anywhere  |
| Can be launched from other applications  | Cannot be launched from other applications  |
| Data transfer between activities requires serialization  |  Data transfer between fragments does not require serialization  |
| Can be resource-intensive  | More lightweight, as multiple fragments can exist within a single `Activity`  |
| Managed by the system’s back stack  | Managed by the `FragmentManager` back stack within an `Activity` or another `Fragment` |

# Links
[Introduction to activities](https://developer.android.com/guide/components/activities/intro-activities)

[Fragments](https://developer.android.com/guide/fragments)

# Next questions
[How to communicate with fragments?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/How%20to%20communicate%20with%20fragments.md)

[What is Jetpack Compose?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/Android/What%20is%20Jetpack%20Compose.md)
