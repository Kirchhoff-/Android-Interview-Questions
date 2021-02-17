# @Binds vs @Provides
`@Provides`, the most common construct for configuring a binding, serves three functions:
- Declare which type (possibly qualified) is being provided — this is the return type;
- Declare dependencies — these are the method parameters;
- Provide an implementation for exactly how the instance is provided — this is the method body.

While the first two functions are unique and critical to every `@Provides` method, the third can often be tedious and repetitive. So, whenever there is a `@Provides` whose implementation is simple and common enough to be inferred by Dagger, it makes sense to just declare that as a method without a body (an abstract method) and have Dagger apply the behavior.

But, if we were to just say that abstract `@Provides` methods should be treated as we do for `@Binds` methods, the specification of `@Provides` would basically be two specifications with a bunch of conditional logic. For example, a `@Provides` method can have any number of parameters of any type, but a `@Binds` method can only have a single parameter whose type is assignable to the return type. Separating those specifications makes it easier to reason about correctness because the annotation determines the constraints.

## Why can’t @Binds and instance @Provides methods go in the same module?
Because `@Binds` methods are *just* a method *declaration*, they are expressed as `abstract` methods — no implementation is ever created and nothing is ever invoked. On the other hand, a `@Provides` method *does* have an implementation and *will* be invoked.

Since `@Binds` methods are never implemented, no concrete class is ever created that implements those methods. However, instance `@Provides` methods *require* a concrete class in order to construct an instance on which the method can be invoked.

**What do I do instead?**

The easiest change is to make the provides method `static`. In addition to being compatible with `@Binds`, they often perform better than instance provides methods. If the method *must* be an instance method (e.g. returns a value from a field), the easiest fix is to separate your `@Provides` methods and `@Binds` methods into two separate modules and include one from the other. A simple example that provides an `HttpServletRequest` and binds `ServletRequest` might look like:

```
@Module(includes = Declarations.class)
final class HttpServletRequestModule {
  @Module
  interface Declarations {
    @Binds ServletRequest bindServletRequest(HttpServletRequest httpRequest);
  }

  private final HttpServletRequest httpRequest;

  HttpServletRequestModule(HttpServletRequest httpRequest) {
    this.httpRequest = httpRequest;
  }
}
```

Unfortunately, `@Binds` only works based on the rules below:
```
@Binds methods must have only one parameter whose type is assignable to the return type
```
So only a single parameter, and the type return is typically the interface of the given parameter object.

So we can replace `@Provide` annotation like this:
```
@Provides
public HomePresenter provideHomePresenter() {
    return new HomePresenterImp();
}
```

to `@Binds`:
```
@Binds abstract HomePresenter bindHomePresenter(HomePresenterImp presenter);
```

# Links
[Frequently Asked Questions](https://dagger.dev/dev-guide/faq.html)  
[Dagger 2 @Binds vs @Provides](https://medium.com/mobile-app-development-publication/dagger-2-binds-vs-provides-cbf3c10511b5)  
[What is the use case for @Binds vs @Provides annotation in Dagger2](https://stackoverflow.com/a/52618571)
