# Livelock
A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then livelock may result. As with deadlock, livelocked threads are unable to make further progress. However, the threads are not blocked — they are simply too busy responding to each other to resume work. In livelock, two or more threads keep on transferring states between one another, the threads are not able to perform their respective tasks.

Example:

Suppose we have class `CommonResource`:
```
public class CommonResource {

    private Worker owner;

    public CommonResource(Worker d) {
        owner = d;
    }

    public Worker getOwner() {
        return owner;
    }

    public synchronized void setOwner(Worker d) {
        owner = d;
    }
}
```

And class `Worker`:
```
public class Worker {
    private String name;
    private boolean active;

    public Worker(String name, boolean active) {
        this.name = name;
        this.active = active;
    }

    public String getName() {
        return name;
    }

    public boolean isActive() {
        return active;
    }

    public synchronized void work(CommonResource commonResource, Worker otherWorker) {
        while (active) {
            // wait for the resource to become available.
            if (commonResource.getOwner() != this) {
                try {
                    wait(10);
                } catch (InterruptedException e) {
                    //ignore
                }
                continue;
            }

            // If other worker is also active let it do it's work first
            if (otherWorker.isActive()) {
                System.out.println(getName() +
                        " : handover the resource to the worker " +
                        otherWorker.getName());
                commonResource.setOwner(otherWorker);
                continue;
            }

            //now use the commonResource
            System.out.println(getName() + ": working on the common resource");
            active = false;
            commonResource.setOwner(otherWorker);
        }
    }
}
```

Here we will get livelock cause two workers active from start. Two threads want to access a shared common resource via a Worker object but when they see that other Worker (invoked on another thread) is also 'active', they attempt to hand over the resource to other worker and wait for it to finish.
```
public class Livelock {

    public static void main (String[] args) {
        final Worker worker1 = new Worker("Worker 1 ", true);
        final Worker worker2 = new Worker("Worker 2", true);

        final CommonResource s = new CommonResource(worker1);

        new Thread(() -> {
            worker1.work(s, worker2);
        }).start();

        new Thread(() -> {
            worker2.work(s, worker1);
        }).start();
    }
}
```

Output:
```
Worker 1 : handing over the resource to the worker: Worker 2
Worker 2 : handing over the resource to the worker: Worker 1
Worker 1 : handing over the resource to the worker: Worker 2
Worker 2 : handing over the resource to the worker: Worker 1
Worker 1 : handing over the resource to the worker: Worker 2
Worker 2 : handing over the resource to the worker: Worker 1
```

To avoid a livelock, we need to look into the condition that is causing the livelock and then come up with a solution accordingly.

For example, if we have two threads that are repeatedly acquiring and releasing locks, resulting in livelock, we can design the code so that the threads retry acquiring the locks at random intervals. This will give the threads a fair chance to acquire the locks they need.

## Links
https://www.baeldung.com/java-deadlock-livelock  
https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html  
https://www.logicbig.com/tutorials/core-java-tutorial/java-multi-threading/thread-livelock.html
