# What's a design pattern? 
Design pattern are typical solutions to commonly occuring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.

You can't just find a pattern and copy it into your program, the way you can with off-the-shelf functions and libraries. The pattern is not a specific piece of code, but a general concept for solving a particular problem. You can follow the pattern details and implement a solution that suits the realities of your own program.

Patterns are often confused with algorithms, because both concepts describe typical solutions to some known problems. While an algorithm always defines a clear set of actions that can achieve some goal, a pattern is more high-level description of a solution. The code of the same pattern applied to two different programs may be different.

An analogy to an algorithm is a cooking recipe: both have clear steps to achieve a goal. On the other hand, a pattern is more like a blueprint: you can see what the result and its features are but the exact order of implementation is up to you.

## What does the pattern consist of ?
Most patterns are described very formally so people can reproduce them in many contexts. Here the sections that are usually present in a pattern description:
- **Intent** of the pattern briefly describe both the problem and the solution.
- **Motivation** further explains the problem and the solution the pattern makes possible.
- **Structure** of classes shows each part of the pattern and how they are related.
- **Code example** in one of the popular programming languages makes it easier to grasp the idea behind the pattern.

# Classification of patterns
Design patterns differ by their complexity, level of detail and scale of applicability to the entire system being designed. I like the analogy to road construction: you can make an intersection safer by either installing some traffic lights or building an entire multi-level interchange with underground passages for pedestrains.

The most basic and low-level patterns are often called _idioms_. They usually apply only to a single programming language.

The most universal and high-level patterns are architectural patterns. Developers can implement these patterns in virtually any langauage. Unlike other patterns, they can be used to design the archiecture of an entire application.

In addition, all patterns can be categorized by their intent, or purpose. There are three main groups of patterns:
1. **Creational patterns** provide object creation mechanisms that increase flexibility and reuse of existing code.
2. **Structural patterns** explain how to assemble objects and classes into larger structures, while keeping the structures flexibe and effcient.
3. **Behavioral patterns** takes care of effective communication and the assignment of responsibilities between objects.
