# 🛒 Redux Cart Example — Vanilla Redux vs Redux Toolkit

This demonstrates how to implement a simple cart feature using both **Vanilla Redux** and **Redux Toolkit**. This is intended to serve as a detailed reference guide so that you (or future you) can understand and revise everything clearly — including the flow, file responsibilities, and which terms are fixed vs customizable.

---

## 📁 Folder Structure

```
📦src
┣ 📂vanillaRedux
┃ ┣ 📜cartActions.js
┃ ┣ 📜cartReducer.js
┃ ┗ 📜store.js
┣ 📂reduxToolkit
┃ ┣ 📜cartSlice.js
┃ ┗ 📜store.js
┣ 📜App.js
┣ 📜index.js
┗ 📜CartComponent.jsx
```

---

## 🔁 Flow Comparison

### Vanilla Redux Flow

```
UI (button click)
  -> mapDispatchToProps
    -> Action Creator (cartActions.js)
      -> Dispatches Action Object
        -> cartReducer.js (switch-case based on type)
          -> New State Returned
            -> Redux Store Updated
              -> mapStateToProps
                -> UI Re-rendered with new state
```

### Redux Toolkit Flow

```
UI (button click)
  -> useDispatch()
    -> Action Creator (auto-generated from createSlice)
      -> Reducer in cartSlice triggered
        -> Mutates or returns new state
          -> Redux Store Updated
            -> useSelector reads new state
              -> UI Re-rendered with new state
```

---

## ⚙️ VANILLA REDUX IMPLEMENTATION

### 1. `cartActions.js`

Defines action types and action creators.

```js
export const ADD_TO_CART = 'ADD_TO_CART';
export const REMOVE_FROM_CART = 'REMOVE_FROM_CART';
export const CLEAR_CART = 'CLEAR_CART';

export const addToCart = (item) => ({
  type: ADD_TO_CART,
  payload: item,
});

export const removeFromCart = (id) => ({
  type: REMOVE_FROM_CART,
  payload: id,
});

export const clearCart = () => ({
  type: CLEAR_CART,
});
```

### 2. `cartReducer.js`

Handles state transitions using `switch-case`.

```js
import { ADD_TO_CART, REMOVE_FROM_CART, CLEAR_CART } from './cartActions';

const initialState = [];

const cartReducer = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TO_CART:
      return [...state, action.payload];
    case REMOVE_FROM_CART:
      return state.filter(item => item.id !== action.payload);
    case CLEAR_CART:
      return [];
    default:
      return state;
  }
};

export default cartReducer;
```

### 3. `store.js`

Sets up the Redux store.

```js
import { createStore } from 'redux';
import cartReducer from './cartReducer';

const store = createStore(cartReducer);

export default store;
```

### 4. `CartComponent.jsx`

React component connected via `connect()`.

```jsx
import React from 'react';
import { connect } from 'react-redux';
import { addToCart, removeFromCart, clearCart } from './vanillaRedux/cartActions';

const CartComponent = ({ cart, addToCart, removeFromCart, clearCart }) => {
  return (
    <div>
      <button onClick={() => addToCart({ id: 1, name: 'Item A' })}>Add</button>
      <button onClick={() => removeFromCart(1)}>Remove</button>
      <button onClick={clearCart}>Clear</button>

      <ul>
        {cart.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
};

const mapStateToProps = (state) => ({ cart: state });
const mapDispatchToProps = { addToCart, removeFromCart, clearCart };

export default connect(mapStateToProps, mapDispatchToProps)(CartComponent);
```

---

## 🧠 Fixed vs Customizable Terms in Vanilla Redux

| Term          | Fixed? | Notes                                 |
| ------------- | ------ | ------------------------------------- |
| `type`        | ✅ Yes  | Required in all actions               |
| `payload`     | ❌ No   | Just a key convention, can be renamed |
| `state`       | ❌ No   | Reducer param, can be renamed         |
| `action`      | ❌ No   | Reducer param, can be renamed         |
| `createStore` | ✅ Yes  | Redux core API                        |

---

## ⚡ REDUX TOOLKIT IMPLEMENTATION

### 1. `cartSlice.js`

Defines state, reducers, and auto-generates actions.

```js
import { createSlice } from '@reduxjs/toolkit';

const cartSlice = createSlice({
  name: 'cart',
  initialState: [],
  reducers: {
    add: (state, action) => { state.push(action.payload); },
    remove: (state, action) => state.filter(item => item.id !== action.payload),
    removeAll: (state) => { state.length = 0; },
  },
});

export const { add, remove, removeAll } = cartSlice.actions;
export default cartSlice.reducer;
```

### 2. `store.js`

```js
import { configureStore } from '@reduxjs/toolkit';
import cartReducer from './cartSlice';

const store = configureStore({
  reducer: {
    cart: cartReducer,
  },
});

export default store;
```

### 3. `CartComponent.jsx` (using Hooks)

```jsx
import React from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { add, remove, removeAll } from './reduxToolkit/cartSlice';

const CartComponent = () => {
  const cart = useSelector(state => state.cart);
  const dispatch = useDispatch();

  return (
    <div>
      <button onClick={() => dispatch(add({ id: 1, name: 'Item A' }))}>Add</button>
      <button onClick={() => dispatch(remove(1))}>Remove</button>
      <button onClick={() => dispatch(removeAll())}>Clear</button>

      <ul>
        {cart.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default CartComponent;
```

---

## 🧠 Fixed vs Customizable Terms in Redux Toolkit

| Term               | Fixed? | Notes                                               |
| ------------------ | ------ | --------------------------------------------------- |
| `createSlice`      | ✅ Yes  | Core function                                       |
| `name`             | ✅ Yes  | Used to prefix action types                         |
| `initialState`     | ✅ Yes  | Must define initial state                           |
| `reducers`         | ✅ Yes  | Must define as object of reducer functions          |
| `state` & `action` | ❌ No   | Reducer params — rename if needed (not recommended) |
| `action.payload`   | ✅ Yes  | Payload is fixed unless you customize action shape  |
| `useDispatch`      | ✅ Yes  | React-Redux hook for dispatching actions            |
| `useSelector`      | ✅ Yes  | React-Redux hook to read from store                 |

---

## ✅ Summary: When to Use What

| Feature             | Vanilla Redux               | Redux Toolkit                   |
| ------------------- | --------------------------- | ------------------------------- |
| Boilerplate         | 🔺 High                     | ✅ Low                           |
| Learning Curve      | ✅ Teaches fundamentals      | Easier and scalable             |
| Action Creators     | Manual                      | Auto-generated                  |
| Reducers            | Manual switch-case          | Object style, with Immer        |
| Store Configuration | Manual with combineReducers | Simplified via `configureStore` |

---

## 📌 Conclusion

This project gives you both a foundational understanding of traditional Redux and the modern approach using Redux Toolkit. Whether you're revisiting after a few weeks or months, this README will help you quickly revise:

- The flow of actions and state
- How Redux integrates into React
- What’s fixed vs flexible in both patterns

Happy coding!

---

Let me know if you want the examples extended to async operations using Redux Thunk or RTK Query!

