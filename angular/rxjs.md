Reactive programming is an asynchronous programming paradigm concerned with data streams and propagation of change. RXJS (Reactive Extension for Javascript) is a library for reactive programming using observables that makes it easier to compose asynchronous or call-back based code.

RxJS is a library for composing asynchronous and event-based programs by using observable sequences. It provides one core type, the Observable, satellite types(Observer, Schedulers, Subjects) and operators inspired by Array@extras(map, filter, reduce, every,etc) to allow handling asynchronous event as collections.

> Think of RxJS as Lodash for events.

ReactiveX combines the `Observer pattern` with the `Iterator pattern` and `functional programming with collections` to fill the need for an ideal way of managinig sequences of events.

The essential concepts in RxJS which solve async event managment are:
- **Observable**: represents the idea of an invokable collection of future values or event.
- **Observer**: is a collection of callbacks that knows how to listen to values delivered by the Observable.

