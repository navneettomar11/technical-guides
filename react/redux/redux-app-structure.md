# The Counter Example App
The sample project we'll look at is a small counter application that lets us add or subtract from a number as we click buttons. It may not be very exciting, but it shows all the important pieces of a React+Redux application in action.

Let's start by looking at how the Redux store is created.

## Creating the Redux store
Open up `app/store.js`, which should look like this:

```javascript
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export default configureStore({
  reducer: {
    counter: counterReducer
  }
})
```

The Redux store is created using the `configureStore` function from Redux Toolkit. `configureStore` requires that we pass in a reducer argument.
