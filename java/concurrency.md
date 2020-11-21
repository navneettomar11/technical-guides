# Process vs Thread
A program in execution is often referred as process. A thread is subset(part) of the process.

A process consists of multiple threads. A thread is smallest part of the process that can execute concurrently with other parts (threads) of the process.

A process is sometime referred as task. A thread is often referred as lightweight process.

A process has its own address space. A thread uses the process's address space and share it with the other threads of that process.

A thread can communicate with other thread (of the same process) directly using method like wait(), notify(), notifyAll(). A process can communicate with other process by using inter-process communication.

# Multithreading
Multithreading means that you have multiple threads of execution inside the same application. A thread is like a separate CPU executing your application. Thus, a multithreaded application is like an application that has multiple CPU executing different parts of the code at the same time.

A thread is not equal to a CPU though. Usually a single CPU will share its execution time among multiple threads, switching between executing each of the threads for a given amount of time. It is also possible to have the threads of an application be executed by different CPUs.

## Why Multithreading
There are several reasons as to why one would use multithreading in an application. Some of the most common reasons for multithreading are:
- Better utilization of a single CPU.
- Better utilization of multiple CPUs or CPU cores.
- Better user experience with regards to responsiveness.
- Better user experience with regards to fairness.

# Concurrency Models
The first Java concurrency model assumed that multiple threads executing withing the same application would also share objects. This type of concurrency model is typical referred to as a "shared state concurrency model". A lot the of the concurrency language constructs and utilities are designed to support this concurrency model.

However, a lot has happened in the world of concurrent architecture and design since the first Java concurrency books were written, and even since the Java 5 concurrency utilities were released. 

The shared state concurrency model causes a lot of concurrency problems which can be hard to solve elegantly. Therefore, an alternative concurrency model referred to as "shared nothing" or "separate state" has gained popularity. In the separate state concurrency model the thread do not share any object or data. This avoid a lot of the concurrent access problems of the shared state concurrency model.

New asynchronous "separate state" platforms and toolkits like Netty, Vert.x and Play/Akka and Qbit have emerged. New non-blocking concurrency algorithms have been published and new non-blocking tools kike the LMax Disrupter have been added to our toolkits. New functional programming parallelism have been introduced with the `Fork and Join Framework` in Java 7. and the collection streams API in Java 8 API.

# Shared State vs Separate State
One important aspect of a concurrency model is, whether the components and threads are designed to share state among the threads, or to have separate state which is never shared among the threads.

Shared state means that the different threads in the system will share some state among them. By state is meant some data, typically one or more objects or similar. When threads share state problems like `race conditions` and `deadlock` etc may occur. It depends on how the thread use and access the shared objects, of course.

Separate state means that different thread in the system do not share any state among them. In case the different threads need to communicate they do so either by exchanging immutable objects among them, or by sending copies of objects (or data) among them. Thus, when no two thread write to the same object(data/state) you can avoid most of the common concurrency problems.

Using a separate state concurrency design can often make some parts of the code easier to implement and easier to reason about, since you know that only one thread will ever write to a given object. You don't have to worry about since you know that only one thread will ever write to a given object. You don't have to worry about concurrent access to that object. However, you might have to think a bit harder about the application design in the big picture, to use separate state concurrency. It's worth it though I feel. Personally I prefer separate state concurrency design.

# Thread Safety


# Parallel Workers
The first concurrency model is what I call the parallel worker model. Incoming jobs are assigned to different workers. 

In the parallel worker concurrency model a delegator distributes the incoming jobs to different workers. Each worker completed the full job. The worker work in parallel, running in different threads, and possibly on different CPUs.

If the parallel worker model was implemented in a car factory, each car would be produced by one worker. The worker would get the specification of the car to build everthing from start to end.

The parallel worker concurrency model is the most commonly used concurrency model in Java applications. Many of the concurrency utilities in the `java.util.concurrent` Java packages are designed for use with this model. You can also see traces of this model in the design of the Java Enterprise edition application servers.

