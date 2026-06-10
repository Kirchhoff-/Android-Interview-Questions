# Feature flags

A feature flag (also known as a feature toggle or feature switch) is a technique that allows developers to enable or disable functionality without releasing a new version of the application.

Instead of permanently exposing a feature to all users, the application checks a flag and decides whether the feature should be available.

```kotlin
if (featureFlags.isNewProfileEnabled()) {
    showNewProfile()
} else {
    showOldProfile()
}
```

## Why use Feature Flags?

- **Disable features without a new release**. If a feature causes problems in production, it can be disabled immediately without releasing a new version of the application. This allows teams to react quickly while minimizing the impact on users;

- **Safely test in production**. Feature flags make it possible to expose new functionality to a small subset of users before releasing it to everyone. This allows teams to gather feedback and identify potential issues before a full rollout;

- **Separate deployment from release**. Teams can deploy code to production while keeping new functionality disabled. The feature can then be enabled only when it is ready for users;

- **A/B testing**. Different versions of a feature can be shown to different groups of users. This helps teams compare user behavior and determine which implementation produces better results.

## Challenges

- **Technical debt**. Feature flags are sometimes left in the codebase long after they are no longer needed. Over time, obsolete flags accumulate, increase maintenance costs, and make the code harder to understand;

- **Code complexity**. Every feature flag introduces additional conditional logic into the codebase. As the number of flags grows, the application flow becomes harder to understand and maintain;

- **Testing complexity**. Feature flags increase the number of possible application states. As a result, more scenarios need to be tested to ensure the application behaves correctly.

## Best practices

- **Use meaningful names**. Feature flag names should clearly describe their purpose. Avoid generic names such as `newFeatureEnabled` or `testFlag`;

- **Assign an owner**. Every feature flag should have a responsible owner who can explain why it exists and decide when it should be removed;

- **Remove obsolete flags**. Feature flags should not remain in the codebase forever. Once a feature is fully released and the old implementation is no longer needed, the flag should be removed;

- **Keep feature flags independent**. Feature flags should not depend on other feature flags. Dependencies between flags increase code complexity, make testing more difficult, and can lead to unexpected behavior.

## Android example

Feature flags can be implemented in different ways. An application can use a third-party solution, such as Firebase Remote Config, LaunchDarkly, ConfigCat, or a custom implementation where flags are loaded from the application's own backend.

Regardless of the implementation details, the usage in the application code usually looks similar:

```kotlin
class ProfileViewModel(
    private val featureFlags: FeatureFlags,
) : ViewModel() {

    fun openProfile() {
        if (featureFlags.isNewProfileEnabled()) {
            openNewProfile()
        } else {
            openLegacyProfile()
        }
    }
}
```

The important idea is to keep feature flag access behind a dedicated abstraction. This makes the application code independent from a specific feature flag provider and easier to test.

# Further reading
- [Getting Started with Feature Flags on Android](https://medium.com/@domen.lanisnik/getting-started-with-feature-flags-on-mobile-7a2a1c15bd14)

- [A guide to feature flags in mobile development](https://bitrise.io/blog/post/a-guide-to-feature-flags-in-mobile-development)

- [Feature Flags: Smaller, Faster, Better Software Development](https://medium.com/@dehora/feature-flags-smaller-better-faster-software-development-f2eab58df0f9)

- [What Are Feature Flags?](https://launchdarkly.com/blog/what-are-feature-flags/)

- [Best Practices for Feature Flagging in Mobile Development](https://jamshidbekboynazarov.medium.com/best-practices-for-feature-flagging-in-mobile-development-156551b3a328)
