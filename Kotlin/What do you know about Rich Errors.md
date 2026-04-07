# Rich Errors
Rich Errors is an upcoming Kotlin language feature that brings a new, type-safe way of handling errors. It allows developers to define functions that can return either a `success` value or a specific set of error types (e.g., `NetworkError` | `TimeoutError`). This approach removes the need for workarounds such as sealed classes or `null`-based error handling.

The `|` operator represents a union type, meaning the function can return one of several possible types. In the context of Rich Errors, this allows combining a success type with a predefined set of error types directly in the return type, making all possible outcomes explicit at the API level: <sup>[1](https://carrion.dev/en/posts/kotlin-24-rich-errors/#:~:text=//%20Basic%20API%20with,and%20try%20again%22\)%0A%7D)</sup>
```
// Basic API with a union return
fun fetchUser(id: UserId): User | NotFound | NetworkFailure

// Suspending API with several domain errors
suspend fun uploadAvatar(file: ImageFile): Url | UnsupportedFormat | QuotaExceeded | NetworkFailure

// Propagating unions across layers
fun loadProfile(id: UserId): Profile | NotFound | NetworkFailure | DecodeError
```

At the call site, you handle the union with a when expression and smart‑casts:
```
when (val r = fetchUser(currentId)) {
    is User -> showProfile(r)
    is NotFound -> showMissingUser()
    is NetworkFailure -> showOfflineMessage(r)
}
```

You can also map a union to UI state or another union, preserving exhaustiveness:
```
fun toUi(result: AuthToken | InvalidCredentials | NetworkFailure): UiState = when (result) {
    is AuthToken -> UiState.Authenticated(result)
    is InvalidCredentials -> UiState.Error("Invalid email or password")
    is NetworkFailure -> UiState.Error("Check your connection and try again")
}
```

## Motivation for Rich Errors
Traditional exception-based error handling is powerful but has some drawbacks:<sup>[2](https://carrion.dev/en/posts/kotlin-24-rich-errors/#:~:text=Traditional%20exception%2Dbased,can%20be%20uneven)</sup>
- **Hidden control flow**: exceptions don’t appear in function signatures;
- **Mixed concerns**: thrown types are not always explicit or structured;
- **Composition friction**: composing results across layers often requires `try`/`catch`;
- **Multiplatform nuances**: mapping exceptions across different platforms can be uneven.

Kotlin already offers `Result<T>` and functional helpers that address part of this. Rich errors extend the idea: make failure channels explicit, typed, and composable — while keeping excellent interop with existing code.<sup>[3](https://carrion.dev/en/posts/kotlin-24-rich-errors/#:~:text=Kotlin%20already%20offers%20Result%3CT%3E%20and%20functional%20helpers%20that%20address%20part%20of%20this.%20Rich%20errors%20extend%20the%20idea%3A%20make%20failure%20channels%20explicit%2C%20typed%2C%20and%20composable%20%E2%80%94%20while%20keeping%20excellent%20interop%20with%20existing%20code.)</sup>

Benefits of Rich Errors:<sup>[4](https://computas.com/blogg/exploring-rich-errors-in-kotlin-a-game-changer-from-kotlinconf-2025/#:~:text=Improved%20Type%20Safety,and%20improving%20clarity.)</sup>
- **Improved Type Safety**: By integrating error types directly into the return type, rich errors ensure that all possible error cases are accounted for at compile time, reducing runtime surprises;
- **Conciseness**: Rich errors eliminate the need for wrapper classes like `Result` and provide explicit error types instead of `null`;
- **Enhanced Readability**: The `|` syntax clearly communicates possible outcomes, and the type system ensures all cases are handled at compile time, reducing boilerplate and improving clarity.

## [Real-World Example](https://xuanlocle.medium.com/kotlin-2-4-introduces-rich-errors-a-game-changer-for-error-handling-413d281e4a05#:~:text=Real%2DWorld%20Example%3A%20Before,credentials.%22\)%0A%20%20%20%20%7D%0A%7D)
The old way (`try`-`catch`):
```
fun loadUserData() {
    try {
        val user = fetchUserLegacy()
        show(user)
    } catch (e: NetworkError) {
        showError("Try again later, network issue.")
    } catch (e: AnotherException) {
        showError("Something else went wrong.")
    }
}
```

The Rich Error way:
```
fun fetchUser(): User | AppError {
    if (/* network fails */ false) return NetworkError(503)
    if (/* user not found */ false) return UserNotFoundError
    return User("123", "Ada")
}

fun loadUserDataNew() {
    val result = fetchUser()
    when (result) {
        is User -> show(result)
        is NetworkError -> showError("Network issue (${result.code}). Try again.")
        is UserNotFoundError -> showError("User not found. Please check your credentials.")
    }
}
```

# Links
[Kotlin 2.4 Rich Errors: What They Are and How to Prepare](https://carrion.dev/en/posts/kotlin-24-rich-errors/)

[Exploring Rich Errors in Kotlin: A Game-Changer from KotlinConf 2025](https://computas.com/blogg/exploring-rich-errors-in-kotlin-a-game-changer-from-kotlinconf-2025/)

[Kotlin 2.4 Introduces Rich Errors — A Game Changer for Error Handling](https://xuanlocle.medium.com/kotlin-2-4-introduces-rich-errors-a-game-changer-for-error-handling-413d281e4a05)

# Further reading
[Rich Errors, aka Error Union Types: Motivation and Rationale](https://github.com/Kotlin/KEEP/blob/main/proposals/KEEP-0441-rich-errors-motivation.md)

[Rich Errors](https://resources.jetbrains.com/storage/products/kotlinconf-2025/may-22/Rich%20Errors%20in%20Kotlin%20_%20Michail%20Zarec%CC%8Censkij.pdf)
