# Semaphore vs Monitor
Both semaphores and monitors are used to solve the critical section problem (as they allow processes to access the shared resources in mutual exclusion) and to achieve process synchronization in the multiprocessing environment.

**Monitor** type high-level synchronization construct. It is an abstract data type. The Monitor type contains shared variables and the set of procedures that operate on the shared variable. 

When any process wishes to access the shared variables in the monitor, it needs to access it through the procedures. These processes line up in a queue and are only provided access when the previous process release the shared variables. Only one process can be active in a monitor at a time. Monitor has condition variables.

**Semaphore** (`java.util.concurrent.Semaphore`) used for limit the number of threads accessing a specific resource. Conceptually, a semaphore maintains a set of permits. Each `acquire()` blocks if necessary until a permit is available, and then takes it. Each `release()` adds a permit, potentially releasing a blocking acquirer. However, no actual permit objects are used; the Semaphore just keeps a count of the number available and acts accordingly.

Advantages of Monitors:
- Monitors are easy to implement than semaphores;
- Mutual exclusion in monitors is automatic while in semaphores, mutual exclusion needs to be implemented explicitly;
- Monitors can overcome the timing errors that occur while using semaphores;
- Shared variables are global to all processes in the monitor while shared variables are hidden in semaphores.

Advantages of Semaphores:
- Semaphores are machine independent (because they are implemented in the kernel services).
- Semaphores permit more than one thread to access the critical section, unlike monitors.
- In semaphores there is no spinning, hence no waste of resources due to no busy waiting.

## Links
https://www.geeksforgeeks.org/monitor-vs-semaphore/  
https://stackoverflow.com/questions/7335950/semaphore-vs-monitors-whats-the-difference  
