# Redux Thunk

## Component Hierarchy 
App
  |-- Navbar 
  |-- About
  |-- PaintingsContainer
        |-- PaintingForm
        |-- PaintingDetail
        |-- Searchbar
        |-- PaintingsList
              |-- PaintingListItem ...

## Overview

- Problem: How do we do async actions in redux?
- What is a thunk
- Redux thunk plumbing (setting it up)
- Using Thunk

## Async actions in redux

In vanilla React, we'd do our fetch in componentDidMount. Now that we're using redux, we dispatch actions instead of setting the state:

```js
componentDidMount() {
  fetch(URL)
    .then(res => res.json())
    .then(paintings => this.props.fetchedPaintings(paintings)))
}
```

We want to move our logic out of our components and into redux.

Why? - Components shouldn't know about state management logic. If we delete this component, we might still want to load the paintings. One goal of redux is to decouple our components from our state.

## Solution: Redux Thunk

- Thunk lets us write more complex action creators
- Instead of returning action objects, action creators can return _thunks_

**Thunk**: a function we can dispatch

Thunks get in the dispatch function as an argument, so they can dispatch further actions.

## Thunk Plumbing

`npm install redux-thunk`

```
import { createStore, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
createStore(reducer, composeEnhancers(applyMiddleware(thunk)))
```

Now we can use thunks!

## Using Thunks

Now our actionCreators can return thunk actions - functions that will get called with `dispatch`.

```js
// plain old actionCreator
const fetchedPaintings = (paintings) => {
  return { type: "FETCHED_PAINTINGS", paintings }
}

// Thunk actionCreator
const fetchPaintings = () => {
  return (dispatch) => {
    fetch(URL)
      .then(res => res.json())
      .then(paintings => dispatch(fetchedPaintings(paintings)))
  }
}
```

Now, in our component, we can just call the prop function:

```js
componentDidMount() {
  this.props.fetchPaintings();
}
```

### Let's refactor our Save Painting feature to persists (by using Thunk)

#### Bonus: How would you refactor the Vote feature to use Thunk?
