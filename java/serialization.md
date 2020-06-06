# Serialization

Serialization is the conversion of the state of an object into a byte stream; deserialization does the opposite. Stated differently, serialization is the conversion of a Java object into a static stream(sequence) of bytes which can then be saved to database or transferred over a network.

The serialization process is instance-independent i.e. objects can be serialized on one platform and deserialized on another. **Classes that are eligible for serialization need to implement a special marker interface _Serializable_**.

Both _ObjectInputStream_ and _ObjectOutputStream_ are high level classes that extend _java.io.Inputstream_ and _java.io.OutputStream_ respectively. _ObjectOutputStream_ can write primitive types and graphs of objects to an _OutputStream_ as a stream of bytes. These streams can subsequently be read using _ObjectInputStream_.

The most important method in _ObjectOutputStream_ is
```java
public final void writeObject(Object o) throws IOException;
```
Which takes a serializable object and convert it into a sequence(stream) of bytes. Similarly, the most important method in _ObjectInputStream_ is:
```java
public final Object readObject()
    throws IOException, ClassNotFoundException;
```
Which can read a stream of bytes and convert it back into a Java object. This can then be cast back to the original object.

Let's illustrate serialization with a Person class. Note that **static fields belongs to a class(as opposed to an object) and are not serialized**. Also, not that we can use the keyword _transient_ to ignore class fields during serialization.
```java
public class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    static String country = "ITALY";
    private int age;
    private String name;
    transient int height;

    //getters and setters
}
```

## Java Serialization Caveats
There are some caveats which concern the serialization in Java.

### 1. Inheritance and Composition
When a class implements the _java.io.Serializable_ interface, all its subclasses are serializable as well. On the contrary when an object has a reference to another object, these object must implement the serializable interface separately or else a _NotSerializableException_ will be thrown:
```java
public class Person implements Serializable {
    private int age;
    private String name;
    private Address address; //must be 
}
```
If one of the fields in a serializable object consists of an array of objects, then all these object must be serializable as well, or else a NotSerializableException will be thrown.

### 2. Serial Version UID
The JVM associates a version (long) number with each serializable class. It is used to verify that the saved and loaded objects have the same attributes and thus are compatible on serialization.

This number can be generated automatically by most IDEs and is based on the class name, its attributes and associated access modifiers. Any changes result in a different number and can cause an InvalidClassException.

If a serializable class doesn't declare a _serialVersionUID_, the JVM will generate one automatically at run-time. However, it is highly recommended that each class declared its serialVersionUID as generated one is compiler dependent and thus may result in unexpected _InvalidClassExceptions_. This computation is extremely sensitive and may vary from one compiler implementation to another and hence you may turn up getting the InvalidClassException even for the same class just because you used different compiler implementations on the sender and the receiver ends of the serialization process.

### 3. Custom Serialization in Java
Java specifies a default way in which object can be serialized. Java classes can override this default behavior Custom serialization can be particularly useful when trying to serializable an Object that has some unserializable attributes. This can be done by providing two methods inside the class that we want to serialize.

```java 
private void writeObject(ObjectOutputStream out) throws IOException;

private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException;
```
With these methods, we can serialize those unserialize attributes into other forms that can be serialized.

## Serialization Proxy Pattern
In java serialization mechansim. It is actual Object of the class that gets serialized. This increase the security risk. Any attacker can construct an real object from that serialized state of object and invoke the method.

Do we want to get our object exposed openly. Obviously No. Won't it be good that instead of real object a proxy of real object is serialized. This proxy will represent that state of real object that was earilier intended to be serialized. Now through this proxy attacker can not hit on real object and object safety is appropriately imposed.

Let's consider we have a class Student that we had been traditionally serializing.
```java
public class Student implements Serializable {

    private String name;
    private String class;

    Student() {}

    Student(String name, String class) {
        this.name = name;
        this.class = class;
    }
}
```
So if you serialize an object of this class actual object of the class will be serialized and exposed.

To create a proxy let's create a private static inner class in Student class. Let the name of class be StudentProxy. StudentProxy class will have single argument constructor. This constructor will only assign data from actual object to proxy object.

```java
private static class StudentProxy implements Serializable {
    private String name;
    private String class;

    StudentProxy(Student student) {
        this.name = student.name;
        this.class = student.class;
    }   
}
```

StudentProxy is private class in Student class. We need to provide a method to create the proxy. In Student class we write a method writeReplace.

```java
private Object writereplace() {
    return new StudentProxy(this);
}
```
This method is simply invoking the constructor and creating the Student Proxy instead of Student itself. Now we are serializing the proxy. But attacker might try to fabricate this to Actual Student object and might succeed to attack the code. How do we save that?

Simply we need to insulate that by adding readObject method to Student class which basically will be invoked if attacker tries to reconstruct the object. In readObject method we need to restrict object creation.

```java
private void readObject(ObjectInputStream stream) throws InvalidObjectException {
    throw new InvalidObjectException("Go via Proxy");
}
```
So here attacker has got no option to attack object directly and If he tries to attack on proxy it will fail to attack the real object as it has no public access to proxy. So we have safegurad our serialization and deserialization mechanism.

Finally in our StudentProxy class we need to provide readResolve method. This method will create the object at deserialization time using Student Constructor. So if serialized object got maligned it would not get though, this method will create same only using same constructor, nothing extra will be added to deserialized object.
```java
private Object readResolve() {
    return new Student(name, class);
}
```
Need to consider the fact that this proxy based security does not come free of cost but it adds to overall performance degradation to certain extend. So one need to be judicious to use proxy pattern only when it is really required.

## Externalization
In serialization, the JVM is totally responsible for the process of writing and reading objects. This is useful in most cases, as the programmer do not have to care about the underlying details of the serialization process. However, the default serialization does not protect sensitive information such as password and credentials, or what if the programmers want to secure some information during the serialization process?

Thus externalization comes to give the programmers full control in reading and writing objects during serialization.

When you want to control the process of reading and writing objects during the serialization and de-serialization process have the object's class implemented the `java.io.Externalizable` interface.  Then you implement your own code to write object's state in the `writeExternal()` method and read object's state in the `readExternal()` method.

Let's have a look at this simple example:
```java
public class Country implements Externalizable {
    private static final long serialVersionUID = 1L;

    private String name;
    private int code;

    // getters and setters

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeUTF(name);
        out.writeInt(code);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        this.name = in.readUTF();
        this.code = in.readInt();
    }
}
```
