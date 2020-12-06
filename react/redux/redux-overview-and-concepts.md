# Introduction
Redux is a predictable state container for Javascript apps. It helps you write applications that behave consistently, run in different environments (client, server and native), and are easy to test.

Redux is a state management framework that can be used with a number of different web technologies, including React.

In Redux, there is a single state object that's responsible for the entire state of your application. This means if you had a  React app with ten components, and each component had its own local `state` the entire state of your app would be defined by a single state object housed in the Redux `store`. This is the first important principle to understand when learning Redux: the Redux store is the single source of truth when it comes to application state.

This is also means that any time any piece of your app wants to update state, it must to do so through the Redux store. The unidirectional data flow makes it easiser to track state management in your app.

The Redux store object provides several methods that allow you to interact with it. For example, you can retrieve the current `state` held in the Redux store object with the `getState()` method.


# Motivation
As the requirement for Javascript single-page applications have become increasingly complicated **our code must mangae more state than even before**. This state can include server response and cached data, as well as locally created data that has not yet been persisted to the server. UI state is also increasing in complexity,as we need to manage active routes, selected tabs, spinners, pagination controls and so on.

Managing this ever-changing state is hard. If a model can update another model, then a view can update a model, which updates another model, and this in turn might cause another view to update. At some point, you no longer understand what happens in your app as you have lost control over the when, why and how of its state. When a system is opaque and non-deterministic, it's hard to reproduce bugs or add new features.

As if this weren't bad enough, consider the new requirement becomming common in front-end product development. As developers, we are expected to handle optimistic updates, server-side rendering, fetching data before performing route transitions and so on. We find ourselves trying to manage a complexity that we have never had to deal with before.

This complexity is diffcult to handle as we're mixing two concepts that are very hard for the human min to reason about: mutation and asynchronicity. I call them `Mentos and Coke`. Both can be great in separation but together they create a mess. Libraries like React attempt to solve this problem in the view layer by removing both asynchrony and direct DOM manipulation. However, managing the state of your data is left up to you. This is where Redux enters.

Following in the steps of Flux, CORS and Event Sourcing, Redux attempt to make state mutations predictable by imposing certain restriction on how and when updates can happen. These restrictions are reflected in the three principles of Redux.

# Core concepts
Imaging your app's state is described as a plain object. For example, the state of a todo app might look like.

```javascript
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}
```
This object is like a "model" except that there are no setters. This is so that different parts of the code can't change the state arbitrarily, causing hand-to-reproduce bugs.

To change something in the state, you need to dispatch an action. An action is a plain Javascript object(notice how we don't introduce any magic?) that describe what happened. Here are a few example actions.

```javascript
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

Enforcing that every change is described as an action. let us have a clear understanding of what's going on in the app. If something changes, we know why it changed. Actions are like breadcrumbs of what has happened. Finally, to tie state and actions together, we write function called a reducer. Again nothing magical about it - it's just a function that take state and actions as arguments, and returns the next state of the app. It would be hard to write such as function for a big, so we write smaller functions mangaging parts of it state.

```javascript
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map((todo, index) =>
        action.index === index
          ? { text: todo.text, completed: !todo.completed }
          : todo
      )
    default:
      return state
  }
}
```
And we write another reducer that manages the complete state of our app  by calling those two reducers for the corresponding state keys.

```javascript
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}
```
This is basically the whole idea of Redux. Note the we haven't using any Redux API. It comes with a few utilities to facilitate this pattern, but the main idea is that you describe how your state is updated over time in response to action objects and 90% of the code you just plain Javascript, with no use of Redux itself, it APIs, or any magic.

# Three Principles
Redux can be described in three fudamental principles:

## Single source of truth
The global state of your application is stored in an object tree within a single store.

This makes is easy to create universal apps, as the state from your server can be serialized and hydrated into the client with no extra coding effort. A single state tree also makes it easier to debug or inspect an application; it also enables you to presist your app's state in development for a faster development cycle. Some functionaility which has been traditionally diffcult to implement - Undo/Redo, for example - can suddenly become trivial to implement, if all  of your state is stored in a single tree.

```javascript
console.log(store.getState())

