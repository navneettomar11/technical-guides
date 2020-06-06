# Exception handling
Imagine that we order a product online, but while en-route, there's a failure in delivery. A good company can handle this problem and gracefully re-route our package so that it still arrives on time.

Likewise, in Java the code can experience errors while executing our instructions. Good exception handling can handle errors and gracefully re-route the program to give the user still a positive experience.

## Why Use It?
We usually write code in an idealized environment the filesystem always contains our files, the network is healthy and the JVM always has enough memory. Sometimes we call this the "happy path".

In production though, filesystems can corrupt, networks break down and JVMs run out of memory. The well being of our code depends on how it deals with "unhappy paths".

We must handle these conditions because they affect the flow of the application negatively and form exceptions.

```java
public static List<Player> getPlayers() throws IOException {
    Path path = Paths.get("players.dat");
    List<Player> players = Files.readAllLines(path);
    return players.stream().map(Player::new).collect(Collectors.toList());
}
```

This code choose not to handle the _IOException_ passing it up the call stack instead. In an idealized environment the code works fine.

But what might happen in production if players.dat is missing?
```unix
Exception in thread "main" java.nio.file.NoSuchFileException: players.dat <-- players.dat file doesn't exist
    at sun.nio.fs.WindowsException.translateToIOException(Unknown Source)
    at sun.nio.fs.WindowsException.rethrowAsIOException(Unknown Source)
    // ... more stack trace
    at java.nio.file.Files.readAllLines(Unknown Source)
    at java.nio.file.Files.readAllLines(Unknown Source)
    at Exceptions.getPlayers(Exceptions.java:12) <-- Exception arises in getPlayers() method, on line 12
    at Exceptions.main(Exceptions.java:19) <-- getPlayers() is called by main(), on line 19
```

Without handling this exception, an otherwise healthly program may stop running altogether! We need to make sure that our code has a plan for when things go wrong.

Also note one more benefit here to exceptions, and that is the stack trace itself. Because of this stack trace, we can often pinpoint offending code without needing to attach a debugger.


## Exception Hierarchy
Utimately, exceptions are just Java objects with all of them extending from Throwable.

                --->Throwable <-----
                |                   |                 
                |                   |
                |                   |
        --->Exception             Error  
        |    (checked)             (unchecked) 
        |
        |
        |
    RuntimeException
    (unchecked)

There are three main categories of exceptional conditions:
* Checked exceptions
* Unchecked exception/ Runtime exceptions
* Errors

## Checked Exceptions
Checked exceptions are exceptions that the Java compiler require us to handle. We have to either declararively throw the exception up the call stack, or we have to handle it ourselves. 

Oracle documentation tell us to checked exceptions when we can resonably expect the caller of our method to be able to recover. A couple of examples of checked exceptions are IOException and ServletException.

## Unchecked Exceptions
Unchecked exceptions are exceptions that the Java compiler does not require us to handle. 

Simply put, if we create an exception that extends RuntimeException, it will be unchecked otherwise it will be checked.

And while this sound convenient, Oracle documentation tells us that there are good reasons for both concepts like differentiating between a situational error(checked) and a usage error(unchecked).

Some examples of unchecked exceptions are NullPointerException and SecurityException.

## Errors
Errors represent seriously and usually irrecoverable conditions like a library incompatibility, infinite recursion or memory leaks.

And even though they don't extends RuntimeException, they are also unckecked. 

In mist cases, it'd be weird for us to handle instantiate or extends Errors. Usually we want these to propage all the way up.

A couple of examples errors are a StackOverflowError and OutOfMemoryError.


## Handling Exceptions
In the Java API, there are plenty of places where things can go wrong, and some of these placrs are marked with exceptions, either in the signature or the Javadoc.
```java
/**
* @exception FileNotFountException
*/
public Scanner(String fileName) throws FileNotFoundException {
    //....
}
```
As stated a little bit earilier, when we call these `risky` methods, we must handle the checked exceptions, and we may handle the unchecked ones. Java gives us serveral ways to do this:
1. **throws**: The simplest way to "handle" an exception is to rethrow it:
    ```java
        public int getPlayeerScore(String playerFile) throws FileNotFoundException {
            Scanner contents = new Scanner(new File(playerFile));
            return Integer.parseInt(content.nextLine());
        }
    ```
    Because _FileNotFoundException_ is a checked exception, this is the simplest way to satisfy the compiler, but ut does mean that anyone that calls our method now needs to handle it too!

    _parseInt_ can throw a _NumberFormatException_, because it is unchecked, we aren't required to handle it.

