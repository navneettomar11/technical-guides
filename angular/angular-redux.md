# Redux

If you haven't heard of Redux yet you can [read a bit about it on the offical website](http://redux.js.org/). Web application data architecture is evolving and the traditional ways of structuring data aren't quite adequate for large webapps. Redux has been extremely popular because its both powerful and easy to understand.

Data architecture can be a complex topic and so Redux's best feature is probably it simplicity. If you strip Redux down to the essential core Redux is fewer than 100 lines of code.

We can build rich, easy to understand web apps by using Redux as the backbone of our application. But first, let's walk through how to write a minimal Redux.

> There are several attempts to use Redux or create a Redux inspired system that works  with Angular. Two notable examples are:
> - ngrx/store and
> - angular2-redux
> `ngrx` is a Redux-inspired architecture that is heavily observables-based. angular2-redux uses Redux itself as a dependency, and adds some angular helpers (dependency-injection, observable wrappers).
> Here we're not going to use either. Instead we're goiung to use Redux directly in order to show the concepts without introducing a new dependency. That said both of these libraries may be helpful to you when writing your apps.

# Redux: Key Ideas
The key ideas of Redux are this:
- All of your application's data is in single data structure called the state which is held in the store.
- Your app reads the state from this store.
- This store is never mutated directly.
- User interaction (and other code) fires action which describe what happened.
- A new state is created by combining he old state and the action by fundction called the reducer.

[!redux-reducer](assets/redux-diagram.png)

If the above bullet isn't clear yet, don't worry about it - putting these ideas into pratice is the goal of the rest of this post.

# Core Redux Ideas
## What's a reducer?
Let's talk about the reducer first. Here's the idea of a reducer: it takes the old state and an action and returns a new state.

