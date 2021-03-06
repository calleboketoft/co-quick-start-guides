# React Redux

Complete app with React, Redux, and Thunks. Organized bottom up.

## Set up project

>npx create-react-app myapp --template typescript

>cd myapp

>npm install --save redux react-redux redux-thunk redux-devtools-extension @types/react-redux

## redux/actionTypes.ts

- Constants for `action.type`

```typescript
export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';
export const SET_NUMBER = 'SET_NUMBER';
```

## redux/actions.ts

- Import `actionTypes.ts`
- Action creators for `action.type` and `action.payload` to be dispatched to store

```typescript
import { INCREMENT, DECREMENT, SET_NUMBER } from './actionTypes';

export const increment = () => ({
  type: INCREMENT,
});

export const decrement = () => ({
  type: DECREMENT,
});

export const setNumber = (number: Number) => ({
  type: SET_NUMBER,
  payload: number,
});
```

## redux/reducers/counter.reducer.ts

- Imports `actionTypes.ts` for type names
- Updates the content of the store for this reducer

```typescript
import { INCREMENT, DECREMENT, SET_NUMBER } from '../actionTypes';

const initialState = 0;

type Action = {
  type: String;
  payload: Number;
}

export function counter(state = initialState, action: Action) {
  switch (action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    case SET_NUMBER:
      return action.payload;
    default:
      return state;
  }
}
```

## redux/reducers/index.ts

- Gathers all reducers
- Combines reducers for the store

```typescript
import { combineReducers } from 'redux';
import { counter } from './counter.reducer';

export default combineReducers({ counter });
```

## redux/store.ts

- Create store
- Import reducers and add them to store
- Activate middlewares and dev tools

```typescript
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from 'redux-devtools-extension';
import thunk from 'redux-thunk';
import rootReducer from './reducers';

export const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(thunk))
);
```

## index.tsx

- Import store
- Connect store to React (react-redux)

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

import { Provider } from 'react-redux';
import { store } from './redux/store';

import './index.css';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

## redux/reducers/counter.selectors.ts

- Get specific data from the store, with or without modification

```typescript
export const selectCounterTimesTen = (state: any) => {
  return state.counter * 10;
};
```

## components/MyButton/MyButton.props.ts

- `mapStateToProps`
  - Called every time state changes. Receives the whole state and returns object with data that the component needs.
- `mapDispatchToProps`
  - Object with the actions that the component needs.

```typescript
import { connect } from 'react-redux';
import { setNumber, increment } from '../../redux/actions';
import { selectCounterTimesTen } from '../../redux/reducers/counter.selectors';

const mapStateToProps = (state: any /*, ownProps*/) => {
  return {
    counter: state.counter,
    counterTimesTen: selectCounterTimesTen(state),
  };
};

const mapDispatchToProps = {
  setNumber,
  increment,
}

export default connect(mapStateToProps, mapDispatchToProps);
```

## components/MyButton/MyButton.component.ts

- The React component itself

```typescript
import React, { FunctionComponent } from 'react';

export const MyButtonComponent: FunctionComponent = (
  props: any
): JSX.Element => {
  const handleClick = (e: any) => {
    e.preventDefault();
    props.increment();
    console.log(props.counterTimesTen);
  };
  return (
    <button type="submit" onClick={(e) => handleClick(e)}>
      Click me
    </button>
  );
};
```

## components/MyButton/index.ts

- Connects `MyButton.props.ts` with `MyButton.component.tsx` for `react-redux`

```typescript
import { MyButtonComponent } from './MyButton.component';
import MyButtonConnector from './MyButton.props';

export default MyButtonConnector(MyButtonComponent);
```

## App.tsx

- Render `MyButton` in the main `App`

```typescript
import React from 'react';
import MyButton from './components/MyButton';

function App() {
  return <MyButton></MyButton>;
}

export default App;
```

## Ready to run

>npm start

Open browser and check out the button. Click it and increase the counter.

# Redux Thunk

## redux/thunks.ts

- Return a function instead of an object from an action
- Allows for async execution of actions
- Used exactly like an action, import in the [name].props.ts file

```typescript
import { setNumber } from './actions';

export const setNumberDelayed = (newNumber: Number) => (dispatch: any) => {
  setTimeout(() => {
    dispatch(setNumber(newNumber));
  }, 1000);
};
```

## components/MyButton/MyButton.props.tsx

- Add `setNumberDelayed` to `mapDispatchToProps` object

```typescript
import { setNumberDelayed } from '../../redux/thunks';

mapDispatchToProps: {
  setNumberDelayed: setNumberDelayed,
}
```

## components/MyButton/MyButton.component.tsx

- Call `setNumberDelayed` from `handleClick`

```typescript
handleClick (e) => {
  props.setNumberDelayed(10);
}
```

# Redux Saga

# Redux Observable