# Java 9 Features

## 1. Java 9 REPL (JShell)
Oracle Corp has introduced a new tool called "jshell". It stands for Java Shell and also knows as REPL(Read, Evaluate, Print Loop). It is used to execute and test any Java Construct like class, interface, enum, object, statement etc, very easily.

## 2. Factory Methods for Immutable List, Set, Map and Map.Entry
Oracle Corp has introduced some convenient factory methods to create Immutable List, Set, Map and Map.Entry. These utility methods are used to create empty or non-empty Collection objects.
In Java SE 8 and earlier versions, We can use Collections class utility methods like `unmodifiableXXX` to create Immuate Collection objects. For instance, if we want to create an Immutable List, then we can use `Collection.unmodifiableList` method.
However, these `Collection.unmodifiableXXX` methods are a tedious and verbose approach. To overcome those shortcomings, Oracle Corp has added a couple of utility methods to List, Set and Map interfaces.
List and Set interfaces have "of()" methods to create an empty or no-empty Immutable List or Set objects as show below:
```java
//Empty List
List immutableList = List.of();

//Non Empty immutableList
List nonEmptyImmutableList = List.of("one", "two", "three");
```

The Map has two sets of methods: `of()` methods and `ofEntries()` methods to create an Immutable Map object and an Immutable Map.Entry object respectively.
```java
//Empty Map
Map emptyImmutableMap = Map.of();

//Non Empty Map
Map nonEmtpyImmutableMap = Map.of(1, "one", 2, "two", 3, "three");
```
## 3. Private methods in interfaces
In Java 8, we can provide method implementation in Interfaces using Default and Static methods. However we cannot create private methods in interfaces.
To avoid redundant code and more re-usability, Oracle Corp is going to introduce private methods in Java SE 9 interfaces. From Java SE 9 onwards, we can write and private static methods too in an interface using `private` keyword.
These private methods are like other class private methods only, there is no difference between them.
```java
public interface Card {
  private Long createCardID() {
    //Method implementation goes here.
  }

  private static void displayCardDetails() {
    //Method implementation goes here.
  }
}
```
## 4. Java 9  Module System
One of the big changes or java 9 feature is the Module system. Oracle Crop is going to introduce the following as part of **Jigsaw Project**.
- Module JDK
- Module Java Source
- Modular Run-time images
- Encapsulate Java internal APIs
- Java Platform Module System

Before Java SE 9 versions, we are using Monolithic Jars to develop  Java-Based applications. This architecture has a lot of limitations and drawbacks. To avoid all these shortcomings, Java SE 9 is coming with the Module System.

JDK 9 is coming with 92 modules (may change in final release). We can use JDK module and also we can create our own modules as shown below:
```java
module com.foo.bar {}
```
Here we are using `module` to create a simple module. Each module has a name, related code and other resources.

## 5. Process API Improvements
Java SE 9 is coming with some improvements in Process API. They have added couple new classes and methods to ease the controlling and managing of OS processes.
Two new interface in Process API:
- java.lang.ProcessHandle
- java.lang.ProcessHandle.Info

```java
 ProcessHandle currentProcess = ProcessHandle.current();
 System.out.println("Current Process Id: = " + currentProcess.getPid());
```

## 6. Try With Resources Improvement
We know, Java SE 7 has introduced a new exception handling construct: Try-With-Resources to manage resources automatically. The main goal of this new statement is "Automatic Better Resource Management".
Java SE 9 is going to provide some improvements to this statement to avoid some more verbosity and improve some Readability.

**Java SE7 example**
```java

void testARM_Before_Java9() throws IOException{
 BufferedReader reader1 = new BufferedReader(new FileReader("journaldev.txt"));
 try (BufferedReader reader2 = reader1) {
   System.out.println(reader2.readLine());
 }
}
```
**Java 9 example**
```java

void testARM_Java9() throws IOException{
 BufferedReader reader1 = new BufferedReader(new FileReader("journaldev.txt"));
 try (reader1) {
   System.out.println(reader1.readLine());
 }
}
```

## 7. CompletableFuture API Improvements
In Java SE 9, Oracle Corp is going to improve CompletableFuture API to solve some problem raised in Java SE 8. They are going add to support some delays and timeouts, some utility methods and better sub-classing.
```java
Executor exe = CompletableFuture.delayedExecutor(50L, TimeUnit.SECONDS);
```
Here delayedExecutor() is a static utility method used to return a new Executor that submits a task to the default executor after the given delay.

## 8. Reactive Streams
Nowdays, Reactive Programming has become very popular in developing applications to get some beautiful benefits. Scala, Play, Akka, etc. Frameworks have already integrated Reactive Streams and getting many benefits. Oracle Corps is also introducing new Reactive Streams API in Java SE 9.
Java SE 9 reactive streams API is a Publish/Subscribe framework to implements Asynchronous, Scalable and Parallel application very easily using Java language.
Java SE 9 has introduced the following API to develop Reactive Streams in Java-based applications.
- java.util.concurrent.Flow
- java.util.concurrent.Flow.Publisher
- java.util.concurrent.Flow.Subscriber
- java.util.concurrent.Flow.Processor

## 9. Diamond Operator for Anonymous Inner Class
We know , Java SE 7 has introduced one new feature: Diamond Operator to avoid redundant code and verbosity, to improve readability. However, in Java SE 8, Oracle Corp(Java Library Developer) has found that some limitations in the use of Diamond operator with Anonymous Inner Class. They have fixed those and going to release them as part of Java 9.
```java
  public List getEmployee(String empid){
     // Code to get Employee details from Data Store
     return new List(emp){ };
  }
```
Here we are using just "List" without specifying the type parameter.

## 10. Optional Class improvements
In Java SE 9, Oracle Corp has added some useful new methods to java.util.Optional class. Here I'm going to discuss about one of those methods with some simple example: stream method.

If a value present in the given Optional object, this stream() method returns a sequential Stream with that value. Otherwise, it returns an empty Stream.

They have added "stream()" method to work on Optional object lazily as shown below:
```java
Stream<Optional> emp = getEmployee(id);
Stream empStream = emp.flatMap(Optional::stream)
```
Here Optional.stream() method is used to convert a Stream of Optional of Employee object into a Stream of Employee so that we can work on this result lazily in the result code.

To understand more about this feature with more examples and to read more new methods added to Optional class.

## 11. Stream API improvements
In Java SE 9, Oracle Corp has added four useful new methods to java.util.Stream interface. As Stream is an interface, all those new implemented methods as default methods. Two of them are very important `dropWhile` and `takeWhile` methods.

If you are familiar with Scala Language or any Functions programming language, you will definitely know about these methods. These are very useful methods in writing some functional style code. Let us discuss the takeWhile utility method here.

This takeWhile() takes a predicate as an argument and returns a Stream of the subset of the given Stream values until that Predicate returns false for the first subset of the given Stream values until that Predicate returns false for the first time. If the first value does NOT satisfy that Predicate it just returns an empty Stream.

```java
jshell > Stream.of(1,2,3,4,5,6,7,8,9,10).takeWhile( i -> i < 5).forEach(System.out::println);
```
