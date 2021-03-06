# Collections
A Collection is a group of individual objects represented as a single unit. Java provides Collection Framework which defines several classes and interfaces to represent a group of objects as a single unit.

The Collection interface (java.util.Collection) and Map interface (java.util.Map) are two main "root" interface of Java collection classes.

### Hierarchy of Collection Framework

```
        Collection                                 Map
            |                                       |   
  -------------------                               |   
  |         |       |                           SortedMap
  Set       List    Queue
  |                 |                           
  SortedSet         Deque    
  |
  NavigableSet
```

* **Collection**: Root interface with basic methods like add(), remove(), contains(), isEmpty(), addAll(), ..., etc.
* **Set**: Doesn't allow duplicates. Example implementation of Set interface are HashSet (Hashing based) and TreeSet(balanced BST based). Note that TreeSet implements SortSet.
* **List**: Can contain duplicates and element are sorted. Example implementation are LinkedList (linked list based) and ArrayList(dynamic array based).
* **Queue**: Typically order elements in FIFO order except exceptions like PriorotyQueue.
* **Deque**: ELements can inserted and removed at both ends . Allows both LIFO anf FIFO.
* **Map**: Contains key and value pairs. Doesn't allow duplicates. Example implementation are HashMap and TreeMap. TreeMap implements SortedMap

# HashCode
Hashing is a fundamental concept of computer science. 

In Java efficient hashing algorithms stand behind some of the most popular collections we have available - such as the HashMap and the HashSet.

## How hashCode() works
Simply put, hashCode() returns an integer value, generated by a hashing algorithm.

Objects that are equal(according to their equals()) must return the same hash code. **It's not required for different objects to return different hash codes**.
The general contract of hashCode() states.
- Whenever it is invoked on the same object more than once during an execution of a Java application hashCode() must consistently return the same value, provided no information used in equals comparision on the object is modified. This value needs not remain consistent from one execution of an application to another execution of the same application.
- If two object are equal according to the equals(Object) method, then calling the hashCode() method on each of the two objects must produce the same value.
- It is not required that if two objects are unequal according to the equals(java.lang.Object) method, then calling the hashCode method on each of the two object must produce distinct integer value. However developers should be aware that producing distinct integer result for unequal objects improves the performance of hash tables.

> As much as is reasonably pratical, the hashCode() method defined by class Object does not return distinct integer for distinct objects. (This is typically implemented by converting the internal address of the object into an integer, but this implementation is not required bt the Java Programming language).

## A Naive hashCode() implementation
It's actually quite straightforward to have a naive hashCode() implementation that fully adheres to the above contract. To demonstrate this, we'r going to define a sample User class the overrides the method default implementation.

```java
public class User {

    private long id;
    private String name;
    private String email;

    //standard getters/setters/constructors

    @Override
    public int hashCode() {
        return 1;
    }

    @Override
    public boolean equals(Object o) {
        if(this == o) return true;
        if(o == null) return false;
        if(this.getClass() != o.getClass()) return false;
        User u = (User) o;
        return id == u.id && 
            name.equals(u.getName()) &&
            email.equals(u.getEmail());
    }
}
```
The User class provides custom implementations for both equals() and hashCode() that fully adhere to the respective contracts. Even more, there's nothing illegitmate with having hashCode() returning any fixed value.

**However, this implementation degrafes the functionality of hash tables to basically zero, as every object would be stored in the same single bucket.**

## Improving the hashCode() implementation.
Let's improve a little bit the current hashCode() implementation by including all fields of the User class so that it can produce different results for unequal objects.

```java
@Override
public int hashCode() {
    return (int) id * name.hashCode() * email.hashCode();
}
```
This basic hashing algorithm is definitively much better than the previous one as it compute the object's hash code by just multiplying the hash code of the name and email fields and the id.

In general terms, we can say that is a reasonable hashCode() implementation, as long as we keep the equals implementation consistent with it.

## Standard hashCode() implementations
The better the hashing algorithm that we use to compute hash code the better will the performance of hash tables be.

Let's have a look at a standard implementation that uses two primes numbers to add even more uniqueness to computer hash codes.

```java
@Override
public int hashCode() {
    int hash = 7;
    hash = 31 * hash + (int) id;
    hash = 31 * hash + (name == ? 0 : name.hashCode());
    hash = 31 * hash + (email == ? 0 : email.hashCode());
    return hash;
}
```
White it's essential to understand roles that hashCode() and equals() method play, we don't have to implement them from scratch every time, as most IDEs generate custom hashCode() and equals() implementations and since Java 7 we got an `Objects.hash()` utility method for comfortable hashing;