# Assembly line
The second concurrency model is what I call the assembly line concurrency model I chose that name just to fit with the "parallel worker" metaphor for earlier. Other developer use names (e.g. reactive systems, or event driven systems) depending on the platform/community.

The worker are organized like workers at an assembly line in a factory. Each worker only performs a part of the full job. When that part is finished the worker forwards the job to the next worker.

Each worker is running in its own thread and shares no state with other workers. This is also sometimes referred to as a shared nothing concurrency model.

# Functional Parallelsim
Functional parallelism is third concurrency model which is being talked about a lot these days.
The basic idea of functional parallelism is that you implement your program using function calls. Functions can be seen as "agents" or "actors" that send messages to each other, just like in the assembly line concurrency model(AKA reactive or event dirven systems). When one function calls another, that is similar to sending a message.

All parameters passed to the function are copied, so no entity outside the receiving function can manipulate the data. This copying is essential to avoid race conditions on the shared data. This makes the functional execution similar to an atomic operation. Each function call can be executed independently of any other function call.

When each function call can be executed independently, each function call can be executed on separate CPUs. That means, that an algorithm implemented functionality can be executed in parallel, on multiple CPUs.

With Java 7 we got this _java.util.concurrent_ package contains the ForkAndJoinPool which can help you implement something similar to functional parallelism. With Java 8 we got parallel streams which can help you parallelize the iteration of large collections. Keep in mind that there are developers who are critical of the ForkAndJoinPool.

# Concurrency vs Parallelism
Concurrency means that an application is making progress on more than on task at the same time. Well if the computer only has one CPU the application may not make progress on more than one task at exactly the same time, but more than one task is being processed at a time inside the application. It does not completely finish one task before it begins the next. Instead the CPU switches between the different tasks until the tasks are complete.

Multiple task make progress at the same time.

Parallelism means that an application splits its task up into smaller subtask which can be processed in parallel, for instance on multiple CPUs at the exact same time.
Each task is broken into subtasks which can be processed in parallel.

# Creating and Starting Java Threads
Creating a thread is Java is done like this:

```java
Thread thread = new Thread();
```

To start the Java thread you will call its start() method like this:
```java
thread.start();
```
There are two ways to specify what code the thread should execute. The first is to create a subclass of Thread and override the `run()` method. The second method is to pass an object that implements `Runnable` to the Thread constructor. 

## Subclass or Runnable ?
There are no rules about which of the two methods that is the best. Both methods works. Personally thought, I prefer implementing Runnable and handle an instance of the implementation to a Thread instance. When have the Runnable executed by the thread pool it is easy to queue the Runnable instance until a thread from the pool is idle. This is a litter harder to do with the Thread subclasses.

Sometimes you may have to implement Runnable as well as subclass Thread. For instance, if creating a subclass of Thread that can execute more than one Runnable. This is typically the case when implementing a thread pool.


# Volatile Field
A volatile field has special properties to the Java Memory model. The reads and write of a volatile variables are synchronization actions, meaning that they have a totla ordering(all threads will observe a consistent order of these actions). A read of volatile variable is guranteed to observe the last write to this variable, according to this order.

If you have field that is accessed from multiple thread, with at least one thread writing to it, then you should consider making it volatile or else there is a little guarantee to what a certain thread would read from this field.

# ThreadLocal
Java ThreadLocal is used to create thread local variables. We know that all threads of an Object share it's variable, so the variable is not thread safe. We can use synchronization for thread safety but if we want to avoid synchronization, we can use `ThreadLocal` variables.

Every thread has it's own `ThreadLocal` variable and they can use it's get() and set() method to get the default value or change it's value local to Thread.

ThreadLocal instances are typically private static fields in classes that wish to associate state with a thread.

