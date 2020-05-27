# Classloaders
Class loaders are responsible for loading Java classes during runtime dynamically to the JVM(Java Virtual Machine). Also they are part of the JRE(Java Runtime Environment). Hence, the JVM doesn't need to know about the underlying files or file systems in order to run Java programs thanks to class loaders.

Also, these Java classes aren't loaded into memory all at once, but when required by an application. This is where class loaders come into the pictures. They are responsible for loading classes into memory.

## Types of Built-in Class Loaders
Let's start by learning how different classes are loaded using various class loaders using simple example:

```java
public void printClassLoaders() throws ClassNotFoundException {
    System.out.println("Classloader of this class:" + PrintClassLoader.class.getClassLoader());

    System.out.println("Classloader of Logging:" + Logging.class.getClassLoader());

    System.out.println("Classloader of ArrayList: "+ ArrayList.class.getClassLoader());
}
```
When executed the above method prints:
> Classloader of this class: sun.misc.Launcher$AppClassLoader@18b4aac2
>
> Class loader of Logging: sun.misc.Launcher$ExtClassLoader@3caeaf62
>
> Class loader of ArrayList: null

As we can see, there are three different class loaders here application; extension and bootstrap (displayed as null);

The **application class loader** load the class where the example method is contained. An application or system loader loads our own files in the classpath.

Next, the extension one loads the Logging class. **Extension class loaders load classes that are an extension of the standard core Java classes**.

Finally, the bootstrap one loads the ArrayList class. A bootstrap or primordial class loader is the parent of the all the others.

However, we can see that the last out for the ArrayList it display null in the output. **Thi is because the boostrap class loader is written in native code, not Java - so it doesn't show up as a Java class**. Due to this reason, the behavior of the bootstrap class loader will differ across JVMs.

### Boostrap Class Loader
Java classes are loaded by an instance of java.lang.ClassLoader. However, class loaders are classes themselves. Hence, the question is, who loads the java.lang.ClassLoader itself?

This is where the boostrap or primordial class loader comes into the picture.
It's mainly responsible for JDK internal classes, typically rt.jar and other core libraries located in *$JAVA_HOME/jre/lib* directory. Additionally, **Bootstrap class loader server as a parent of all the other ClassLoader instances**

### Extension Class Loader
The **extension class loader is a child of the bootstrap class loader and take care of loading the extensions of the standard core Java classes so that it's available to all applications running on the platform.

Extension class loader loads from the JDK extension directory, usually *$JAVA_HOME/lib/ext* directory or any other directory mentioned in the *java.ext.dirs* system property.

### System Class Loader
The system or application class loader, on the other hand takes care of loading all the application level classes into the JVM. **It loads files found in the classpath environment variable -classpath or -cp command line option**. Also it's child of Extension class loader.

## How Do Clas Loaders Work?
Class loaders are part of the Java Runtime Environment. When the JVM requests a class, the class loader tries to locate the class and load the class definition into the runtime using the fully qualified class name.

The **java.lang.ClassLoader.loadClass() methods is responsible for loading the class definition into runtime**. It tries to load the class based on a fully qualified name.

If the class isn't already loaded, it delegates the request to the parent class loader. This process happends recursively. 

Eventually, if the parent class loader doesn't find the clas then the child class will call *java.net.URLClassLoader.findClass() method to look for classes in the file system itself.

If the last child class loader isn't able to load the class either, it throw *java.lang.NoClassDefFoundError* or *java.lang.ClassNotFoundException*

> Note: **ClassNotFoundException** is an expception that occur when you try to load a class at runtime using **Class.forName()** or **loadClass()** methods and mentioned classes are not found in the classpath. It is thrown by the application itself. It is thrown by the methods like Class.forName(), loadClass() and findSystemClass(). It occurs when classpath is not update with required JAR files.
>
> **NoClassDefFoundError** is an error that occur when a particular class is present at compile time, but was missing at runtime. It is thrown by the JRE. It occurs when required class definition is missing at runtime.