/* Prints
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
*/
```

## State is read-only
The only way to change the state is to emit an action, an object describing what happened.

This ensures that neither the views nor the network callbacks will ever write directly to the state. Instead they express an intent to transform the state. Because all changes are centralized and happen one by one in strict order, there are no subtle race conditions to watch for. As actions are just plain objects, they can be logged, serialized stored, and later replayed for debugging or testing purposes.

```javascript
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
})

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

## Changes are made with pure functions
To specify how the state tree is transformed by actions, you write pure reducers.

Reducers are just pure functions that take the previous state and an action and return the next state. Remember to return new state objects, instead of mutating the previous state. You can start with a single reducer and as your app grows, split it off into smaller reducers that manage specific parts of the state tree. Because reducers are just functions, you can control the order in which they are called, pass additional data, or even make reusable reducers for common tasks such as pagination,

```javascript
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}

import { combineReducers, createStore } from 'redux'
const reducer = combineReducers({ visibilityFilter, todos })
const store = createStore(reducer)
```
# What is Redux ?
Redux is pattern and library for managing and updating application state, using events called "actions". It serves as a centralized store for state that needs to be used across your entire application, with rules ensuring that the state can only be updated in a predictable fashion

## Why should I use Redux?
Redux helps you manage "global" state - state that is need across many parts of your application.

The patterns and tools provided by Redux make it easier to understand when, where, why and how the state in your application is being updated, and how application logic will behave when those chages occur. Redux guides you toward writing code that is predictable and testable, which helps give you confidence that your application will work as expected.

## When Should I use Redux ?
Redux helps you deal with shared state management, but like any tool, it has tradeoffs. There's more concept to learn and more code to write. It also add some indirection to your code, and ask you to follow certain restrictions. It's trade-off between short term and long term productivity.

Redux is more useful when:
- You have large amounts of application state that are needed in many places in the app.
- The app state is updated frequently over time.
- The logic to update the state may be complex.
- The app has a medium or large-size codebase, and might be worked on by many people.

> Not all apps need Redux. Take some time to think about the kind of app you're building and decide what tools would be best to help solve the problem you're working on.

# Redux Terms and Concepts
Before we dive into some actual code, let's talk about some of the terms and concepts you'll need to know to use Redux.

## State Management
Let's start by looking at a small React Counter Component. It tracks a number in component state, and increments the number when a button is clicked.

```javascript
function Counter() {
  // State: a counter value
  const [counter, setCounter] = useState(0)

  // Action: code that causes an update to the state when something happens
  const increment = () => {
    setCounter(prevCounter => prevCounter + 1)
  }

  // View: the UI definition
  return (
    <div>
      Value: {counter} <button onClick={increment}>Increment</button>
    </div>
  )
}
```
It is a self-contained app with the following parts:
- The **state**, the source of truth that drives our app.
- The **view**, a declarative description of the UI based on the current state.
- The **actions**, the event occur in the app based on user input and trigger updates in the state.

This is a small example of "**one-way-data_flow**":
- State describes the condtion of the app at a specific point in time.
- The UI is rendered based on that state.
- When something happens (such as a user clicking a button), the state is updated based on what occured.
- The UI re-renders based on the new state.

