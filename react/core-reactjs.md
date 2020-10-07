# Introduction
React created by Facebook, is a open-soruce Javascript library for building user interfaces. It is used to create components, handle state and props, utilize event listeners and certain life cycle method to update data as it changes.

React combines HTML with Javascript functionality to create it own markup language JSX.

# JSX Element
React uses a syntax extension of Javascript called JSX that allows you to write HTML directly within Javascript. This has several benefits. It lets you use the full programmatic power of Javascript withing HTML, and helps to keep your code readable. For the most part, JSX is similar to the HTML that you have already learned however there are a few key differences that will be covered throughout these challenges.

For instance, because JSX is a syntactic extension of Javascript, you can actually write Javascript directly within JSX. To do this, you simply include the code you want to be treated as Javascript within curly braces : `{'this is treated as Javascript code'}`.

However, because JSX is not valid Javascript, JSX code must be compiled into Javascript. THe transpiler Babel is a popular tool for this process. For your convenience, it's already added behind the scenes for these challenges. If you happen to write syntactically invalid JSX, you will see the first test in these challenges fail.

One important thing to know about nested JSX is that it must return a single element.

This is one parent element would wrap all of the other levels of nested elements.

For instance, several JSX elements written as sibilings with no parent wrapper element will not transpile.

Here 's example:
**Valid JSX**
```html
<div>
  <p>Paragraph One</p>
  <p>Paragraph Two</p>
  <p>Paragraph Three</p>
</div>
```
**Invalid JSX**
```html
<p>Paragraph One</p>
<p>Paragraph Two</p>
<p>Paragraph Three</p>
```

JSX is a syntax gets compiled into valid Javascript. Sometimes, for readability, you might need to add comments to your code. Like most programming languages  JSX has its own way to do this.

To put comments inside JSX you use the syntax `{/* */}` to wrap around the comment text.

With React, we can render this JSX directly to the HTML DOM using React's rendering API known as ReactDOM.

ReactDOM offers a simple method to render React element to the DOM which looks like this: `ReactDOM.render(componentToRender, targetNode)`, where the first argument is the React element or component that you want to render, and the second argument is the DOM node that you want to render the component to.

As you would expect `ReactDOM.render()` must be called after the JSX element declarations, just like how you must declare variables before using them.

One key difference in JSX is that you can no longer use the word `class` to define HTML classes. This is because `class` is reversed word in Javascript. Instead JSX uses `className`.

In fact, the naming convention for all HTML attributes and event references in JSX become camelCase. For example, a click event in JSX is `onClick`, instead of `onclick`. Likewise, `onchange` become `onChange`. While this is subtle difference, it is an important one to keep in mind moving forward.

Another important way in which JSX differs from HTML is in the idea of the self-closing tag.

# Stateless Functional Component
Components are the core of React. Everything in React is a component and here you will learn how to create one.

 There are two ways to create a React component. The first way is to use a Javscript function. Defining a component in this way creates a stateless functional component. A stateless component as one that can receive data and render it, but does not manage or track changes to the data. 

 To create a component with a function, simply write a Javascript function that returns either JSX or null. One important thing to note is that Rect requires your function name to begin with a capital letter. Here's an example of a stateless functional component that assigns an HTML class in JSX.

 Because a JSX component represents HTML, you could put several components together to create a more complex HTML page. This is one of the key advantages of the component architecture React provides. It allows you to compose your UI for many separate, isolated components. This makes it easier to buold and maintain complex user interfaces.

 # Create a React Component
 The other way to define a React component is with the ES6 `class` syntax. In the following example, `Kitten` extends `React.Component`.

 ```javascript
 class Kitten extends React.Compnent {
     constructor(props) {
         super(props);
     }

     render() {
         return (<h1>Hi!</h1>);
     }
 }
 ```

 Imagine you are building an App and have created three components a `Navbar`, `Dashboard` and `Footer`. To compose these components together, you could create an `App` parent component which renders each of these three components as children. To render a component as a child in a React component, you include the component name written as custom HTML tag in the JSX. For example, in the `render` method you could write:'

```javascript
return (
 <App>
  <Navbar />
  <Dashboard />
  <Footer />
 </App>
)
```

When React encounters a custom HTML tag that references another component (a component name wrapped in `</>`), it renders the markup for that component in the location of the tag. This should illustrate the parent/child relationship between the `App` component and the `Navbar`, `Dashboard` and `Footer`.

## Use React to Render Nested Component
Component composition is one of React's powerful features. When you work with React, it is important to start thinking about your user interface in terms of components like the App. You break down your UI into basic building blocks, and pieces become the components. This helps to separate the code responsible for the UI from the code responsible for handling your application logic. It can greatly simplify the development and maintain of complex projects.