2. **try-catch**: If we want to try and handle the exceptions ourselved, we can use a try-catch block. We can handle it by rethrowing our exception.
    ```java
    public int getPlayerScore(String playerFile) {
        try {
            Scanner contents = new Scanner(new File(playerFile));
            return Integer.parseInt(content.nextLine());               
        } catch (FileNotFoundException noFile) {
            throw new IllegalArugmentException("File not found");
        }
    }
    ```    
    Or by performing recovery steps:
    ```java
    public int getPlayerScore(String playerFile) {
        try {
            Scanner contents = new Scanner(new File(playerFile));
            return Integer.parseInt(content.nextLine());               
        } catch (FileNotFoundException noFile) {
            log.warn("File not found, resetting score.");
            return 0;
        }
    }
    ```
3. **finally**: Now, there are times when we have code that needs to execute of whether an exception occurs, and this is where the finally keyword comes in.
    In our examples so far, there's been a nasty bug lurking in the shadows, which is that Java default won't return file handles to the operating system.
    Cetainly, whether we can read the file or not, we want to make sure that we do the appropriate cleanup!.

    Let try this the "lazy" first:
    ```java
    public int getPlayeerScore(String playerFile) {
        Scanner contents = null;
        try {
            contents = new Scanner(new File(playerFile));
            return Integer.parseInt(content.nextLine());
        } finally {
            if (contents != null) {
                contents.close();
            }
        }
    }
    ```
    Here, the finally block indicates what code we want Java to run regardless of what happens with trying to read the file. 

    Even if a FileNotFoundException is thrown up the call stack, Java will call the contents of finally before doing that. We can also both handle the exception and make sure that our resources get closed.
    ```java
    public int getPlayeerScore(String playerFile) {
        Scanner contents = null;
        try {
            contents = new Scanner(new File(playerFile));
            return Integer.parseInt(content.nextLine());
        } catch (FileNotFoundExceptiob noFile) {
            log.warn("File not found, resetting score.");
            return 0;
        } finally {
            try{
                if (contents != null) {
                    contents.close();
                }
            } catch(IOException io) {
                log.error("Couldn't close the reader", io);
            }
        }
    }
    ```
    Because close is also "risky" method, we also need to catch its exception!

4. **try-with-resources**: Fortunately, as of Java, we can simplify the above syntax when working with things that extend AutoClosable.
    ```java
    public int getPlayeerScore(String playerFile) {
        Scanner contents = null;
        try(Scanner contents = new Scanner(new File(playerFile))){
            return Integer.parseInt(content.nextLine());
        } catch (FileNotFoundException noFile) {
            log.warn("File not found, resetting score");
            return 0;
        }
    }
    ```
    When we place references that are AutoClosable in the try declaration, then we don't need to close the resource ourselves.

    We can still use a finally block, though to do any other kind of cleanup we want.

5. **Multiple catch Blocks**: Sometimes, the code can throw more than one exception, and we can have more than one catch block handle each individually:
    ```java
    public int getPlayerScore(String playerFile) {
        try (Scanner contents = new Scanner(new File(playerFile))) {
            return Integer.parseInt(contents.nextLine());
        } catch (IOException e) {
            logger.warn("Player file wouldn't load!", e);
            return 0;
        } catch (NumberFormatException e) {
            logger.warn("Player file was corrupted!", e);
            return 0;
        }
    }
    ```
    Multiple catches give us the chance to handle each exception differently, should the need arise. 

    Also note here that we didn't catch FileNotFoundException and that is because it extends IOException. Because we're catching IOException. Java will consider any of its subclasses also handled.

    Let's say though, that we need to treat FileNotFoundException differently from the more general IOException.
    ```java
    public int getPlayerScore(String playerFile) {
        try (Scanner contents = new Scanner(new File(playerFile)) ) {
            return Integer.parseInt(contents.nextLine());
        } catch (FileNotFoundException e) {
            logger.warn("Player file not found!", e);
            return 0;
        } catch (IOException e) {
            logger.warn("Player file wouldn't load!", e);
            return 0;
        } catch (NumberFormatException e) {
            logger.warn("Player file was corrupted!", e);
            return 0;
        }
    }
    ```
    Java let us handle subclass exceptions separately, remember to place them higher in the list of catches.