```java

package com.journaldev.threads;

import java.text.SimpleDateFormat;
import java.util.Random;

public class ThreadLocalExample implements Runnable{

    // SimpleDateFormat is not thread-safe, so give one to each thread
    private static final ThreadLocal<SimpleDateFormat> formatter = new ThreadLocal<SimpleDateFormat>(){
        @Override
        protected SimpleDateFormat initialValue()
        {
            return new SimpleDateFormat("yyyyMMdd HHmm");
        }
    };
    
    public static void main(String[] args) throws InterruptedException {
        ThreadLocalExample obj = new ThreadLocalExample();
        for(int i=0 ; i<10; i++){
            Thread t = new Thread(obj, ""+i);
            Thread.sleep(new Random().nextInt(1000));
            t.start();
        }
    }

    @Override
    public void run() {
        System.out.println("Thread Name= "+Thread.currentThread().getName()+" default Formatter = "+formatter.get().toPattern());
        try {
            Thread.sleep(new Random().nextInt(1000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //formatter pattern is changed here by thread, but it won't reflect to other threads
        formatter.set(new SimpleDateFormat());
        
        System.out.println("Thread Name= "+Thread.currentThread().getName()+" formatter = "+formatter.get().toPattern());
    }

}
```
# Atomic Operation
Atomic operations are performed in a single unit of task without interference from other operations. Atomic operations are necessity n multithread environment to avoid data inconsistency.

i++ is not an atomic operation. So by the time one thread read its value and increment it by one, another thread has read the older value leading to the wrong result.

To solve this issue, we have to make sure the increment operation on count is atomic, we can do that using Synchronization but Java 5 java.util.concurrent.atomic provided wrapper classes for int and long that can be used to achieve this atomically without the usage of Synchronization. 

# Lock Interface
Lock interface provides more extensive locking operations than can be obtained using synchronization method and statements. They allow more flexible structuring may have quite different properties and may support multiple associated condition objects.
The advantage of a lock are
- It's possible to make them fair.
- It's possible to make thread responsive to interruption while waiting on a Lock object.
- Its possible to try to accquire the lock but return immediately or after a timeout if the lock can't be acquired.
- Its possible to acquire and release locks in different scopes and in different orders.

# What is the Executor Framework?
The Executor Framework contains a bunch of components that are used to efficiently manage worker threads. The Executor API de-couples the execution task from the actual task to be executed via Executors. The design is one of the implementation of the `Producer Consumer` pattern.

# Executor
Executor and ExecutorService are two related interfaces of _java.util.concurrent_ framework. Executor is a very simple interface with a single execute method accepting Runnable instance for execution. In most cases, this is the interface that your task-executing code should depend on.

# ExecutorServices
ExecutorService extends the Executor interface with multiple methods for handling and checking the lifecycle of a concurrent task  execution service(termination of tasks in case of shutdown) and methods for more complex asynchronous task handling including Futures.

The Java ExecutorService interface `java.util.concurrent.ExecutorService` represents an asynchronous execution mechanism which is capable of executing tasks concurrently in the background. 

