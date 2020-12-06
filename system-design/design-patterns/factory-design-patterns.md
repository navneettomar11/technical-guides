# Factory Method

## Intent
Factory Method is a creational design pattern that provides an interface for creating object in a superclass, but allows subclasses to alter the type of objects that will be created.

## Problem 
Imagine that you're creating a logistic management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the `Truck` class.

After a while, your app become pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

Great news, right? But how about the code? At present most of your code is coupled to the `Truck` class. Adding `Ships` into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

As a result, you will end up with pretty nasty code, riddled with conditionals that switch the app's behavior depending on the class of transportation objects.

## Solution
The Factory Method pattern suggests that you replace direct object construction calls (using the `new` operator) with calls to a special factory method. Don't worry: the objects are still are created via the `new` operator, but it's being called from withing the factory method. Objects returned by a factory method are often referred to as _products_.

![Subclass can alter the class of objects being returned by the factory method](https://refactoring.guru/images/patterns/diagrams/factory-method/solution1.png)

At first glance, this change may look pointless: we just moved the constructor call from one part of the program to another. However, consider this: now you can override the factory method in a subclass and the change the class of products being created by the method.

There's a slight limitation though: subclasses may return different types of products only if these products have a common base class or interface. Also, the factory method in the base class should have its return type declared as this interface.

![All products must follow the same interface](https://refactoring.guru/images/patterns/diagrams/factory-method/solution2-en.png)

For example, both `Truck` and `Ship` classes should implement the `Transport` interface, which declares a method called `deliver`. Each class implements this method differently: trucks deliver cargo by land, ship deliver cargo by sea. The factory method in the `RoadLogistics` class return truck objects, whearas the factory method is the `SeaLogistivs` class returns ships.

The code that uses the factory method(often called the client code) doesn't see a difference between the actual products returned by various subclasses. The client treats all the products as abstract Transport. The client knowns that all transport objects are supposed to have the `deliver` method, but exactly how it works isn't important to the client.

## Structure

![Factory Design Pattern Structure](https://refactoring.guru/images/patterns/diagrams/factory-method/structure.png)

1. The Product declares the interface which is common to all objects that can be produced by the create and its subclasses.
2. Concrete Products are different implementations of the product interface.
3. The Creator class declares the factory method that returns new product objects, It's important that the return type of this method matches the product interace. You can declared the factory method as abstract to force all subclasses to implement their own version of the method. As an alternative, the base factory method can return some default type.
4. Concreate Creators override the base factory method so it returns a different type of product.

## Pseudocode
This example illustrates how the **Factory Method** can be used for creating cross-platform UI elements without coupling the client code to concreate UI classes.

![The cross-platform dialog example](https://refactoring.guru/images/patterns/diagrams/factory-method/example.png)

The base dialog class uses different UI element to render its window. Under various operating systems, these elements may look a little bit different, but they should still behave consistently. A button in Windows is still a button in Linux.

When the factory method comes into play, you don't need to rewrite the logic of the dialog for each operating system. If we declare a factory method that produces buttons inside the base dialog class, we can later create a dialog subclass that returns Window-styled buttons from the factory method. The subclass then inherits most of the dialog's code from the base class, but, thanks to the factory method, can render Window-looking buttons on the screen.

For this pattern to work, the base dialog class must work with abstract buttons: a base class or an interface that all concrete button follow. This way the dialog's code remains functional, whichever type of buttons it works with.

```java

interface Button {
    render();
    onCick(func);
}

public class WindowButton implements Button {...}
public class HtmlButton implements Button {...}

public abstract class Dialog {
    abstract Button createButton();

    public void render() {
        Button okButton = createButton();
        okButton.onClick(closeDialog);
        okButton.render();
    }
}

public class WindowDiloag extends Dialog {

    public Button createButton() {
        return new WindowButton();
    }
}

public class WebDialog extends Dialog {
     public Button createButton() {
        return new HtmlButton();
    }
}

public class DialogFactory {

    public Dialog getDialog(String osName) {
        switch(osName) {
            case "Windows": return new WindowDialog();
            case "Web": return new WebDialog();
            default:
                throw new RuntimeException("Invalid OS");
        }
    }
}
```

## Applicability

**Use the Factory method when you don't know beforehand the exact types and dependencies of the objects your code should work with.**
The Factory method separates product construction code from the code that actually uses the product. Therefore it's easier to extend the product construction code independently from the rest of the code.
For example, to add a new product type to the app, you'll only need to create a new creator subclass and override the factory method in it.

**Use the Factoroy method when you want to provide users of your libray or framework with a way to extends its internal components.**
Inheirtance is probably the easiest way to extend the default behavior of a library or framework. But how would the framework recognize that your subclass should be used instead of a standard component?

Let's see how that would work. Imagine that you write an app using an open source UI framework. Your app should have round buttons, but the framework only provides square ones. You extend the standard `Button` class with a glorious `RoundButton` subclass. But now you need to tell the main `UIFramework` class to use the new button subclass instead of a default one.

To achieve this, you create a subclass `UIWithRoundButtons` from a base framework class and override its `createButton` method. While this method return `Button` object in the base class, you make your subclass return `RoundButton` objects. Now use the `UIWithRoundButtons` class instead of `UIFramework`.

**Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.**
You often experience this need when dealing with large resource-intensive objects such as database connections, file systems and network resources.
Let think about what has to be done to reuse an existing object:
1. First, you need to create some storage to keep track of all of the created objects.
2. When someone requests an object, the program should look for a free object inside that pool.
3. ... and then return it to the client code.
4. If there are no free objects, the program should create a new one (and add it to the pool).

That's a lot of code! And it must all be put into a single place so that you don't pollute the program with duplicate code.

Probably the most obvious and convenient place where this code could be place is the constructor of the class whose objects we're trying to reuse. However, a constructor must always return new object by definition. It can't return existing instances.

Therefore, you need to have a regular method capable of creating new objects as well as existing ones. That sounds very much like a factory method.

## Pros and Cons
|Pros| Cons|
|----|-----|
|You avoid tight coupling between creator and the concrete products| The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you're introducing the pattern into an existing hierarchy of creator classes.|
|_Single Responsiblity Principle_. You can move the product creation code into one place in the program, making the code eaiser to support||
|_Open/Closed Principle_. You can introduce new types of products into the program without breaking existing client code||
|

> Source: [https://refactoring.guru/design-patterns](https://refactoring.guru/design-patterns)