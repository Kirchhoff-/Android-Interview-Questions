

  ## Diffrence between blocking and suspending in Kotlin
Difference is explained assuming there are 2 functions `Function A` and `Function B`

### Blocking
`Function A` has to be completed before `Function B` continues. The thread is locked for `Function A` to complete its execution.

### Suspending
`Function A`  while it has started could be suspended and let `Function B` execute, `Function A` can be resumed later. The thread is not locked by `Function A`. `Function A` can be started, paused and resumed without interrupting the  execution of `Function B`

### Example
### Blocking

    fun main(args: Array<String>) {  
        println("Main execution started")  
        threadFunction(1, 200)  
        threadFunction(2, 500)  
        Thread.sleep(1000)  
        println("Main execution stopped")  
    }  
      
    fun threadFunction(counter: Int, delay: Long) {  
        thread {  
      println("Function ${counter} has started on ${Thread.currentThread().name}")  
            Thread.sleep(200)  
            println("Function ${counter} is finished on ${Thread.currentThread().name}")  
        }  
    }
#### Output

    Main execution started
    Function 1 has started on Thread-0
    Function 2 has started on Thread-1
    Function 1 is finished on Thread-0
    Function 2 is finished on Thread-1
    Main execution stopped
In the above program the control is as follows

 - The `main` method will invoke `threadFunction 1`and `threadFunction 2` in the main thread. Each method will create a separate thread , prints `Function has started`  along with the thread name( Thread - 0/ Thread- 1),  wait for a given time then prints `Function is finshed` along with the thread name. 
 - Finally the `main` method will wait for 1000 ms in order to wait the threads to finish and then stop executing.
 - Here important point to notice is that  **The main thread is doing nothing just waiting until all other threads complete their execution which occupies the CPU with unnecessary waiting. It would be much better if it utilises the same time executing any other tasks. In android app this program will freeze the UI as it has only one main thread as the main thread is waiting for other threads to complete its execution**.

### Suspend

        fun main(args: Array<String>) = runBlocking {  
          println("Main execution started")  
            joinAll(  
                async { suspendFunction(1, 200) },  
                async { suspendFunction(2, 500) }  ,
                async {  
      println(" Other task is working on ${Thread.currentThread().name}")  
        delay(100)  
    }
          )  
            println("Main execution stopped")  
        }  
          
        suspend fun suspendFunction(counter: Int, delay: Long) {  
            println("Coroutine ${counter} has started work on ${Thread.currentThread().name}")  
            delay(500)  
            println("Function ${counter} is finished on ${Thread.currentThread().name}")  
        }

#### Output
    Main execution started
    Coroutine 1 has started work on main
    Coroutine 2 has started work on main
    Other task is working on main
    Coroutine 1 is finished on main
    Coroutine 2 is finished on main
    Main execution stopped

In the above program the control is as follows
 - The `main` method will invoke `suspendFunction 1`and `suspendFunction 2` in the main thread. 
 -  Each `suspend Function` will be executed in the main thread , prints `Coroutine has started work` along with thread name which main thread and when `delay` is called the coroutine is suspended as call to `delay` is a suspension point , once the waiting period is finished the coroutine wakesup and prints `Coroutine is finished`. While the coroutines are suspended the other  coroutine wil starts and prints `Other task is working on main`.
 - Here important point to notice is that **While the coroutine is executing  the  main method is not blocked so that it can execute other tasks**.
 