## Pass props to a stateless functional component
In React, you can pass props, or properties to child components. Say you have an `App` component which renders a child component called `Welcome` which is a stateless functional component. You can pass `Welcome` a `user` property by writing:

```javascript
<App>
  <Welcome user='Mark' />
</App>
```

You use **custom HTML attributes** created by you and supported by React to be passed to the component. In this case, the created property `user` is passed to the component `Welcome`. Since `Welcome` is a stateless functional component, it has access to this value like so:

```javascript
const Welcome = (props) => <h1>Hello, {props.user}!</h1>
```

It is standard to call this value `props` and when dealing with stateless functional components, you basically consider it as an argument to a function which returns JSX. You can access the values of the argument in the function body. 

React also has an option to set default props. You can assign default props to component as a property on the component itself and React assign the default prop if necessary. This allows you to specify what prop value should be if no value is explicitly provided. For example, if you declare `MyComponent.defaultProps = {location: 'San Francisco'}`, you have defined a location prop that's set to the string `San Francisco`, unless you specify otherwiese. React assigns default props if props are undefined, but if you pass `null` as the values for a prop, it will remain `null`.

The ability to set default props is a useful feature in React. The way to override the default is to explicitly set the props values for a component.

## Use PropTypes to Define the Props you Expect
React provides useful type-checking features to verify that components receive props of the correct type. For example, you application makes an API call to retrieve data that you expect to be in an array, which is then passed to a component as a prop. You can set `propTypes` on your component to require the data to be of type `array`. This will throw a useful warning when the data is of any other type.

It's considered a best pratice to set `propTypes` when you know the type of a prop ahead of time. You can define a `propTypes` property for a component in the same way you defined `defaultProps`. Doing this will check the props of a given key are present with a given type. Here's an example to require the type `function` for a prop called `handleClick`:

```javascript
MyComponent.propTypes = { handleClick: PropTypes.func.isRequired }
```

In the example above, the `PropTypes.func` part checks that `handleClick` is a function. Adding `isRequired` tells React that `handleClick` is a required property for that component. You will see a warning if that prop isn't provided. Also notice that `func` represent `function`. Among the seven Javascript primitive types, `function` and `boolean`(written as `bool`) are the only two that use unusal spelling. In addition to the primitive types, there are other types available. For example, you can check that a prop is a React element.

## Accesss Props Using this.props
Anytime you refer to a class component within itself, you use the `this` keyword. To access props within a class component, you preface the code that use to access it with `this`. For example, if an ES6 class component has a prop called `data`, you write `{this.props.data}` in JSX.

# Create a Stateful Component
One of the most importatn topics in React is `state`. State consists of any data your application needs to know about that can change over time. You want your apps to respond to state changes and present an updated UI when necessary. React offers a nice solution for the state management of modern web applications.

You create state in a React component by declaring a `state` property on the component class in its `constructor`. This initializes the component with `state` when it created. The `state` property must be set to a Javascript `object`. Declaring it look like this:

```javascript
this.state = {
  // describe your state here
}
```

You have access to the `state` object throughout the life of your component. You can update it, render it in your UI, and pass it as props to child components. The `state` object can be as complex or as simple as you need it to be. Note that you must create a class component by extending `React.Component` in order to create `state` like this.

Once you define a component's initial state, you can display any part of it in the UI that is rendered. If a component is stateful, it will always have access to the data in `state` in its `render()` method. You can access the data with `this.state`.

If you want to access a state value within the `return` of the render method, you have to enclose the value in curly braces.

`State` is one of the most powerful features of components in React. It allows you to track important data in your app and render a UI in response to changes in this data. If your data changes your UI will change. React uses what is called a virtual DOM, to keep track of changes behind the scenes. When state data updates, it trigger a re-render of the component using that data - including child componenets that received the data as a prop. React updates the actual DOM, but only where necessary. This means you don't have to worry about changing the DOM. You simply declare what the UI should look like.

Note that if you make a component stateful, no other components are aware of its `state`. Its `state` is completely encapsulated, or local to that component unless you pass state data to a child component as `props`. This notion of encapsulated `state` is very important because it allow you write certain logic, then have that logic contained and isolated in one place in your code.

There is another way to access `state` in a component. In the `render()` method, before the `return` statement, you can write Javascript directly. For example, you could declare functions, access data from `state` or `props`, perform computations on this data, and so on. Then, you can assign any data to variables, which you have access to in the `return` statement.

There  is also a way to change the component's `state`. React provides a method for updating component `state` called `setState`. You call the `setState` method within your component class like so: `this.setSetate()`, passing in an object with key-values pairs. The keys are your state properties and the values are the updated state data. For instance, if we were storing a `username` in state and wanted to update it, it would look like this:

```javascript
this.setState({
  username: 'Lewis'
});
```

