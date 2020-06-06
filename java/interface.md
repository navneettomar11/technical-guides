# Interface
Object define their interaction with the outside world through the methods that they expose. Methods from the object's interface with the outside world; the buttons on the front of yours television set, for example, are the interface between you and the electrical wiring on the other side of its plastic casing. You press the "power" button to turn the televison on and off.

In its most common form, an interface is a group of related method with empty bodies. A bicylce's behavior, if specified as an interface, might as follows:
```java
interface Bicycle {

    //wheel revolutions per minute
    void changeCadence(int newValue);

    void changeGear(int newValue);

    void seepUp(int increment);

    void applyBrakes(int decrement);
}
```
To implement this interface, the name of your class would change (to a particular brand of bicycle, for example, such as ACMEBicyle), and you'd use the _implements_ keyword in the class declaration:

```java
class ACMEBicycle implements Bicycle {
     int cadence = 0;
    int speed = 0;
    int gear = 1;

   // The compiler will now require that methods
   // changeCadence, changeGear, speedUp, and applyBrakes
   // all be implemented. Compilation will fail if those
   // methods are missing from this class.

    void changeCadence(int newValue) {
         cadence = newValue;
    }

    void changeGear(int newValue) {
         gear = newValue;
    }

    void speedUp(int increment) {
         speed = speed + increment;   
    }

    void applyBrakes(int decrement) {
         speed = speed - decrement;
    }

    void printStates() {
         System.out.println("cadence:" +
             cadence + " speed:" + 
             speed + " gear:" + gear);
    }
}
```

Implementing an inteface allows a class to become more formal about the behavior it promise to provide. Interfaces form a contract between the class and the outside world, and this contract is enforced at build time by the compiler. If you class claims to implement an interface, all methods defined by the interface must appear in its source code before the class will successfully compile.

## private and static Methods in Interface
With the release of Java 8, interface now can include static methods.
Similar to a class, we can access static methods of an interface using its references.
Also, interfaces support private methods with the release of Java 9. Now you can use **private methods** and **private static methods** in interfaces.

Since you cannot instantiate interfaces, private methods are used as helper methods that provide support to other methods in interfaces.

## default method in interfaces
With the release of Java 8, nmethod with implementation (default methods) were introduced inside an interface. Before that, all the methods were abstract in Java. 
To delcare default methods inside interfaces, we use the `default` keyword. For example,

```java
    public default void getSides() {
        // body of getSides    
    }
```
Let's take a scenario to understand why default methods are introduced in Java. 

Suppose, we need to add a new method in an interface.

We can add the method in our interface easily without implementation. However, that's not the end of the story. All our classes that implement that interface must provide an implementation for the method.

If a large number of classes were implementing this interface, we need to track all these classes and make changes in them. This is not only tedious but error-prone as well.

To resolve this Java introduced default methods. Default method are inherited like ordinary methods.

## Marker/Tag Interface
A marker interface is an interface that **has no methods or constants inside it**. It provide **run-time information about objects**, so the compiler and JVM have **additional information about the object**.

Java has many build-in marker interface, such as Serializable, Cloneable and Remote.

Let's take the example of the *Cloneable interface*. If we try to clone an object that doesn't implement this interface the JVM throws a CloneNotSupportException. Hence, the Cloneable marker interface is an indicator to the JVM that we can call the Object.clone() method.

In the same way, when calling the ObjectOutputStream.writeObject(), **the JVM checks if the object implements the Serializable marker interface**. When it's not the case a NotSerializableException is thrown. Therefore the object isn't serialized to the output stream.

### Custom Marker Interface
Let's create our own marker interface.

For example, we could create a marker that indicates whether an object can be removed from the database:
```java
public interface Deletable {
}
```
In order to delete an entity from the database the object representing this entity has to implement our Deletable marker interface.

```java
public class Entity implements Deletable {

     //implementation details
} 
```
Let's say that we have DAO object with a method for removing from the database. We can write our delete() methods so that only objects implemententing our marker interface can be deleted.

```java
public class ShapeDao {
 
    // other dao methods
 
    public boolean delete(Object object) {
        if (!(object instanceof Deletable)) {
            return false;
        }
         // delete implementation details
        
        return true;
    }
}
```

As we can see, we are giving an indication to the JVM about the runtime behavior of our objects. If the object implements our marker interface, it can be deleted from the database.

### Marker Interface vs Annotations
By introducing annotations, Java has provided us with an alternative to achieve the same results as the marker interfaces. Moreover, like marker interfaces we can apply annotations to any class and we can use  them as indicators to perform certain actions. 

So what is the key difference?
Unlike annotations, interface allow us to take advantage of polymorphism. As a result we can add additional restrictions to the marker interface.

For instance, let's add a restriction that only a Shape type can be removed from the database.
```java
public interface Shape {
    double getArea();
    double getCircumference();
}
```
In this case, our marker interface, let's call it _DeletableShape_, will look like the following:

```java
public interface DeletableShape extend Shape {}
```

Then our class implement the marker interface
```java
public class Rectange implements DeletableShape {
    //implementation details
}
```
Therefore, all DeletableShape implementation are also Shape implementations. Obviously we can't do that using annotation.

However, every design has trade-offs and polymorphism can be used as a counter-argument against marker interfaces. In our example, every class extending Rectangle will automatically implement DeletableShape.

### Marker Interfaces vs Typical Interfaces
We could get the same result by modifying our DAO's delete() method to test whether our object is a Shape or not instead of testing whether it's a Deletable.
```java
public class ShapeDao {
    //other dao methods

    public boolean delete(Object object) {
        if (!object instanceof Shape)) {
            return false;
        }

        //delete implementation details

        return true;
    }
}
```

So why create a marker interface when we can achieve the same result using a typical interface?

Let imagine that, in addition to the Shape type, we want to remove the Person type from the database as will. In this cases there are two options to achieve that:

The first iption is to add an additional check to our previous delete() method to verify whether the object to delete is an instance of Person or not.
```java
public boolean delete(Object object) {
    if (! (object instanceof Shape || object instanceof Person)) {
        return false;
    }
    //delete implementation details
    return true;
}
```
But what if we have more types that we want to remove from the database as well? Obviously, this won't be a good option because we have to change our method for every new type.

The second iption is to make the Person type implements the Shape interface, which acts as a marker interface. But is a Person object really a Shape? The answer is clearly no, and that makes the second option worse than the first one.

Hence although we can achieve the same results by using a typical interface as a marker, we'll end up with a poor design.

