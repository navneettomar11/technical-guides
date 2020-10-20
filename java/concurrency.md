
## Process vs Thread
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