A reducer must be a [pure function](https://en.wikipedia.org/wiki/Pure_function). That is:
1. It must not mutate the current state directly.
2. It must not use any data outside of its arguments.

Put another way, a pure function will always **return the same value, given the same set of arguments**. And a pure function won't call any functions which have an effect on the outside world, e.g. no database calls, no HTTP calls, and no mutating outside data structures.

Reducers should always treat the current state as **read-only**. A reducer **does not change the state** instead, it **return a new state**.(Often this new state will start with a copy of old state, but let's not get ahead of ourselves).

Let's define our very first Reducer. Remember, there are three things involved: 
1. An `Action`, which defines what to do (with optional arguments)
2. The `State` which stores all of the data in our application.
3. The `Reducer` which takes the state and the Action and returns a new state.

## Defining Action and Reducer Interfaces

Since we're using TypeScript we an to make sure this whole process is typed, so let's setup an interface for our `Action` and our `Reducer`: 

### The Action Interface
Our Action interface looks like this:
```typescript
interface Action {
    type: string;
    payload?: any;
}
```
Notice that our Action has two fields:
1. `type` and
2. `payload`

The `type` will be an identifying string that describes the action like `INCREMENT` or `ADD_USER`. The payload can be object of any kind. The `?` on `payload` means that this field is optional.

### The Reducer Interface
Our `Reducer` interface look like this:
```typescript
interface Reducer<T>{
    (state: T, action: Action): T;
}
```
Our Reducer is using a feature of TypeScript called generics. In the case `T` is the type of the `state`. Notice that we're saying that valid Reducer has a function which takes a state (of type `T`) and an `action` and returns a new state (also of type `T`).

## Creating Our First Reducer
The simplest possible reducer return the state itself.(You might call this the identity reducer because it applies the identity function on the state. This is the default case for all reducers).

```typescript
let reducer: Reducer<number> = (state: number, action Action) => {
    return state;
} 
```
Notice that this Reducer makes the generic type concrete to the `number` by the syntax Reducer<number>. 
We're not using the `Action` yet, but let's try this `Reducer` just the same.

## Running Our First Reducer
Let's put it all together and run this reducer:
```typescript
interface Action{
    type: string;
    payload?: any;
}

interface Reducer<T>{
    (state: T, action: Action): T;
}

let reducer: Reducer<number> = (state: number, action: Action) => {
    return state;
}

console.log(reducer(0, null)); // -> 0
```
It seems almost silly to have that as a code example, but it teaches us our first principle of reducers.
**By default, reducers return the original state.**
In this case, we passed a state of the number `0` and a `null` action. The result from this reducer is the state 0.

But let's do something more intersesting and make out state change.

## Adjusting the Counter with actions
Eventually our state is going to be much more sophisiticated than a single number. We're going to be holding the all of the data for our app in the `state`, so we'll need better data structure for the state eventually.

That said, using a single number for the state let us focus on other issues for now. So let's continue with the idea that our `state` is simply a single number that is storing a counter.

Let's say we want to be able to change the `state` number. Remember that in Redux we do not modify the state. Instead, we create actions which instruct the reducer on how to generate a new state.

Let's create an `Action` to change our counter. Remember that the only required property is a `type`. We might define our first action like this:
```typescript
let incrementAction: Action = {type: 'INCREMENT'}
```
We should also create a second action that instruct our reducer to make the counter smaller with: 
```typescript
    let decrementAction: Action = {type: 'DECREMENT'}
```
Now that we have these actions, let's try using them in our reducer:

```typescript
let reducer: Reducer<number> = (state: number, action: Action) => {
    if(action.type === 'INCREMENT') {
        return state + 1;
    }
    if(action.type === 'DECREMENT') {
        return state - 1;
    }
    return state;
}
```
And now we can try out the whole reducer:
```typescript
let incrementAction: Action = {type: 'INCREMENT'};
console.log(reducer(0, incrementAction)); // -> 1
console.log(reducer(1, incrementAction)); // -> 2

let decrementAction: Action = {type: 'DECREMENT'};
console.log(reducer(100, decrementAction)); // -> 99
```
Neat! Now the new value of the state is returned according to which action we pass into the reducer.

## Reducer switch
Instead of having so many `if` statement, the common practice is to convert the reducer body to a `switch` statement:
```typescript
let reducer: Reducer<number> = (state: number, action: Action) => {
    switch(action.type){
        case 'INCREMENT':
            return state  + 1;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;        
    }
};

let incrementAction: Action = {type: 'INCREMENT'};
console.log(reducer(0, incrementAction)); // -> 1
console.log(reducer(1, incrementAction)); // -> 2

let decrementAction: Action = {type: 'DECREMENT'};
console.log(reducer(100, decrementAction)); // -> 99

//any other action just returns the input state
let unknownAction: Action = {type: 'UNKNOWN'};
console.log(reducer(100, unknowAction)); // -> 100
```
Notice that the `default` case of the `switch` returns the original `state`. This ensures that if an unknown action is passed in, there's no error and we get the original `state` unchanged.

## Action "Arguments"
In the last example our actions contained only a `type` which told our reducer either to increment or decrement the state.

But often changes in our app can't be described by a single value - instead we need parameters to describe the changes. This is why we have the `payload` field in our `Action`.

In this counter example, say we wanted to add 9 to the counter. One way to do this would be to send 9 INCREMENT actions, but that wouldn't be every efficient especially, if we wanted to add, say 9000.

Instead. let's add a `PLUS` action that will use the `payload` parameter to send a number which specifies how much we want to add to the counter. Defining this action is easy enough:

```typescript
let plusSevenAction = { type: 'PLUS', payload: 7};
```
Next, to support this action, we add a new `case` to our reducer that will handle a 'PLUS' action:
```typescript
let reducer: Reducer<number> = (state: number, action: Action) => {
    switch(action.type){
        case 'INCREMENT':
            return state  + 1;
        case 'DECREMENT':
            return state - 1;
        case 'PLUS':
            return state + action.payload;    
        default:
            return state;        
    }
};
```
`PLUS` will add whatever number is in the `action.payload` to the `state`. We can try it out:

```javascript
console.log(reducer(3, {type: 'PLUS', payload: 7})); //-> 10
console.log(reducer(3, {type: 'PLUS', payload: 9000})); //-> 9003
console.log(reducer(3, {type: 'PLUS', payload: -2})); //-> 1
```
In the first line we take the state `3` and `PLUS` a payload of `7`, which result in 10. Neat! However, notice that while we're passing in a state, it doesn't really ever change. That is, we're not storing the result of our reducer's changes ans reusing it for future actions.

# Storing Our State
Our reducers are pure functions, and do not change the world around them. The problem is, in our app, thing do change. Specifically, our state changes and we need to keep the new state somewhere.

In Redux, we keep our state in the store. The store the responsibility of running the reducer and then keeping the new state. Let's take a look at a minimal store:
```typescript
class Store<T> {
    private _state: T;

    constructor(private reducer: Reducer<T>,
    initialState: T){
        this._state = initialState;
    }

    getState(): T{
        return this._state;
    }

    dispatch(action: Action): void {
        this._state = this.reducer(this._state, action);
    }
}
```
Notice that our store is generally typed - we specifiy the type of the state with generic type `T`. We store the state in the privare variable `_state`.

We also give our `Store` a `Reducer`, which is also type to operate on `T`,the state type this is 
because **each store is tied to a specific reducer**. We store the `Reducer` in the private variable `reducer`.
> In Redux, we generally have 1 store and 1 top-level reducer per application.

Let's take a closer look at each method of our `State`:
- In our constructor we set the `_state` to the initial state.
- `getState()` simply returns the current `_state`.
- `dispatch()` takes an action, send it to the reducer and then **updates the value `_state`** with the return value.

Notice that `dispatch` **doesn't return anything**. It's only updating the store state(once the result returns). This is an important principle of ReduxL dispatching action is a "fire-and-forget" maneuver. **Dispatching actions is not a direct manipulation of the state, and it doesn't return the new state.

When we dispatch actions, we're sending off a notification of what happened. If we want to know what the current state of the system is, we have to check the state of the store.

## Using the Store
Let's try using our store:
```typescript
let store = new Store<number>(reducer, 0);
console.log(store.getState()); // -> 0

store.dispatch({type: 'INCREMENT'});
console.log(store.getState()); // -> 1

store.dispatch({type: 'INCREMENT'});
console.log(store.getState()); // -> 2

store.dispatch({type: 'DECREMENT'});
console.log(store.getState()); // -> 1
```

We start by creating a new Store and we save this in store, which we can use to get the current state and dispatch actions. 

The state is set to `0` initially, and we `INCREMENT` twice and `DECREMENT` once and our final state is `1`.

## Beign Notified with subscribe
It's great that our `Store` keeps track of what changed, but in the above example we have to ask for the state changes with `store.getState()`. It would be nice for us to know immediately when a new action was dispatched so that we could respond. To do this we can implement the Observer pattern - that is we'll register a callback function that will subscribe to all changes.

Here's how we want it to work:
1. We will register a listener function using `subscribe`.
2. When `dispatch` is called, we will iterate over all listeners and call them, which is the notification that the state has changed.

### Registering Listeners
Our listener callbacks are a going to be a function that takes no arguments. Let's define an interface that makes it easy to describe this:
```typescript
interface ListenerCallback{
    (): void;
}
```
After we subscribe a listener, we might want to unsubscribe as well, so lets define the interface for an unsubscribe function as well:
```typescript
interface UnsubscribeCallback {
    (): void;
}
```
Not much going on here -it's another function that takes no arguments and has no return value. But by defining these types it makes our code clearer to read.
Our store is going to keep a list of `ListenerCallbacks` let's add that to our `Store`:
```typescript
class Store<T> {
    private _state: T;
    private _listeners: ListenerCallback[] = [];
}
```
Now we want to able to add to that list of `_listeners` with a `subscribe` function:
```typescript
subscribe(listener: ListenerCallback): UnsubscribeCallback{
    this._listeners.push(listener);
    return () => {
        this._listeners = this._listeners.filter( l=> l !== listener);
    };
}
```
`subscribe` accepts a `ListenerCallback` (i.e. a function with no arguments and no return value) and returns an `UnsubscribeCallback` (the same signature). Adding the new listener is easy: we `push` it on to the `_listener` array.

The return value is a function which will update the list of `_listeners` to be the list of `_listeners` without the `listener` we just added. That is, it return the `UnsubscribeCallback` that we can use to remove this listener from the list.

### Notifying Our Listeners
Whenever our state changes, we want to call these listener functions. What this means is, whenever we `dispatch` a new action, whenever thate state changes, we want to call all of this listeners:
```typescript
dispatch(action: Action): void {
    this._state = this.reducer(this._state, action);
    this._listeners.forEach((listener: ListenerCallback)=> listener());
}
```
### The Complete Store
We'll try this out below, but before we do that, here's the complete code listing for our new `Store`
```typescript
class Store<T> {
    private _state: T;
    private _listeners: ListenerCallback[] = [];

    constructor(
        private reducer: Reducer<T>,
        initialState: T
    ){
        this._state = initialState;
    }

    getState(): T{
        return this._state;
    }

    dispatch(action: Action): void {
        this._state = this.reducer(this._state, action);
        this._listeners.forEach((listener: ListenerCallback) => listener());
    }

    subscribe(listener: ListenerCallback): UnsubscribeCallback {
        this._listener.push(listener);
        return () => { //return an "unsubscribe" function
        this._listener = this.listeners.filter( l=> l !== listener);

        };
    }
}
```
### Trying Out subscribe
Now that we can subscribe to change in our store, let's try it out:

```typescript
let store = new Store<number>(reducer, 0);
console.log(store.getState());

//subscribe
let unsubscribe = store.subscribe(() =>{
    console.log('subscribed: ', store.getState());
});

store.dispatch({type: 'INCREMENT'}); // -> subscribed : 1
store.dispatch({type: 'INCREMENT'}); // -> subscribed: 2

unsubscribe();
store.dispatch({type: 'DECREMENT'}); // (nothing logged)

//decrement happened, even though we weren't listening for it.
console.log(store.getState()); // -> 1
```

Above we subscribe to our store and in the callback function we'll log `subscribed: ` and then the current store state.

We store the unsubscribe callback and then notice that after we call unsubscribe() our log message isn't called. We can still dispatch actions, we just won't see the results until we ask the store for them.

# A Messaging App
In our messaging app, as in all Redux apps, there are three main parts to the data model:
1. The state
2. The actions
3. The reducer

## Messaging App state
The state in our counter app was a single number. However in our messaging app, the state is going to be **an object**

This state object will have a single property, `messages`. `messages` will be an array of strings, with each string representing an individual message in the application. For example: 
```typescript
//an example 'state' value
{
    messages: [
        'here is mesaage one',
        'here is message two'
    ]
}
```
We can define the type for the app's state like this:
```typescript
interface AppState {
    messages: string[];
}
```
## Messaging App actions
Our app will process two actions: `ADD_MESSAGE` and `DELETE_MESSAGE`. 
The `ADD_MESSAGE` action object will always have the property `message`, the message to be added to the state. The `ADD_MESSAGE` action object has this shape:
```typescript
{
    type: 'ADD_MESSAGE',
    message: 'Whatever message we want here'
}
```
The `DELETE_MESSAGE` action object will delete a specified message from the state. A challenge here is that we have to be able to specify which message we want to delete.

If our message were objects, we could assign each message an id property when it is created. However, to simplify this example, our messages are just simple strings, so we'll have to get a handle to the message another way. The easiest way for now is to just use the index of the message in the array( as a proxy for the ID).

With that in mind, the `DELETE_MESSAGE` action object has his shape:
```typescript
{
    type: 'DELETE_MESSAGE',
    index: 2 // <- or whatever index is appropriate
}
```
We can define the types for these actions by using the `interface ... extends ` syntax in Typescript:
```typescript
interface AddMessageAction extends Action {
    message: string
}

interface DeleteMessageAction extends Action {
    index: number;
}
```
In this way our `AddMessageAction` is able to specify a `message` and the `DeleteMessageAction` will specify an `index`.

## Messaging App reducer
Remember that our reducer needs to handle two actions:
`ADD_MESSAGE` and `DELETE_MESSAGE`. Let's talk about these individually.

### Reducing ADD_MESSAGE
```typescript
let reducer: Reducer<AppState> = (state: AppState, action: Action): AppState => {
    switch(action.type){
        case 'ADD_MESSAGE':
            return {
                message: state.messages.concat(
                    <AddMessageAction>action.message
                ),
            };
    }
}
```
We start by switching on the `action.type` and handling the `ADD_MESSAGE` case.

### Adding an Item Without Mutation
When we handle an `ADD_MESSAGE` action, we need to add the given message to the state. As will all reducer handlers, we need to return a new state. Remember that our reducer must be pure and not mutate the old state.
What would be the problem with the following code ?
```typescript
case 'ADD_MESSAGE': 
    state.messages.push(action.message);
    return { messages: messages};
//...    
```
The problem is that this code mutates the `state.messages` array, which changes our old state! Instead what we want to do is create a copy of the `state.messages` array and add our new message to the copy.
```typescript
case 'ADD_MESSAGE':
return {
    messages: state.messages.concat(
        (<AddMessageAction>action).message
    ),
};
```
Remember that the reducer **must return a new** `AppState`. When we return an object from our reducer it must match the format of the `AppState` that was input. In this case we only have to keep the key `messages`, but in more complicated states we have more fields to worry about.

### Deleting an Item Without Mutation
Remember that when we handle the `DELETE_MESSAGE` action we are passing the index of the item in the array as the faux ID.(Another common way of handling the same idea would be to pass a real item ID). Again, becaise we do not want to mutate the old `messages` array, we need to handle this cases with care:
```typescript
    case 'DELETE_MESSAGE':
    let idx = (<DeleteMessageAction>action).index;
    return {
        messages: [
            ...state.messages.slice(0, idx),
            ...state.messages.slice(idx+1, state.messages.length)
        ]
    }
```
Here we use the `slice` operator twice. First we talk all of the items up until the item we are removing. And we concatenate the items that come after.

## Trying Out Our Actions
Now let's try running our actions:
```typescript
let store = new Store<AppState>(reducer, {messages: []});
console.log(store.getState());

store.dispatch({
    type: 'ADD_MESSAGE',
    message: 'Would you say the fringe was made of silk?'
} as AddMessageAction);

store.dispatch({
    type: 'ADD_MESSAGE',
    message: 'Wouldn\'t have no other kind but silk'
} as AddMessageAction);

store.dispatch({
    type: 'ADD_MESSAGE',
    message: 'Has it really got a team of snow white horses?'
} as AddMessageAction);

console.log(store.getState());
// -> 
// { messages:
//    [ 'Would you say the fringe was made of silk?',
//      'Wouldnt have no other kind but silk',
//      'Has it really got a team of snow white horses?' ] }
```
Here we start with a new store and we call `store.getState()` and see that we have an empty `messages` array.
Next we add three messages to our store. For each message we specify the `type` as `ADD_MESSAGE` and we cast each object to an `AddMessageAction`.
Finally we log the new state and we can see that `messages` contains all three messages.
Our three `dispatch` statements are a bit ugly for two reasons:
1. we manually have to specify the `type` string each time. We could use a constant, but it would be nice if we didn't have to do this and 
2. We're manually casting to an `AddMessageAction`.

Instead of creating these objects as an object directly we should create an function that will create these objects. This idea of writing a function to create actions is so common in Redux that the pattern has a name: Action Creators.

## Action Creators
Instead of creating the `ADD_MESSAGE` action directly as objects, let's create a function to do this for us:
```typescript
class MessageActions {
    static addMessage(message:string): AddMessageAction {
        return {
            type: 'ADD_MESSAGE',
            message: message
        };
    }

    static deleteMessage(index: number): DeleteMessageAction {
        return {
            type: 'DELETE_MESSAGE',
            index: index
        };
    }
}
```
Here we've created a class with two static methods `addMessage` and `deleteMessage`. They return an `AddMessageAction` and a `DeleteMessageAction` respectively.

Now let's use our new action creators:
```typescript

let store = new Store<AppState>(reducer, { messages: [] });
console.log(store.getState()); // -> { messages: [] }
 
store.dispatch(
  MessageActions.addMessage('Would you say the fringe was made of silk?'));
 
store.dispatch(
  MessageActions.addMessage('Wouldnt have no other kind but silk'));
 
store.dispatch(
  MessageActions.addMessage('Has it really got a team of snow white horses?'));
 
console.log(store.getState());
// -> 
// { messages:
//    [ 'Would you say the fringe was made of silk?',
//      'Wouldnt have no other kind but silk',
//      'Has it really got a team of snow white horses?' ] }
```
This feel much nicer!

An added benefit is that if we eventually decided to change the format of our message, we could do it without having to update all of our `dispatch` statements. For instance, say we wanted to add the time each message was created. We could add a `created_at` field to `addMessage` and now all `AddMessageActions` will be given a `created_at` field:
```typescript
class MessageActions {
  static addMessage(message: string): AddMessageAction {
    return {
      type: 'ADD_MESSAGE',
      message: message,
      // something like this
      created_at: new Date()
    };
  }
  // ....
```
## Using Real Redux
Now that we've built our own mini-redux you might be asking "What do I need to do to use the real Redux? "Thankfully, not very much. Let's update our code to use the real Redux now!

The first thing we need to do is import `Action`, `Reducer`, and `Store` from the `redux` package. We're also going to import a helper method `createStore` while we're at it:
```typescript
import {
  Action,
  Reducer,
  Store,
  createStore
} from 'redux';
```
Next, instead of specifying our inital state when we create the store instead we're going to let the reducer create the initial state. Here we'll do this as the default argument to the reducer. This way if there is no state passed in (e.g. the first time it is called initialization) we will use the initial state:
```typescript
let initialState: AppState = { messages: [] };
 
let reducer: Reducer<AppState> =
  (state: AppState = initialState, action: Action) => { }
 ```
 What's neat about this is that the rest of our reducer stays the same!
 The last thing we need to do is create the store using `createStore` helper method from Redux:
 ```typescript
 let store: Store<AppState> = createStore<AppState>(reducer);
 ```
 After that, everything else just works!
```typescript
let store: Store<AppState> = createStore<AppState>(reducer);
console.log(store.getState()); // -> { messages: [] }
 
store.dispatch(
  MessageActions.addMessage('Would you say the fringe was made of silk?'));
 
store.dispatch(
  MessageActions.addMessage('Wouldnt have no other kind but silk'));
 
store.dispatch(
  MessageActions.addMessage('Has it really got a team of snow white horses?'));
 
console.log(store.getState());
// -> 
// { messages:
//    [ 'Would you say the fringe was made of silk?',
//      'Wouldnt have no other kind but silk',
//      'Has it really got a team of snow white horses?' ] }
```
Now that we have a handle on using Redux in isolation, the next step is to hook it up to our web app. Let's do that now.

# Using Redux in Angular
In the last section we walked through the core of Redux and showed how to create reducers and use stores to manage our data in isolation. Now it's time to level-up and integrate Redux with our Angular components.

In this section we're going to create a minimal Angular app that contains just a counter which we can increment and decrement with a button
[!angular counter component](assets/counter-app.png)

By using such a small app we can focus on the integration points between redux and angular.

# Planning Our App
If you recall, the three steps to planning our Redux apps are to:
1. Define the structure of our central app state
2. Define actions that will change that state and
3. Define a reducer that takes the old state and an action and returns a new state.

For this app, we're just going to increment and decrement a counter. We did this in the last section, and so our actions, store, and reducer will all be very familiar.
The other thing we need to do when writing Angular apps is decide where we will create components. In this app, we'll have a top-level `AppComponent` which will have one component, the `AppComponent` which contains the view we see in the screenshot.

At a high level we're going to do the following:
1. Create our `Store` and make it accessible to our whole app via dependency injection.
2. Subscribe to changes to the `Store` and display them in our components.
3. When something changes (a button pressed) we will dispatch an action to the `Store`.

# Setting Up Redux

## Defining the Application State
Let's take a look at our `AppState`:
```typescript
export interface AppState{
    counter: number;
}
```
Here we are defining our core state structure as `AppState` - it is an object with one key, `counter` which is a `number`.

## Defining the Reducers
Next lets define the reducer which will handle incrementing and decrementing the counter in the application state:
```typescript
import {
  INCREMENT,
  DECREMENT
} from './counter.actions';
 
const initialState: AppState = { counter: 0 };
 
// Create our reducer that will handle changes to the state
export const counterReducer: Reducer<AppState> =
  (state: AppState = initialState, action: Action): AppState => {
    switch (action.type) {
    case INCREMENT:
      return Object.assign({}, state, { counter: state.counter + 1 });
    case DECREMENT:
      return Object.assign({}, state, { counter: state.counter - 1 });
    default:
      return state;
    }
  };
```
We start by importing the constants `INCREMENT` and `DECREMENT`, which are exported by our action creators. They're just defined as the strings `INCREMENT` and `DECREMENT`, but it's nice to get extra help from the compiler in case we make a typo. We'll look at those action creators in a minute.
The `initialState` is an `AppState` which sets the counter to `0`.
The `counterReducer` handles two actions: `INCREMENT`, which adds `1` to the current counter and `DECREMENT`, which subtracts 1. Both action use `Object.assign` to ensure that we don't mutate the old state, but instead create a new object that gets returned as the new state.

## Definig Action Creators
Our action creators are functions which return objects that define the action to be taken. `increment` and `decrement` below return an object that defines the appropriate `type`.
```typescript
import {
  Action,
  ActionCreator
} from 'redux';
 
export const INCREMENT: string = 'INCREMENT';
export const increment: ActionCreator<Action> = () => ({
  type: INCREMENT
});
 
export const DECREMENT: string = 'DECREMENT';
export const decrement: ActionCreator<Action> = () => ({
  type: DECREMENT
});
```
Notice that our action creator functions return the type `ActionCreator<Action>`. `ActionCreator` is a generic class defined by Redux that we use to define functions that create actions. In this case we're using the concreate class Action, but we could use a more specific `Action` class such as `AddMessageAction`.

## Creating the Store
Now that we have our reducer and state, we could create our store like so:
```typescript
let store: Store<AppState> = createStore<AppState>(counterReducer);
```
## Providing the Store
Now that we have the Redux core setup, let's turn our attention to our Angular components. Let's create our top-level app component, `AppComponent`. This will be the component we use to `bootstrap` Angular:
We're going to use the `AppComponent` as the root component. Remember that since this is a Redux app, we needd to make our store instance accesible everyehere in our app. How should we do this? We'll use dependency injection(DI).
When we want to make something available via DI then we use the `providers` configuration to add it to the list of providers in our `NgModule`.

When we provide something to the DI system, we specify two things:
1. the token to use to refer this injectable dependency.
2. the way to inject the dependency.

Oftentimes if we want to provide a singleton service we might use the `useClass` option as in:

In the case above, we're using the class `SpotifyService` as the token in DI system. The `useClass` option tells Anguler to crete an instance of `SpotifyService` and reuse that instance whenever the `SpotifyService` injection is requsted(e.g. maintain a Singleton).
One problem with us using this methos is that we don't want Angular to create our store - we did it ourseleves above with `createStore`. We just want ot use the `store` we've already created.
To do this we'll use the `useValue` option of `provide`. We've done this before with configurable values like `API_URL`:
The one thing we have left to figure out is what token we want to use to inject. Our `store` is of type `Store<AppState>`:
```typescript
export function createAppStore(): Store<AppState> {
  return createStore<AppState>(
    reducer,
    compose(devtools)
  );
}
 
export const appStoreProviders = [
   { provide: AppStore, useFactory: createAppStore }
];
```
`Store` is an interface, not a class and unfortunately, we can't use interfaces as a dependency injection key.
This means we need to create our own token that we'll use for injecting the store. Thankfully, Anguler makes this easy to do. Let's create this token in it's own file so that way we can `import` it from anywhere in our application.
```typescript
export const AppStore = new InjectionToken('App.store');
```
Here we have created a `const AppStore` which use the `OpaqueToken` class from Angular. `OpaqueToken` is a better choice than injecting a string directly because it help us avoid collisions. 

Now we can use this token `AppStore` with `provide`.

## Bootstrapping the App
Back in `app.module.ts`. let's create the `NgModule` we'll use to bootstrap our app:
```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';
 
import { appStoreProviders } from './app.store';
 
import { AppComponent } from './app.component';
 
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [ appStoreProviders ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
Now we are able to get a reference to our Redux store anywher in our app by injecting `AppStore`. The place we need it most now is our `AppComponent`.
> Notice that we exported the function `appStoreProviders` from `app.store.ts` and then used that function on providers. Why not use the `{provider:..., useFactory:...}` syntax directly? The answer is related to AOT-if we want to ahead-of-time compile a provider that uses a function, we must first export is as a function from another module.


