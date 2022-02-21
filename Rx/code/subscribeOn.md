## Puzzle 1 - subscribeOn + observeOn
On which thread `doSomething()` will be executed?

```
Observable.just("item")
    .observeOn(Schedulers.io())
    .subscribeOn(Schedulers.computation())
    .subscribe { doSomething() }
```

 A) Main thread
 
 B) Computation
 
 C) IO
 
 D) It's not exectued
 
<details>
C) IO
</details>

## Puzzle 2 - 2 subscribeOn
On which thread `doSomething()` will be executed?

```
Observable.just("item")
    .subscribeOn(Schedulers.computation())
    .subscribeOn(Schedulers.io())
    .subscribe { doSomething() }
```

 A) Main thread
 
 B) Computation
 
 C) IO
 
 D) It's not exectued
 
<details>
B) Computation
</details>

## Puzzle 3 - subscribeOn + 2 doOnSubscribe
On which order `foo()` and `bar()` will be executed?

```
Observable.just("item")
    .doOnSubscribe { foo() }
    .doOnSubscribe { bar() }
    .subscribeOn(Schedulers.computation())
    .subscribe()
```

A) `foo()` then `bar()`

B) `bar()` then `foo()`

C) only `foo()` is executed

D) It’s not executed

<details>
A) foo() then bar()
</details>

## Puzzle 4 - 2 subscribeOn + 2 doOnSubscribe
On which order `foo()` and `bar()` will be executed?

```
Observable.just("item")
    .doOnSubscribe { foo() }
    .subscribeOn(Schedulers.computation())
    .doOnSubscribe { bar() }
    .subscribeOn(Schedulers.computation())
    .subscribe()
```

A) `foo()` then `bar()`

B) `bar()` then `foo()`

C) only `foo()` is executed

D) only `bar()` is executed

<details>
B) bar() then foo()
</details>

## Puzzle 5 - 3 subscribeOn + 2 doOnSubscribe
On which thread `foo()` and `bar()` will be executed?

```
Observable.just("item")
    .subscribeOn(Schedulers.single())
    .doOnSubscribe { foo() }
    .subscribeOn(Schedulers.computation())
    .doOnSubscribe { bar() }
    .subscribeOn(Schedulers.io())
    .subscribe()
```

A) `foo()` on Single then `bar()` on Computation

B) `bar()` on IO then `foo()` on Computation

C) `bar()` on Computation then `foo()` on Single

D) I give up

<details>
B) bar() on IO then foo() on Computation
</details>

## Puzzle 6 - subscribeOn + timer
On which thread `doSomething()` will be executed?

```
Observable.timer(10, TimeUnit.MILLISECONDS)
    .subscribeOn(Schedulers.io())
    .subscribe { doSomething() }
```

A) `doSomething` is executed on IO

B)  `doSomething` is executed on Single

C)  `doSomething` is executed on Computation

D)  `doSomething` is executed on main

<details>
C) doSomething is executed on Computation
</details>

## Puzzle 7 - subscribeOn + Subject
On which thread `doSomething()` will be executed?

```
val subject = BehaviorSubject.create<String>()

Observable.just(0)
    .observeOn(Schedulers.io())
    .subscribe { subject.onNext("any") }

Thread.sleep(10)

subject.subscribe { doSomething(it) }
```

A) `doSomething` is executed on main

B) `doSomething` is executed on Computation

C) `doSomething` is not executed

D) `doSomething` is executed on io

<details>
A) doSomething is executed on main
</details>

# Links 
[RxJava2 puzzle: Do you understand subscribeOn?](https://www.glureau.com/2020/05/01/RxJava-Puzzle-SubscribeOn/)

# Further reading 
[RxJava2 subscribeOn: How to use it](https://www.glureau.com/2020/05/02/RxJava-SubscribeOn-HowTo/)