6. **Union catch Blocks**: When we know the way we handle errors is going to be the same through Java 7 introduced the ability to catch multiple exceptions in the same block.
    ```java
    public int getPlayerScore(String playerFile) {
        try (Scanner contents = new Scanner(new File(playerFile))) {
            return Integer.parseInt(contents.nextLine());
        } catch (IOException | NumberFormatException e) {
            logger.warn("Failed to load score!", e);
            return 0;
        }
    }
    ```    

## Throwing Exceptions
If we don't want to handle the exception ourselves or we want to generate our exceptions for others to handle, then we need to get familiar with the throw keyword.

Let's say that we have the following checked exception we've created ourseleves:
```java
public class TimeoutException extends Exception {
    public TimeoutException(String message) {
        super(message);
    }
}
```
and we have a method that could potentially take a long time to complete
```java
public List<Player> loadAllPlayers(String playersFile) {
    // ... potentially long operation
}
```
### Throwing a Checked Exception
Like returning from a method, we can throw at any point. Of course we should throw when we are trying to indicate that something has gone wrong.
```java
public List<Player> loadAllPlayers(String playersFile) throws TimeoutException {
    while ( !tooLong ) {
        // ... potentially long operation
    }
    throw new TimeoutException("This operation took too long");
}
```

Because TimeoutException is checked, we also must use the throws keyword in the signature so that callers of our methods will know to handle it.

### Throwing an Unchecked Exception
If we want to do something like, say validate input we can use an unchecked exception instead:
```java
public List<Player> loadAllPlayers(String playersFile) throws TimeoutException {
    if(!isFilenameValid(playersFile)) {
        throw new IllegalArgumentException("Filename isn't valid!");
    }
    
    // ...
}
```

Because IllegalArgumentException is unchecked, we don't have to mark the method, thought we are welcome to. Some mark the method anyway as a form of documentation.

### Wrapping and Rethrowing
We can also choose to rethrow an exception we've caught
```java
public List<Player> loadAllPlayers(String playersFile) 
  throws IOException {
    try { 
        // ...
    } catch (IOException io) {      
        throw io;
    }
}
```
Or do a wrap and rethrow
```java
public List<Player> loadAllPlayers(String playersFile) 
  throws PlayerLoadException {
    try { 
        // ...
    } catch (IOException io) {      
        throw new PlayerLoadException(io);
    }
}
```
This can be nice for consolidating many different exceptions into one.

### Rethrowing Throwable or Exception
If the only possible exceptions that a given block of code could raise are unchecked exceptions, then we can catch and rethrow Throwable or Exception without adding them to our method signature.
```java
public List<Player> loadAllPlayers(String playersFile) {
    try {
        throw new NullPointerException();
    } catch (Throwable t) {
        throw t;
    }
}
```
While simple, the above code can't throw a checked exception and because of that, even though we are rethrowing a checked exception, we don't have to mark the signature with a throws clause.

This is handy with proxy classes and methods.

### Inheirtance
When we mark methods with a throws keyword, it impacts how subclasses can override our method. In circumstance where our method throws a checked exception.
```java
public class Exceptions {
    public List<Player> loadAllPlayers(String playersFile) 
      throws TimeoutException {
        // ...
    }
}
```
A subclass can have a "less risky" signature:
```java
public class FewerExceptions extends Exceptions {   
    @Override
    public List<Player> loadAllPlayers(String playersFile) {
        // overridden
    }
}
```
But not a "more riskier" signature
```java
public class MoreExceptions extends Exceptions {        
    @Override
    public List<Player> loadAllPlayers(String playersFile) throws MyCheckedException {
        // overridden
    }
}
```

This is because contract are determined at compile time by the reference type. If I create an instance of MoreExceptions and save it to Exceptions.
```java
Exceptions exceptions = new MoreExceptions();
exceptions.loadAllPlayers("file");
```

Then the JVM will only tell me to catch the TimeoutException which is wrong since I've said that MoreExceptions#loadAllPlayers throws a different exception.
**Simply put, subclasses can throw fewer checked exceptions than their superclass, but not more**.

## Why FileNotFoundException is Checked Exception ?
Exception always encountered at runtime only. Difference is made when a exception is handled.
**Checked or unchecked means whether it is forced to handle at compile time or it will only be identified when it is encounterd at runtime.**
If an exception is checked means compiler has way to identify whether the exception can occur or not, and whenever you compile it, you will be forced to handle a checked exception and by doing this changes of runtime exceptions will be reduced.

During file handling, compiler doesn't check whether file is present or not, it just check whether you have handled FileNotFoundException or not, because one you are dealing with a file changes of encounterting this exception is very high and you should handle it in your code. For arithmetic exception there is not way to find it during compile time and thus it is left unchecked.
