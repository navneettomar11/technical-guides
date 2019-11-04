# Cohesion 
In computer programming, cohesion refers to the degree to which the elements inside a module belong together. In one sense, it is a measure of the strength of relationship between the methods and data of a class and some unifying purpose or concept served by that class. In another sense, it is a measure of the strength of relationship between the class's methof and data themselves.
Cohesion is an ordianal type of measurement and is usually described as "high cohesion" or "low cohesion". Modules with high cohesion tend to be preferable, because high cohesion is associated with several desirable traits of software including robustness, reliability, reusability, and understandability. In contrast, low cohesion is associated with undesirable traits such as being diffcult to maintain, test, reuse or even understand.

Cohesion is often contrasted with coupling, a different concept. High cohesion often correlates with loose coupling and vice versa. 

## High Cohesion
In object-oriented programming, if the methods that serve a class tend to be similar in many aspects, then the class is said to have high cohesion. In a highly cohesive system code readability and resulability is increased, while complexity is manageable.

Cohesion is increased if:
- The functionalities embedded in a class, accessed through its methods, have much in common.
- Methods carry out a small number of related activities, by avoiding coarsely grained or unrelated sets of data.

Advantages of high cohesion(or "strong cohesion") are:
- Reduced module complexity (they are simpler, having fewer operations).
- Increased system maintainability, because logical changes in the domain affect fewer modules, and because changes in one module require fewer changes in other modules.
- Increased module reusability, because application developers will find the component they need more easily among the cohesive set of operations provide by the module.

