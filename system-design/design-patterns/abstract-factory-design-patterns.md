# Abstract Factory 

## Intent
Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

## Problem
Imagine that you're creating a furniture shop simulator. You code consists of classes that represent:
1. A family of related product say: `Chair` + `Sofa` + `CoffeeTable`.
2. Several variants of this family. Fo example, products `Chair` + `Sofa` + `CoffeeTable` are available in thses variants: `Modern`, `Victorian`, `ArtDeco`.

You need a way to create individual furniture objects so that they match other objects of the same family. Customers get quite mad when they receive non-matching furniture.

Also, you don't want to change existing code when adding new products or families of products to the program. Furniture vendors update their catalogs very often, and  you wouldn't want to change the code each time it happens.

If an application is to be portable, it need to encapsulate platform dependencies. These `platforms` might include: window system, operating system, database etc. Too often, this encapsulation is not engineered in adavance, and lots of `#ifdef` case statements with options for currently supported platforms begin to procreate like rabbits throughout the code.


## Solution
The first thing the Abstract Factory pattern suggest is to explicitly declare interfaces for each distinct Product of the product family (e.g. chair, sofa or coffeetable). Then you can make all variants of product follow those interfaces. For example, all chair variabnts can implement the `Chair` interface; all coffee table variant can implement the `CoffeeTable` interface and so on.

![Solution01](https://refactoring.guru/images/patterns/diagrams/abstract-factory/solution1.png)

The next move is to declare the _AbstractFactory_- an interface with a list of creation methods for all products that are part of the product family(for example, createChair, createSofa and createCoffeeTable). These methods must retunr **abstracr** product types represented by the interfaces we extracted previously: `Chair`, `Sofa`, `CoffeeTable` and so on.

![Solution2](https://refactoring.guru/images/patterns/diagrams/abstract-factory/solution2.png)

Now, how about the product variants? For each variant of a product family, we create a separate factory class based on the `AbstractFactory` interface. A factory is a class that returns products of a particular kind. For example, the `ModernFurnitureFactory` can only create `ModernChair`, `ModernSofa` and `ModernCoffeeTable` objects.

The client code has to work with both factories and products via their respective abstract interfaces. This lets you chagne the type of a factory that you pass to the client code, as well as the product variant that the client code receives, without breaking the actual client code.

Say the client wants a factory to produce a chair. The client doesn't have to be aware of the factory's class nor does it matter what kind of chair it gets. Whether it's a Modern model or a Victorian-style chair, the client must treat all chairs in the same manner, using the abstract `Chair` interface. With this approach, the only thing that the client knowns about the chair is  that it implements the `sitOn` method in some way.  Also, whichever variant of the chair is returned, it'll always match the type of sofa or coffee table produced by the same factory object.

There's one more thing left to clarify: if the client is only exposed to the abstract interfaces, what creates the actual factory object? Usually the application creates a concrete factory object at the initializiation stage. Just before that, the app must select the factory type depending on the configuration or the environment settings.

## Structure

![Structure of Abstract Factory](https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure.png)

1. **Abstract Products** declare interfaces for a set of distinct but related products which make up a product family.
2. **Concrete Products** are various implementations of abstract products, grouped by variants. Each abstract product(chair/sofa) must be implemented in all given variants(Victorian/Modern).
3. The **Abstract Factory** interface declares a set of methods for creating each of the abstract products.
4. **Concrete Factories** implementation creation methods of the abstract factory. Each concrete factory corresponds to a specific variant of products and creates only those product variants.
5. Although concrete factories instanitiate concreate product signatures of their creation methods must return corresponding abstract products. This way the client coe that uses a factory doesn't get coupled to the specific variant of the product it get from a factory. The Client can work with any concreate factory/product variant, as long as it communicates with their objects via abstract interfaces.

## Pseudocode
This example illustrates how the Abstract Factory pattern can be used for creating cross-platform UI elements without coupling the client code to concrete UI classes, which keeping all created elements consistent with a selected operating system.

![Pseucode](https://refactoring.guru/images/patterns/diagrams/abstract-factory/example.png)

The same UI elements in a cross-platform application are expected to behave similarly, but look a little bit different under different operating systems. Moreover, its you job to make sure that the UI elements match the style of the current operating system. You wouldn't want your program to render macOS controls when its executed in Windows.

The Abstract Factory interface declares a set of creation methods that the client code can use to produce different types og UI elements. Concrete factories correspond to specfic operating systems and create UI elements that match  that particular OS.

It works like this: when an application launches, it checks the type of the current operating system. The app uses this information to create a factory object from a class that matches the operating system. The rest of the code uses this factory to create UI elements. This prevents the wrong elements from being created.

With this approach, the client code doesn't depend on concrete classes of factories and UI elements as long as it works with these objects via their abstract interfaces. This also lets the client code support other factories or UI eleent that you might add in the future.

As a result, you don't need to modify the client code each time you add a new variation of UI elements to your app. You just have to create a new factory that produces theses elements and slightly modify the app's intialization code so it selects that class when appropriate.

```java
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WinFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new WinButton();
    }

    @Overribe
    public Checkbox createCheckbox() {
        return new WinCheckbox();
    }
}


class MacFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new MacButton();
    }

    @Overribe
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}

interface Button {
    void paint();
}

class WinButton implements Button {
    @Override
    public void paint() {
        System.out.println("Render Window Button");
    }
   
}

class MacButton implements Button {
    @Override
    public void paint() {
        System.out.println("Render Mac Button");
    }
}

interface Checkbox {

    void paint()
}

class MacCheckbox implements Checkbox {
    @Override
    public void paint() {
        System.out.println("Render Mac Checkbox");
    }
}


class WinCheckbox implements Checkbox {
    @Override
    public void paint() {
        System.out.println("Render Window Checkbox");
    }
}

class AbstractFactory  {

    private GUIFactory guiFactory;
    private Button button;

    public AbstractFactory(GUIFactory factory) {
        this.factory = factory;
    }

    public void createUI() {
        this.button = guiFactory.createButton();
    }

    pulbic void paint() {
        this.button.paint();
    }
}

class ApplicationConfigurator {

    public static void main(String[] args) {
        private GUIFactory factory;
        String os = args[0];
        if(os.equals("Win")) {
            factory = new WinFactory();
        } else if (os.equal("Mac")) {
             factory = new MacFactory();
        }

        AbstractFactory abstractFactory = new AbstractFactory(factory);

    }
}
```

## Applicability
**Use the Abstract Factory when your code needs to work with various familes of related products, but you don't want it to depend on the concrete class of thoses products- they might be unknown beforehand or you might be unknown or you simply want to allow for future extensibility.**

The Abstract Factory provides you with an interface for creating object from each class of the product family. As long as your code creates objects via this interface you don't have to worry about creating the wrong variant of a product which doesn't match the products already created by your app.
- Consider implementing the Abstract Factory when you have a class with a set of Factory methods that blur its primary responsiblity.
- In a well-designed program each class is responsible only for one thing. When a class deals with multiple product types, it may be worth extracting its factory method into a stand-alone factory class or a full blown Abstract Factory implementation.


Source: [https://refactoring.guru/design-patterns/abstract-factory](https://refactoring.guru/design-patterns/abstract-factory)

