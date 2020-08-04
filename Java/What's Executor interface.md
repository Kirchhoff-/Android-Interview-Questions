# Executor interface

The `java.util.concurrent` package defines three executor interfaces:

- `Executor`, a simple interface that supports launching new tasks;
- `ExecutorService`, a subinterface of `Executor`, which adds features that help manage the lifecycle, both of the individual tasks and of the executor itself;
- `ScheduledExecutorService`, a subinterface of `ExecutorService`, supports future and/or periodic execution of tasks.

Typically, variables that refer to executor objects are declared as one of these three interface types, not with an executor class type.

## The `Executor` interface 
The `Executor` interface provides a single method, execute, designed to be a drop-in replacement for a common thread-creation idiom. If `r` is a `Runnable` object, and `e` is an `Executor` object you can replace:

```
(new Thread(r)).start();
```

with:

```
e.execute(r);
```

However, the definition of execute is less specific. The low-level idiom creates a new thread and launches it immediately. Depending on the `Executor` implementation, `execute` may do the same thing, but is more likely to use an existing worker thread to run `r`, or to place `r` in a queue to wait for a worker thread to become available.

The executor implementations in `java.util.concurrent` are designed to make full use of the more advanced `ExecutorService` and `ScheduledExecutorService` interfaces, although they also work with the base `Executor` interface.

## The `ExecutorService` Interface

The `ExecutorService` interface supplements `execute` with a similar, but more versatile `submit` method. Like `execute`, `submit` accepts `Runnable` objects, but also accepts `Callable` objects, which allow the task to return a value. The `submit` method returns a `Future` object, which is used to retrieve the `Callable` return value and to manage the status of both `Callable` and `Runnable` tasks.

`ExecutorService` also provides methods for submitting large collections of `Callable` objects. Finally, `ExecutorService` provides a number of methods for managing the shutdown of the executor. To support immediate shutdown, tasks should handle interrupts correctly.

## The `ScheduledExecutorService` Interface
The `ScheduledExecutorService` interface supplements the methods of its parent `ExecutorService` with `schedule`, which executes a `Runnable` or `Callable` task after a specified delay. In addition, the interface defines `scheduleAtFixedRate` and `scheduleWithFixedDelay`, which executes specified tasks repeatedly, at defined intervals.

## ExecutorService vs. Fork/Join

After the release of Java 7, many developers decided that the `ExecutorService` framework should be replaced by the fork/join framework. This is not always the right decision, however. Despite the simplicity of usage and the frequent performance gains associated with fork/join, there is also a reduction in the amount of developer control over concurrent execution.

`ExecutorService` gives the developer the ability to control the number of generated threads and the granularity of tasks which should be executed by separate threads. The best use case for `ExecutorService` is the processing of independent tasks, such as transactions or requests according to the scheme “one thread for one task.”

In contrast, according to Oracle's documentation, fork/join was designed to speed up work which can be broken into smaller pieces recursively.

## Links
https://docs.oracle.com/javase/tutorial/essential/concurrency/exinter.html  
https://www.baeldung.com/java-executor-service-tutorial  
http://tutorials.jenkov.com/java-util-concurrent/executorservice.html