![A thread delegating a task to an ExecutorService for asynchronous execution](http://tutorials.jenkov.com/images/java-concurrency-utils/executor-service.png)

Once the thread has delegated the task to the ExecutorService, the thread continues its own execution independent of the execution of that task. The ExecutorService then executes the task concurrently independently of the thread that submitted the task.

```java
ExecutorService executorService = Executors.newFixedThreadPool(10);

executorService.execute(new Runnable() {

    public void run() {
        System.out.println("Asynchronous task");
    }
});

executorService.shutdown();
```
First an ExecutorService is created using the Executors newFixedThreadPool() factory method. This creates a thread pool with 10 threads executing tasks.

Second, an anonymous implementation of the Runnable interface is passed to the execute() method. This causes the Runnable to be executed by one of the threads in the ExecutorService.

The ExecutorService has interface has three standard implementations:
- **ThreadPoolExecutor** for executing tasks using a pool of threads. Once a thread is finished executing the task it goes back into the pool. If all threads in the pool are busy, then task has to wait for its turn.
- **ScheduledThreadPoolExecutor** allows to schedule task execution instead of running it immediately when thread is available. It can also schedule tasks with fixed rate or fixed delay.
- **ForkJoinPool** is a special ExecutorService for dealing with recursive algorithms task. If you use a regular ThreadPollExecutor for a recursive algorithm,  you will quickly find all your threads are busy waiting for the lower levels of recursion to finish. The ForkJoinPool implements the so-called work-stealing algorithm that allows it to use available thread more efficiently.

# Future interface
The submit() and invokeAll() methods return an object or a collection of objects of type Future, which allows us to get the result of a tasks execution or check the task's status (it is running or executed).

The Future interface provides a special blocking method get() which returns an actual result of the Callback task's execution or null in the case of Runnable task. Calling the get() method while the task is still running will cause execution to block until the task is properly executed and the result is available.

```java
Future<String> future = executorService.submit(callableTask);
String result = null;
try{
    result = future.get();
}catch(InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
```

With very long blocking caused by the get() method, an application's performance can degrade. If the resulting data is not crucial, it is possible to avoid such a problem by using timeouts.

```java
String result = future.get(200, TimeUnit.MILLISECONDS);
```

If the execution period is longer than specified (in this case 200 milliseconds), a TimeoutException will be thrown.

The isDone() method can be used to check if the assigned task is already processed or not.

The Future interface also provides for the cancellation of task execution with the cancel() method and to check the cancellation with isCancelled() method.

```java
boolean canceled = future.cancel(true);
boolean isCancelled = future.isCancelled();
```

# CountDownLatch
A java.util.concurrent.CountDownLatch is a concurreny construct that allows one or more threads to wait for a given set of operations to complete.

A CountdownLatch is intializaed with a given count. This count is decremented by calls to the countDown() method. Thread waiting for this count to reach zero can call one of the await() methods. Callings await() blocks the thread until the count reaches zero.

Below is a simple example. Ater the Decrementer has called countDown() 3 times on the CountDownLatch the waiting Waiter is released from the await() call.

```java
CountDownLatch latch = new CountDownLatch(3);

Waiter      waiter      = new Waiter(latch);
Decrementer decrementer = new Decrementer(latch);

new Thread(waiter)     .start();
new Thread(decrementer).start();

Thread.sleep(4000);

public class Waiter implements Runnable{

    CountDownLatch latch = null;

    public Waiter(CountDownLatch latch) {
        this.latch = latch;
    }

    public void run() {
        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Waiter Released");
    }
}

public class Decrementer implements Runnable {

    CountDownLatch latch = null;

    public Decrementer(CountDownLatch latch) {
        this.latch = latch;
    }

    public void run() {

        try {
            Thread.sleep(1000);
            this.latch.countDown();

            Thread.sleep(1000);
            this.latch.countDown();

            Thread.sleep(1000);
            this.latch.countDown();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

# Interview Questions
## Describe the Different State of a Thread and When Do the State Transitions Occur.
The state of a Thread can be checked using the Thread.getState() method. Different state of a Thread are described in the Thread.State enum. They are.
- **NEW** - a new Thread instance that was not yet start via Thread.start().
- **RUNNABLE** - a running thread. It is called runnable because at any given time it could be either running or waiting for the next quantum of time from the thread scheduler. A NEW thread enter the RUNNABLE state when you call _Thread.start()_ on it.
- **BLOCKED** - a running thread become blocked if it needs to enter a synchronized section but cannot do that due to another thread holding the monitor of this section.
- **WAITING** - a thread enter this state if it waits for another thread to perform a particular action. For instance a thread enters this state upon calling the Object.wait() method on a monitor it holds, or Thread.join() method on another method.
- **TIME_WAITING** - sames as the above, but a thread enters this state after calling timed versions of Thread.sleep, Object.wait(), Thread.join and some other methods.
- **TERMINATED** - a thread has completed the execution of its Runnable.run method and terminated.

## What is difference Between the Runnable and Callable interfaces? How are they used?
The Runnable interface has a single `run` method, it represent a unit of computation that has to be run in separate thread. The Runnable interface does not allow this method to return value or to throw unchecked expception.

The Callable interface has a single `call` method and represent a task have a value. That's why the call method return a value. It can also throw exception. Callable is generally used in ExecutorService instances to start an asynchronous task and them call the returned Future instance to get its value.