```java
Objects.hash(name, email);
```

> What can be noticed here is that all those implementations utilize number 31 in some form - this is because 31 has a nice property - its multiplication can be replaced by a bitwise shift which is faster than the standard multiplication: 31 * i == (i << 5) - i;

# How HashMap works in Java
Hashmap works based on the hashing principle, but it is not as simple as it sounds. Hashing is the mechansim of assigning unique code to variable or attribute using an algorithm to enable easy retrieval. A true hashing mechanism should always return the same hashCode() when it is applied to the same object.
## What is Entry Class ?
HashMap has an inner class called an Entry Class which holds the key and values.
```java
static class Node<K,V> implements Map.Entry<K,V> {
    final K key;
    final V value;
    Entry<K,V> next;
    final int hash;
    ...
}
```
## How does the put() method work internally?
The code will look like this:
```java
public V put(K key, V value) 
{
    if (key == null)
       return putForNullKey(value);
    int hash = hash(key.hashCode());
    int i = indexFor(hash, table.length);
    for (Entry<K,V> e = table[i]; e != null; e = e.next) 
    {
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) 
         {
             V oldValue = e.value;
             e.value = value;
             e.recordAccess(this);
             return oldValue;
          }
     }
     modCount++;
     addEntry(hash, key, value, i);
     return null;
 }
```
First, it checks if the key given is null or not. If the given key is null, it will be stored in the zero position, as the hashcode of null will be zero.

Then it applies the hascode to the key `.hashCode()` by calling the hashcode method. In order to get the value within the limits of an array, the `hash(key.hashCode())` is called which performs some shifting operations on the hashcode.

The `indexFor()` method is used to get the exact location to store the Entry object.

Then comes the most important part - if two different object have the same hascode, will it be stored in the same bucket? To handle this, let's think of the LinkedList in the data structure. It will have a "next" attribute, which will always point to the next object, the same way the next attribute in the Entry class points to the next object. Using this different objects with the same hashcode will be place next to each other.

In the case of Collision, the HashMap checkssfor the value of the next attribute. If ti is null, it inserts the Entry ovbject in that location. If next attribute is not null, then it keeps the loop running until the next attribute is null then stores the Entry object there.

## How does the Get() method work iternally?
Almost the same logic applied in the put() method will be used to retrieve the value.
```java
public V get(Object key) 
{
    if (key == null)
       return getForNullKey();
     int hash = hash(key.hashCode());
     for (Entry<K,V> e = table[indexFor(hash, table.length)];e != null;e = e.next) 
     {
         Object k;
         if (e.hash == hash && ((k = e.key) == key || key.equals(k)))
             return e.value;
     }
         return null;
 }
```
1. First, it get the hash code of the key object, which is passed, and finds the bucket location.
2. If the correct bucket is found, it returns the value.
3. If no match is found, it return null

# Iterator
Iterators are used in Collection framework in java to retrieve one by one. There are three iterators.
1. **Enumeration**: It is an interface used to get elements of legacy collections (Vector, Hashtable). Enumeration is the first iterator present from JDK 1.0, resets are included in JDK 1.2 with more functionality. Enumerations are also used to specify the input streams to a SequenceInputStream. We can crete Enumeration object by calling elements() method of vector class on any vector object.
    ```java
    //Here "v" is an Vector class object; e is of type Enumeration interfaces and refers to v
    Enumeration e = v.elements();
    ```
2. **Iterator**:  It is a universal Iterator as we can apply it to any. Collection object. By using Iterator, we can perform both read and remove operations. It is improved version of Enumeration with additional functionality of remove ability of a element.

    Iterator must be used whenever we want to enumerate elements in all Collection framework implemented interfaces like Set, List, Queue, Deque and also in all implemented classes of Map interface. Iterator is the only cursor availiable for entire collection framework.

    Iterator object can be created by calling `iterator()` method present in Collection interface.
    ```java
    //Here "c" is any Collection object, its is of type Iterator interface and refers to "c"
    Iterator itr = c.iterator();
    ```
