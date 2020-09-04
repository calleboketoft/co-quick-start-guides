## React Redux Thunk

index.tsx

```typescript
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

redux/store.ts

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

redux/actionTypes.ts

```typescript
export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';
export const SET_NUMBER = 'SET_NUMBER';
```

redux/actions.ts

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

redux/reducers/index.ts

```typescript
import { combineReducers } from 'redux';
import { counter } from './counter';

export default combineReducers({ counter });
```

redux/reducers/counter.reducer.ts

```typescript
import { INCREMENT, DECREMENT, SET_NUMBER } from '../actionTypes';

const initialState = 0;

type Action = {
  type: String;
  payload: Number;
}

export function counter(state = initialState, action: Action) {
  switch (action.type) {
    case actionTypes.INCREMENT:
      return state + 1;
    case actionTypes.DECREMENT:
      return state - 1;
    case actionTypes.SET_NUMBER:
      return action.payload;
    default:
      return state;
  }
}
```

redux/reducers/counter.selectors.ts

```typescript
export const selectCounterTimesTen = (state) => {
  return state.counter * 10;
};
```

components/AddButton/index.ts

```typescript
import { AddButtonComponent } from './AddButton.component';
import AddButtonConnector from './AddButton.props';

export default AddButtonConnector(AddButtonComponent);
```

components/AddButton/AddButton.props.ts

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
  setNumber: setNumber,
  increment: increment,
}

export default connect(mapStateToProps, mapDispatchToProps);
```

components/AddButton/AddButton.component.ts

```typescript
import React, { FunctionComponent } from 'react';

export const SaveButtonComponent: FunctionComponent = (
  props: any
): JSX.Element => {
  const handleClick = (e: any) => {
    e.preventDefault();
    props.increment();
    console.log(props.counterTimesTen);
  };
  return (
    <button type="submit" onClick={(e) => handleClick(e)}>
      Save
    </button>
  );
};
```