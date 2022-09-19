# Coroutine Job
Conceptually, a `Job` is a cancellable thing with a lifecycle that culminates in its completion. For every coroutine that is created, a `Job` instance is returned to uniquely identify that coroutine and allow you to manage its lifecycle. 

The most basic instances of Job interface are created like this:
- **Coroutine job** is created with `launch` coroutine builder. It runs a specified block of code and completes on completion of this block.
- **CompletableJob** is created with a `Job()` factory function. It is completed by calling `CompletableJob.complete`.

Jobs can be arranged into parent-child hierarchies where cancellation of a parent leads to immediate cancellation of all its children recursively. Failure of a child with an exception other than `CancellationException` immediately cancels its parent and, consequently, all its other children. 

## Job states
| State | isActive | isCompleted | isCancelled |
|---|---|---|---|
| *New* (optional initial state) | `false` | `false` | `false` |
| *Active* (default initial state) | `true` | `false` | `false` |
| *Completing* (transient state) | `true` | `false` | `false` |
| *Cancelling* (transient state) | `false` | `false` | `true` |
| *Cancelled* (final state) | `false` | `true` | `true` |
| *Completed* (final state) | `false` | `true` | `false` |

Usually, a job is created in the *active* state (it is created and started). However, coroutine builders that provide an optional `start` parameter create a coroutine in the *new* state when this parameter is set to `CoroutineStart.LAZY`. Such a job can be made *active* by invoking `start` or `join`.

A job is *active* while the coroutine is working or until `CompletableJob` is completed, or until it fails or cancelled.

Failure of an *active* job with an exception makes it *cancelling*. A job can be cancelled at any time with `cancel` function that forces it to transition to the *cancelling* state immediately. The job becomes *cancelled* when it finishes executing its work and all its children complete.

Completion of an *active* coroutine's body or a call to `CompletableJob.complete` transitions the job to the *completing* state. It waits in the *completing* state for all its children to complete before transitioning to the *completed* state. Note that *completing* state is purely internal to the job. For an outside observer a *completing* job is still active, while internally it is waiting for its children.

```
                                          wait children
    +-----+ start  +--------+ complete   +-------------+  finish  +-----------+
    | New | -----> | Active | ---------> | Completing  | -------> | Completed |
    +-----+        +--------+            +-------------+          +-----------+
                     |  cancel / fail       |
                     |     +----------------+
                     |     |
                     V     V
                 +------------+                           finish  +-----------+
                 | Cancelling | --------------------------------> | Cancelled |
                 +------------+                                   +-----------+
```

A `Job` instance in the `coroutineContext` represents the coroutine itself.

## Cancellation cause
A coroutine job is said to *complete exceptionally* when its body throws an exception; a `CompletableJob` is completed exceptionally by calling `CompletableJob.completeExceptionally`. An exceptionally completed job is cancelled and the corresponding exception becomes the *cancellation cause* of the job.

Normal cancellation of a job is distinguished from its failure by the type of this exception that caused its cancellation. A coroutine that threw `CancellationException` is considered to be *cancelled normally*. If a cancellation cause is a different exception type, then the job is considered to have *failed*. When a job has *failed*, then its parent gets cancelled with the exception of the same type, thus ensuring transparency in delegating parts of the job to its children.

Note, that `cancel` function on a job only accepts `CancellationException` as a cancellation cause, thus calling `cancel` always results in a normal cancellation of a job, which does not lead to cancellation of its parent. This way, a parent can `cancel` its own children (cancelling all their children recursively, too) without cancelling itself.

## [SupervisorJob](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-supervisor-job.html)
hildren of a supervisor job can fail independently of each other.

A failure or cancellation of a child does not cause the supervisor job to fail and does not affect its other children, so a supervisor can implement a custom policy for handling failures of its children:
- A failure of a child job that was created using `launch` can be handled via `CoroutineExceptionHandler` in the context;
- A failure of a child job that was created using `async` can be handled via `Deferred.await` on the resulting deferred value.

If parent job is specified, then this supervisor job becomes a child job of its parent and is cancelled when its parent fails or is cancelled. All this supervisor's children are cancelled in this case, too. The invocation of `cancel` with exception (other than `CancellationException`) on this supervisor job also cancels parent.

# Links
[Job](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-job/)

[Coroutines (Part II) – Job, SupervisorJob, Launch and Async](https://victorbrandalise.com/coroutines-part-ii-job-supervisorjob-launch-and-async/)

[SupervisorJob](https://kotlinlang.org/api/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-supervisor-job.html)

# Further reading
[Jobs, Waiting, Cancellation in Kotlin Coroutines](https://www.geeksforgeeks.org/jobs-waiting-cancellation-in-kotlin-coroutines/)