3. **ListIterator**: It is only applicable for List collection implemented classes like ArrayList, LinkedList, etc. It provides bi-directional iteration. ListIterator must be used when we want to enumerate elements of List. This cursor has more functionality than iterator. ListIterator object can be created by calling listIterator() method present in List interface.
    ```java
    // Here "l" is any List object, ltr is of type ListIterator interfce and refers to "l"
    ListIterator ltr = l.listIterator();
    ```

## Fail-fast vs Fail-safe Iterators
Fail-fast iterators fail as soon as they realized that structure of Collection has been changed since iteration has begun.  Structural changes means adding, removing or updating any element from collection while one thread is iterating over that collection.

Fail-safe iterators are just opposite to fail-fast. They never fail if you modify the underlying collection on which they are iterating because they work on clone of Collection instead of original collection and that's why they are called as fail-safe iterator.

Iterator of CopyOnWriteArrayList is an example of fail-safe iterator also iterator written by ConcurrentHashMap keySet is also fail-safe iterator and never throw ConcurrentModificationException.

# Immutable Collection
Unmodifiable collections are usually read-only views (wrappers) of other collections. You can't add, remove or clear them, but the underlying collection  can change.
Immutable collections can't be changed at all - they don't wrap another collection - they have their own elements.

So, basically in order to get an immutable collection out of a mutable one, you have to copy its element to the new collection, and disallow all operations.

Now Java 9 has factory methods of Immutable List, Set, Map and Map Entry.

List and Set interfaces have "of()" methods to create empty or non-empty Immutable list or Set objects.
```java
List emptyList = List.of();
List nonEmptyList = List.of("one", "two", "three");
```

# ConcurrentHashMap
ConcurrentHashMap provides a concurrent alternative of HashTable or Synchronized Map classes with aim to support higher level of concurrency by implementing fined grained locking. Multiple reader can access the Map concurrently while a portion of Map gets locked for write operation depends upon concurrency level of Map. ConcurrentHashMap provides better scalability than there synchronized counter part. Iterator of ConcurrentHashMap are fail-safe iterators which doesn't throw ConcurrentModificationException thus eliminates another requirment of locking during iteration which result in further scalability and performance.

```java
    Map<Integer, Integer> map = new ConcurrentHashMap<>();

    map.put(1,1);
    map.put(2,1);
    map.put(3,1);
    map.put(4,1);
    map.put(5,1);
    System.out.println("ConcurrentHashMap before iterator: "+map);
    Iterator<Integer> it = map.keySet().iterator();
    while (it.hasNext()) {
        Integer key = it.next();
        if(key == 3) {
            map.put(3,3);
        }
    }
    System.out.println("ConcurrentHashMap after iterator: "+map);
```

## How does iterator know about the modificiation in the Collection ?
We have taken the set of keys from HashMap and then iterating over it.

HashMap contains a variable to count the number of modifications and iterator use it when you call its next() function to get the next entry.
```java
    /**
     * The number of times this HashMap has been structurally modified
     * Structural modifications are those that change the number of mappings in
     * the HashMap or otherwise modify its internal structure (e.g.,
     * rehash).  This field is used to make iterators on Collection-views of
     * the HashMap fail-fast.  (See ConcurrentModificationException).
     */
    transient volatile int modCount;
```
It's a counter used to detect modifications to the collection when iterating the collections: iterators are fail-fast, and throw an exception if the collection has been modified during the iteration. `modCount` is used to track modifications.

# CopyOnWriteArrayList and CopyOnWriteArraySet
CopyOnWriteArrayList is a concurrent alternative of synchronixed List. CopyOnWriteArrayList provides better concurrency than synchronization List by allowing multiple concurrent reader and replacing the whole list on write operation. Yes, write operation is cost on CopyOnWriteArrayList but it performs better when there are multiple reader  and requirements of iteration is more than writing. Since CopyOnWriteArrayList Iterator also don't throw ConcurrentModificationException it eliminates need to lock the collection during iteration. Remember both ConcurrentHashMap and CopyOnWriteArrayList doesn't provides same level of locking as Synchronized Collection and achieves thread-safety by there locking and mutability strategy. So they perform better if requirment suits there nature. Similarly, CopyOnWriteArraySet is a concurrent replacement to Synchronized Set.
```java
    List<String> list = new CopyOnWriteArrayList<>();
    list.add("1");
    list.add("2");
    list.add("3");
    list.add("4");
    list.add("5");

    // get the iterator
    Iterator<String> it = list.iterator();

    //manipulate list while iterating
    while(it.hasNext()){
        System.out.println("list is:"+list);
        String str = it.next();
        System.out.println(str);
        if(str.equals("2"))list.remove("5");
        if(str.equals("3"))list.add("3 found");
        if(str.equals("4")) list.set(1, "4");
    }
```
### How do you remove an entry from a Collection ? and subsequently what is the difference between the remove() method of Collection and remove() method of Iterator, which one you will use while removing elements during iteration?
Collection interface defines remove(Object obj) method to remove objects from Collection. List interface adds another method remove(int index), which is used to remove objecg at specific index. You can use any of these method to remove an entry from Collection, while not iterating. Things change, when you iterate. Suppose you are traversing a List and removing only certain elements based on logic, then you need to use Iterator's remove() method. This method removes current element from Iterator prespective. If you use Collection's or List's remove() method during iteration then your code will throw `ConcurrentModificationException`. That why it's advised to use Iterator remove() method to remove() objects from Collection.

