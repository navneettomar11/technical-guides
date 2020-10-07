# Why do we need Asynchronous work?
The simplest answer is we want to improve the user experience. We want to make our application more responsive. We want to deliver a smooth user experience to our users without freezing the main thread, slowing them down and we don't want to provide the jenky performance to our users.
To keep the main thread free we need to do a lot of heavy and time-consuming work we want to do in the background. We also want to do heavy work and complex calculations on our servers as mobile devices are not very powerful to do the heavy lifting. So we need asynchronous work for network operations.

# The ev

# Reactive Programming
In computing, reactive programming is a declarative programming paradigm concerned with data streams and the propagation of change. With this paradigm it is possible to express static (e.g. arrays) or dynamic (e.g. event emitters) data streams with ease, and also communicate that an inferred dependency within the associated execution model exists, which facilitates the automatic propagation execution model exists, which facilitates the automatic propagation of the changed data flow.

For example, in an imperative programming setting, a:=b + c would mean that a is being assigned the result of b + c in the instant the expression is evaluated, and later, the values of b and c can be changes with no effect on the value of a. On the other hand, in reactive programming, the value of a is automatically updated whenever the values of b or c change, without the program having to re-execute the statement a:= b + c to determine the presently assigned value of a.

Another example is a hardware description language such as Verilog where reactive programming enables changes to be modeled as they propagate through circuits.

Reactive programming has been proposed as a way to simplify the creation of interactive user interfaces and near-real-time system animation.

For examples, in a model-view-controller (MVC) architecture, reactive programming can facilitate changes in an underlying model that are reflected automatically in an associated view.

In simple words