React excpects you to never modify `state` directly, instead always use `this.setState()` when state changes occur. Also, you should note that React may batch multiple state updates in order to improve performance. What this means is that state update thrpough the `setState` method can be asynchronous. There is an alternative syntax for the  `setState` method provides a way around this problem.

# Bind 'this' to a Class Method
In addition to setting and updating `state` you can also define methods for component class. A class method typically needs to use the `this` keyword so it can access properties on the class(such as state and props) inside the scope of the method. There are a few ways to allow your class methods to access `this`.

One common way is to explicitly bind `this` in the constructor so `this` become bound to the class methods when the component is initialized. `this.handleClick = this.handleClick.bind(this)` for its `handleClick` method in the constructor. Then you call a function like `this.setState()` within your class method, `this` refers to the class and will not be `undefined`.

> The `this` keyword is one of the most confusing aspects of Javascript but it plays an important role in React. 

# Controlled Input
Your application may have more complex interactions between `state` and the rendered UI. For example, form control elements for text input. such as `input` and `textarea`, maintain their own state in the DOM as the user types. With React, you can move this mutable state into a React Component's `state`. The user's input becomes part of the application `state`, so React controls the value of that input field. Typically, if you have React components with input fields the user can type into, it will be a controlled input form.

## Pass State as Props to Child Component
A common pattern is to have a stateful component containing the `state` important to your app, that then renders child components. Yo want these components to jave access to some pieces of that `state`, which are passed in as props.

For example, maybe you have `App` component that renders a `Navbar`, among other components. In your `App`, you have `state` that contains a lot of user information, but the `Navbar` only needs access to the user's username so it can display it. YOu pass the piece of state to the `Navbar` component as a prop.

This pattern illustrates some important paradigms in React. The first is _unidirectional data flow_. State flows in one directional down the tree of your application's components, from the stateful parent component to child components. The child components only receive the state data they need. The second it that complex stateful component. The rest of your components simply receive state from the parent as props, and render a UI from that state. It begans to create a separation where state management is handled in one part of code and UI rendering in another. This principle of separating state logic from UI logic is one of React's key principles. When it's used correctly, it makes the design of complex, stateful applications much easier to manage.

## Pass a Callback as props
You can pass `state` as props to child component, but you're not limited to passing data. You can also pass handler functions or any method that's defined on a React component to a child component. This is how you allow child components to interact with their parent components. You pass methods to a child just like a regular prop. It's assigned a name and you have access to that method name under `this.props` in the child component.

# Use the Lifecycle method componentWillMount
React components have several special methods that provide opportunities to perform actions at specific points in the lifecycle of a component. These are called lifecycle methods ot lifecycle hooks, and allow you to catch components at certain points in time. This can be before they are rendered, before they update, before they receive props, before they unmount and so on. Here is a list some of the main lifecycle methods: `componentWillMount()`, `componentDidMount()`, `shouldComponentUpdate()`, `componentDidUpate()`, `componentWillUnmount()`.

# Use the Lifecycle method componentDidMount
Most web developers, at some point, need to call an API endpoint to retrieve data. If you're working with React, it's important to know where to perform this action.

The best practice with React is to place API calls or calls to your server in the lifecycle method `componentDidMount()`. This method is called after a component is mounted to the DOM. Any calls to `setState()` here will trigger a re-rendering of your component. When you call an API in this method, and set your state with the data that the API returns, it will automatically trigger an update once you receive the data.

# Add Event Listeners
The `componentDidMount()` method is also the best place to attach any event listeners you need to add for specific functionality. React provides a synthetic event system which wraps the native event system present in browsers. This means that the synthetic event system behaves exactly the same regardless of the user's browser even if the native events may behave differently between different browsers.

You've already isong some these synthetic event handlers such as `onClick()`. React's syntehtic event system is great to use from most interactions you'll manage on DOM elements. However, if you want to attach an event handler to the document or window objects, you have to do this directly.

# Optimize Re-renders with shouldCompleteUpdate
So far, if any component receives new `state` or new `props`, it re-render itself and all its children. This is usually okay. But React provides a lifecycle method you can call when child components receive new `state` or `props`, and declare specifically if the components should update or not. The method is `shouldComponetUpdate()` and it takes `nextProps` and `nextState` as parameters.

This method is a useful way to optimize performance. For example, the default behavior is that your component re-renders when it receives new `props`, even if the `props` haven't changed. You cn use `shouldComponentUpdate()` to prevent this comparing the `props`. The method must return a `boolean` value that tells React whether or not to update the component. You can compare the current props (`this.props`) to the next props (`nextProps`) to determine if you need to updated or not, and return `true` or `false` accordingly.

# Render with an If-Else condition
Another application of using Javascript to control your rendered view is to tie the element that are rendered to a condition. When the condition is true, one view renders. When it's false, it is different view. Yo can do this with a standard `if/else` statement in the `render()` method of a React component.
  