# Priority Queue
> Unbounded Queues are queues which are NOT bounded by capacity that means we should not provide the size of the queue. For example LinkedList. A
The `java.util.PriorityQueue` class is an unbounded queue based on priority heap and the elements of the priority queue are ordered by default in natural order. We can provide a Comparator for ordering at the time of instantiation of priority queue.

Java Priority Queue doesn't allow `null` values and we can't create PriorityQueue of objects that are non-comparable. We use java Comparable and Comparator for sorting Objects and PriorityQueues use them for priority processing of it's element.

The head of the priority queue is the least element based on the natural ordering or comparator based ordering, if there are multiple objects with same ordering, then it can poll any one of them randomly. When we poll the queue, it returns the head object from the queue.

PriorityQueue size is unbounded but we can specify the intial capacity at the time of it's creation. When we add elements to the prioity queue, it's capacity grow automatically.

Priority queue is not thread safe so java provides PriorityBlockingQueue class that implements the BlockingQueue interface to use in Java multithreading environment.

Java Priority Queue implementation provides O(log(n)) time for enqueuing and dequeing method.

For example, let's say we have an application that generate stocks reports for daily trading session. This application processes a lot of data and take times to process it. So customers are sending request to the application that is actually getting queued but we want to process premium customers first and standard customer after them. So in this case **PriorityQueue** implementation in java can be really helpful.

# Blocking Queue
BlockingQueue is also one of better known collection class in Java 5. BlockingQueue makes it easy to implement producer-consumer design pattern by providing inbuilt blocking support for put() and take() method. put() method will block if queue is full while take() method will block if queue is empty. Java 5 API provides two concrete implementation of BlockingQueue in form of ArrayBlockingQueue is backed by Array and its bounded in nature while LinkedBlockingQueue is optionally bounded. Consider using BlockingQueue to solve producer consumer problem in Java instead of writing your own wait-notify code. Java 5 also provides PriorityBlockingQueue, another implementaton of BlockingQueue which is ordered on priority and useful if you want to process elements an order than FIFO.
```java
public class BlockingQueueExample {

    public static void main(String[] args) {
        BlockingQueue<Integer> blockingQueue = new ArrayBlockingQueue<>(5);
        int[] count = new int[] {0};
        Runnable producer = new Runnable() {
            @Override
            public void run() {
                try{
                    count[0]++;
                    blockingQueue.put(count[0]);
                    Thread.sleep(1000);
                } catch (InterruptedException iex) {
                    iex.printStackTrace();
                }
            }
        };

        Runnable consumer = new Runnable() {
            @Override
            public void run() {
                try {
                    System.out.println(blockingQueue.take());
                }catch (InterruptedException iex) {
                    iex.printStackTrace();
                }
            }
        };

        Thread producerThread = new Thread(producer);
        Thread consumerThread = new Thread(consumer);

        producerThread.start();
        consumerThread.start();
    }
}
```
## What is the difference between `poll()` and `remove()` method of Queue interface ?
Though both poll() and remove() method from Queue is used to remove the object and returns the head of the queue, there is a subtle difference between them. If Queue is empty() then call to remove() method will throw Exception, while a call to poll() method returns null. By the way, exactly which element is removed from queue depends upon queue's ordering policy and varies between different implementation, for example, `PriorityQueue` keeps the lowest element as per `Comparator` or `Comparable` at head position.

