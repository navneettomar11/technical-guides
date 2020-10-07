# What is NgRx?
NgRx is a framework for building reactive application in Anguler. NgRx provides state management, isolation of side effects, entity collection managment, router bindings, code generation, and developer tools that enhance developer experience when building many different type of applications.

# What is reactive programming?
In plain terms reactive programming is about non-blocking applications that are asynchronous and event-driven and require a small number of threads to scale vertically (i.e. within the JVM) rather than horizontally (i.e through clustering).

# Why NgRx for State management?
NgRx provides state management for creating maintainable explicit applications, by storing single state and the uses of actions in order to express state changes.
- **Serializability**: By normalizing state changes and passing them through observables, NgRx provides serializability and ensures state is predicably stored. This enable to save the state to an external storage, for example, localStorage.
- **Type Safety**: Type safety is promoted through the architecture with reliance on the Typescript compiler for programm correctness.
- **Encapsulation**: Using NgRx Effects and Store any interaction with external resources side effects, like network requests, web socket and any business logic can be isolated from the UI. This isolation allows for more pure and simple components, and keep the single responsiblity principle.
- **Testable**: Because Store uses pure function for changing state and selecting data from state, and ability to isolate side effects from the UI, the testing becomes very straightforward. NgRx also provides tests setup lile `provideMockStore` and `provideMockActions` for isolated tests, and a better test experience.
- **Performance**: Store is built on a single immutable data state, making change detection turn into a very easy task using on `OnPush` strategy. NgRx is also powered by memoized selector functions which optimize state query computations.


# When Should I Use NgRx for State Management
In paricular, you might use NgRx when you build an application with a lot of user interactions and multiple data sources, when managinng state in service are no longer sufficient.
A good substance that might answer the question - "Do I need NgRx", is the **SHARI** principle:
- **S**hared: state that is accessed by many components and services.
- **H**ydrated: state is persisted and rehydrated from external storage.
- **A**vailable: state that needs to be available when re-entering routes.
- **R**etrieved: state that must be retrieved with a side-effect.
- **I**mpacted: state that is impacted by actions from other sources.

However, realizing that using NgRx comes with some tradeoffs is also curcial. It is not meant to be the shortest or quickest way to write code and encourage its user the usage of many files. It is also often require a steep learning curve, including some good understanding of RxJs and Redux.

# Core Principles
- State is a single, immutable data structure.
- Components delegate responsibilities to side effects, which are handled in isolation.
- Type-safety is promoted throughout the architecture with reliance on Typescript compiler for program correctness.
- Actions and state are serializable to ensure state is predictably stored, rehydrated, and replayed.
- Promotes the use of functional programming when building reactive applications.
- Provide straightforward testing strategies for validating of functionality.

# Packages
- **Store** - RxJS powered state management for Angular apps, inspired by Redux.
- **Store Devtools** - Instrumentation for @ngrx/store enabling time-trave debugging.
- **Effects** - Side effect model for @ngrx/store.
- **Router Store** - Binding to connect the Angular Router to @ngrx/store.
- **Entity** - Entity State adapter for managing record collections.
- **NgRx Data** - Extension for simplified entity data management.
- **Schematics** - Scaffolding library for Angular application using NgRx libraries.

# State management
State management refers to the management of the state of one or more user interface controls such as text fields, OK button, radio buttons etc, in a graphical user interface. In this user interface programming technique, the state of one UI control depends on the state of other UI controls. For example, a state managed UI control such as a button will be in the enabled state when input fields have valid inputs values and the button will be in the disabled state when the inputs fields are empty or have invalid values.

![NgRx State Management](assets/state-management-lifecycle.png)

# @gnrx/store
Store is RxJS powered state management for Angular application, inspired by Redux. Store is a controlled state container designed to help write performant, consistent applications on top of Angular.

## Key Concepts
- `Actions` describe unique events that are dispatched from component and services.
- State changes are handled by pure function called `reducers` that take the current state and latest action to compute a new state.
- `Selectors` are pure function used to select, derive and compose pieces of state.
- State is accessed with the `Store` an obseravable of state and observer of actions.