![https://redux.js.org/assets/images/one-way-data-flow-04fe46332c1ccb3497ecb04b94e55b97.png](https://redux.js.org/assets/images/one-way-data-flow-04fe46332c1ccb3497ecb04b94e55b97.png)

However, the simplicity can break down when we have **multiple components that need to share and use the same state, especially if those components are located in different parts of the application. Sometimes this can be solved by "lifting state up" to parent components, but tht doesn't always help.

One way to solve this is to extract the shared state from the components and put it into a centralized location outside the component tree. With this, our component tree becomes a big "view" and any component can access the state or trigger actions, no matter where they are in the tree!

By defining and separating the concepts involved in state mangement and enforcing rules that maintain independence between views and states, we give our code more structure and maintainability.

This is the basic idea behind Redux: a single centralized place to contain the global state in your application and specific patterns to follow when updating that state to make the code predictable.

## Immutablility
"Mutable" means "changeable". If something is "immutable" it can never be changed.

Javascript object and arrays are all mutable by default. If I create an object I can change the contents of its fields. If I create an array, I can change the content as well:

```javascript
const obj = {a: 1, b: 2};
//still the same object outside, but the content have changed
obj.b = 3

const arr = ['a', 'b'];
arr.push('c');
arr[1] = 'd';
```

This is called mutating th object or array. It's the same object or any array reference in memory, but now the contents inside the object have changed.

**In order to update values immutably, your code must make copies of existing objects/arrays, and then modify the copies.**

We can do this by hand using Javascript's array/object spread operators, as well as array methods that return new copies of the array instead of the original array:

```javascript
const obj = {
  a: {
    // To safely update obj.a.c, we have to copy each piece
    c: 3
  },
  b: 2
}

const obj2 = {
  // copy obj
  ...obj,
  // overwrite a
  a: {
    // copy obj.a
    ...obj.a,
    // overwrite c
    c: 42
  }
}

const arr = ['a', 'b']
// Create a new copy of arr, with "c" appended to the end
const arr2 = arr.concat('c')

// or, we can make a copy of the original array:
const arr3 = arr.slice()
// and mutate the copy:
arr3.push('c')
```
**Redux expects that all state updates are done immmutably.** We'll look at where and how this is important a bit as well as some easier ways to write immutable update logic.

## Terminology
There are some some important Redux terms that you'll need to be familiar with before we continue:

### Actions
An action is a plain Javascript object that has a `type` field. **You can think of an action as an event that describes something that happened in the application.**

The `type` field should be a string that gives this action a descriptive name like `"todo/todoAdded"`. We usually write that type string like `"domain/eventName"`, where the first part is the feature or category that this action belong to, and the second part is the specific thing that happened.

An action object can have other fields with additional information about what happened. By convention, we put that information in a field called `payload`.

A typical action object might look like this:
```javascript
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk'
}
```
### Action Creators
An action creator is a function that creates and return an action object. We typically use these so we don't have to write the action object by hand every time.
```javascript
const addTodo = text => {
  return {
    type: 'todos/todoAdded',
    payload: text
  }
}
```
### Reducers
A reducer is a function that receives the current `state` and an `action` object, decides how to update the state if necessary, and return the new state: `(state, action) => newState`. **You can think of a reducer as an event listener which handles events based on the received action(event) type.**
Reducers must always follow some specific rules:
- They should only calculate the new state value based on the state and action arguments.
- They are not allowed to modify the existing state. Instead, they must make immutable updates, by copying the existing state and making changes to the copied values.
- They must not do an asynchronous logic, calculate random values or cause other "Side effects".

The logic inside reducer function typically follows the same series of steps:
- Check to see if the reducer care about this action.
  - If so, make a copy of the state, update the copy with new values and return it.
- Otherwise, return the existing state unchanged.

Here's small example of a reducer, showing the steps that each reducer should follow:
```javascript
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/increment') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```
Reducers can use any kind of logic inside to decide what the new state should be `if/esle`, `switch`, loops and so on.

> ## Detail Explainaton: Why are they called 'Reducers'?
>
> The `Array.reduce()` method lets you take an array of values, process each item in the array one at a time, and return a single final result. You can think of it as "reducing the array down to one value". 
>
> `Array.reducer()` takes a callback function as an argument, which will be called one time for each item in the array. It takes two arguments:
> - `previousResult`: the value that your callback return last time.
> - `currentItem`: the current item in the array
> 
> The first time that the callback runs, there isn't a `previousResult` available, so we need to alsp pass in an initial value that will be used as the first `previousResult`. 
>
> If we wanted to add together an array of numbers to find out what the total is, we could write a reduce callback that look like this:
>
> ```javascript
> const numbers = [2, 5, 8]
> 
> const addNumbers = (previousResult, currentItem) => {
>   console.log({ previousResult, currentItem })
>   return previousResult + currentItem
> }
> 
> const initialValue = 0
> 
> const total = numbers.reduce(addNumbers, initialValue)
> // {previousResult: 0, currentItem: 2}
> // {previousResult: 2, currentItem: 5}
> // {previousResult: 7, currentItem: 8}
> console.log(total)
> // 15
> ```
>  Notice that this `addNumber` "reduce callback" function doesn't need to keep track of anything itself. It takes `previousResult` and `currentItem` arguments, does something with them, and returns a  new result value.
> 
> **A Redux reducer function is exactly the same idea as this 'reduce callback' function!** It takes a "previous result" (the `state`) and the "current item" (the `action` object), decides a new state value based on those arguments, and returns that new state.
>
> If we were to create an array of Redux actions, call `reduce()`, and pass in a reducer function, we'd get a final the same way:
>
> ```javascript
> const actions = [
>  { type: 'counter/increment' },
>  { type: 'counter/increment' },
>  { type: 'counter/increment' }
>]
>
>const initialState = { value: 0 }
>
>const finalResult = actions.reduce(counterReducer, initialState)
>console.log(finalResult)
>// {value: 3}
> ```
>
> We can say that **Redux reducers reduce a set of actions(over time) into a single state.** THe difference is that with `Array.reduce()` it happen all at once, and Redux, it happens over the lifetime of your running app.

### Store
The current Redux application state lives in an object called the **store**.

The store is created by passing in a reducer, and has a method called `getState` that returns the current state value:

```javascript
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

### Dispatch
The Redux store has a method called `dispatch`. **The only way to update the state is to call `store.dispatch()` and pass in an action object**. The store will run its reducers function and save the new state value inside, and we can call `getState()` to retrive the updateed value:

```javascript
store.dispatch({ type: 'counter/increment' })

console.log(store.getState())
// {value: 1}
```
You can think of dispatching actions as "triggering an event" in the application. Something happened, and we want the store to know about it. Reducers act like event listeners, and when they hear an action they are intereseted in, they update the state in response.

We typically call action creators to dispatch the right action:
```javascript
const increment = () => {
  return {
    type: 'counter/increment'
  }
}

store.dispatch(increment())

console.log(store.getState())
// {value: 2}
```

## Selectors
Selectors are functions that know how to extract specific pieces of information from a store state value. As an application grows bigger, this can help avoid repeating logic as different parts of the app need to read the same data:
```javascript
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```

# Redux Application Data Flow
Eariler, we talked about "one-way data flow", which describes this sequence of steps to update the app:
- State describe the condition of the app at a specific point in time
- The UI is rendered based on the state
- When something happens (such as a user clicking a button), the state is updated based on what occured.
- The UI re-renders based on the new state

For Redux specifically, we can break these steps into more detail:
- Initial setup:
  - A Redux store is created using a root reducer function.
  - The store calls the root reducer once, and saves the return value as its initial `state`.
  - When the UI is first rendered, UI components access the current state of the Redux store and use the data to decide what to render. They also subscribe to any future store updates so they can know if the state has changed.
- Updates:
  - Something happens in the app, such as a user clicking a button.
  - The app code dispatches an action to the Redux store, like `dispatch({type: 'counter/increment'})`
  - The store runs the reducer function again with the previous `state` and the current `action`, and the return value as the new `state`.
  - The store notifies all parts of the UI that are subscribed that the store has been updated.
  - Each UI component that needs data from the store checks to see if the parts of the state they need have changed.
  - Each component that see its data has changed forces a re-render with the new data, so it can update what's shown on the screen.

Here's what that data  flow look like visually:
![Redux data flow](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

Source: [https://redux.js.org](https://redux.js.org/tutorials/essentials/part-1-overview-concepts)