# NavigableMap & NaviagableSet
NavigableMap map was added in Java 1.6 it adds navigation capability to Map data structure. It provides method like `lowerKey()` to get keys which is less than specified key, `floorKey()` to return keys which is less than or equal to specified key, `ceilingKey()` to gets keys which is greater specified key from a Map. It also provide similar methods to get entries eg. lowerEntry(), floorEntry(), ceilingEntry() and higherEntry(). Apart from navigation methods, it also provides utilies to create sub-Map eg creating map from entries of an existing Map like tailMap, headMap and subMap. headMap() method returns a NavigableMap whose keys are less than specified, tailMap() return a NavigableMap whose keys are greater than the specified and subMap() gives a NaviagableMap between a range specified by toKey and fromKey.

A SortedSet extend with navigation method reporting closest matches for given search targets. Method lower, floor, ceiling and higher return elements respectively less than, less than or equal, greater than or equal and greater than a given element, return null if there is no such element. A NavigableSet may be accesed and traversed in either ascending or descending order. 

# Comparable and Comparator interface
In Java, all collection which have feature of automatic sorting use compare methods to ensure the correct sorting of elements. For example whic classes which uses sorting are TreeSet, TreeMap, etc.

To sort the data elements a class needs to implement Comparator or Comparable interface. That's why all Wrapper classes like Integer, Double and String class implements Comparable interface.

Comparable helps in preserving default natural sorting whereas Comparator helps in sorting the elements in some special required sorting pattern. The instance of comparator if passed usually as collection's constructor argument in supporting collections.


# Generics
In a nutshell, generics enables types(classes and interfaces) to be parameters when defining classes, interfaces and methods. Much like more familiar formal parameters used in method declarations, type parameters provide a way for you to re-use the same code with different inputs. The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types.
Code that uses generics has many benefits over non-generic code
* **Stronger type checks at compile time**: A Java compier applies strong checking to generic code and issues errors if code violates type safety. Fixing compile-time errors is easier than fixing runtime errors, which can be  diffcult to find.
* **Elimination of casts**: The following code snipper generics requires casting:
    ```java
        List list = new ArrayList();
        list.add("hello");
        String s = (String) list.get(0);
    ```
    When re-written to use generics, the code does not require casting:
    ```java
        List<String> list = new ArrayList<String>();
        list.add("hello");
        String s = list.get(0);
    ```
* **Enabling programmers to implements generic algorithms**: By using generic, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.

## Generic Types
A generic type is a generic class or interface that is parameterized over types. The following `Box` class will be modified demonstrate the concept.

Begin by examining a non-generic `Box` class that operates on objects of any type. It needs only to provide two methods: _set_ which adds an object to the box, and _get_ which retrieves it:
```java
public class Box {
    private Object object;

    public void set(Object o) {
        this.object = o;
    }

    public Object get() {
        return this.object;
    }
}
```
Since its methods accept or return an Object, you are free to pass in whatever you want, provided that it is not one of the primitive types. There is no way to verify at compile time, how the class is used. One part of the code may place an integer in the box and expected to get integers out of it, while another part of the code may mistakely pass in a String, resulting in a runtime error.

A generic class is defined with the following format:
```java
class name<T1,T2,T3,....,Tn> {
    /* code */
}
```
The type parameter section, delimited by angle brackets(<>), follows the class name. It specifies the type parameters (also called type variables) T1, T2,T3,...,Tn.

To update the `Box` class to use generic, you create a generic type declaration by changing the code "public class Box" to "public class Box<T>". This introduces the type variable, T that can be used anywhere inside the class.

With this change, the Box class becomes:
```java
public class Box<T> {
    // T stands for "Type"
    private T t;

    publi void set(T t) {
        this.t = t;
    }
    public T get() { return this.t;}
}
```

As you can see, all occurences of object are replaced by T. A type variable can be any non-primitive you specify: any class type, any interface type, any array type or even another variable.

### Type parameter naming conventions
The most commonly used by type parameter names are:
* E - Element (used extensively by the Java Collections Framework)
* K - Key
* N - Number
* T - Type
* V - Value
* S, U, V etc - 2nd, 3rd, 4th types


# Interview Questions
## What is the difference between Synchronized Collection and Concurrent Collection?
Java 5 has added severak new Concurrent Collection classes e.g.  ConcurrentHashMap, CopyWriteArrayList, BlockingQueue, etc. Java also provided a way to get Synchronized copy of collection e.g. ArrayList, HashMap by using Collections.synchronizedMap() Utility function one significant difference is that Concurrent Collections has better performance than synchronized collection because they lock only a portion of Map to achieve concurrency and synchronization.