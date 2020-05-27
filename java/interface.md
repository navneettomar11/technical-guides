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
TO implement this interface, the name of your class would change (to a particular brand of bicycle, for example, such as ACMEBicyle), and you'd use the _implements_ keyword in the class declaration:

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

In the same way, when calling the ObjectOutputStream.writeObject(), **the JVM checks if the obhect implements the Serializable marker interface**. When it's not the case a NotSerializableException is thrown. Therefore the object isn't serialized to the output stream.

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

