# Introduction
Redux is a predictable state container for Javascript apps. It helps you write applications that behave consistently, run in different environments (client, server and native), and are easy to test.

Redux is a state management framework that can be used with a number of different web technologies, including React.

In Redux, there is a single state object that's responsible for the entire state of your application. This means if you had a  React app with ten components, and each component had its own local `state` the entire state of your app would be defined by a single state object housed in the Redux `store`. This is the first important principle to understand when learning Redux: the Redux store is the single source of truth when it comes to application state.

This is also means that any time any piece of your app wants to update state, it meust to do so through the Redux store. The unidirectional data flow makes it easiser to track state management in your app.

The Redux store object provides several methods that allow you to interact with it. For example, you can retrieve the current `state` held in the Redux store object with the `getState()` method.


# Motivation
As the requirement for Javascript single-page applications have become increasingly complicated **our code must mangae more state than even before**. This state can include server response and cached data, as well as locally created data that has not yet been persisted to the server. UI state is also increasing in complexity,as we need to manage active routes, selected tabs, spinners, pagination controls and so on.

Managing this ever-changing state is hard. If a model can update another model, then a view can update a model, which updates another model, and this in turn might cause another view to update. At some point, you no longer understand what happens in your app as you have lost control over the when, why and how of its state. When a system is opaque and non-deterministic, it's hard to reproduce bugs or add new features.

As if this weren't bad enough, consider the new requirement becomming common in front-end product development. As developers, we are expected to handle optimistic updates, server-side rendering, fetching data before performing route transitions and so on. We find ourselves trying to manage a complexity that we have never had to deal with before.

This complexity is diffcult to handle as we're mixing two concepts that are very hard for the human min to reason about: mutation and asychronicity. I call them `Mentos and Coke`. Both can be great in separation but together they create a mess. Libraries like React attempt to solve this problem in the view layer by removing both asynchrony and direct DOM manipulation. However, managing the state of your data is left up to you. This is where Redux enters.

Follwing in the steps of Flux, CORS and Event Sourcing, Redux attempt to make state mutations predictable by imposing certain restriction on how and when updates can happen. These restrictions are reflected in the three principles of Redux.

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
This object is like a "model" except that there are no setters. This is so that diffetent parts of the code can't change the state arbitratily, causing hand-to-reproduc bugs.

To change something in the state, you need to dispatch an action. An action is a plain Javascript object(notice how we don't introduce any magic?) that describe what happened. Here are a few example actions.

```javascript
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

Enforcing that every change is described as an action. let us have a clear understanding of what's going on in the app. If something changes, we know why it changed. Actions are like breadcrumbs of what has happened. Finally, to tie state and actions together, we write function called a reducer. Again nothing magical about it - it's just a function that state state and actions as arguments, and returns the next state of the app. It would be hard to write such as function for a big, so we write smaller functions mangaging parts of it state.

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
Redux is pattern and library for managing and updating application state, using events called "actions"/ It serves as a centralized store for state that needs to be used across your entire application, with rules ensuring that the state can only be updated in a predictable fashion

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




# Redux Action
Since Redux is a statement