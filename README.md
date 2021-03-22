# Configure-Redux-for-Next-App

You need these 3 packages for redux to be added to the next app.

`$. yarn add next-redux-wrapper redux react-redux redux-thunk redux-devtools-extension`


Then add your folder structure for `redux`


    .
    ├── ...
    ├── store        
    │   ├── reducers        
    │   ├── actions      
    │   └── types.js
        └── store.js
    └── ...

**types.js**

```js
export const ADD_TODO = 'ADD_TODO'
```

**store.js**

```js
import { createStore, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'
import { composeWithDevTools } from 'redux-devtools-extension'

import rootReducer from './reducers'

const initialState = {}
const middleware = [thunk]

const store = createStore(rootReducer, initialState, composeWithDevTools(applyMiddleware(...middleware)));

export default store;
```

**reducers/index.js**

```js
import { combineReducers } from 'redux';
import { todosReducer } from './TodosReducer'
export default combineReducers({
    todos: todosReducer
})
```

**reducers/TodosReducer.js**

```js
import * as types from '../types';

const initialState = {  
    todos: [],
    todo: {},
    loading: false,
    error: null
}

export const todosReducer = (state = initialState, action) => {
    switch(action.type) {
        case types.GET_TODOS: 
            return {
                ...state,
                todos: action.payload,
                loading: false,
                error: null
            }
    }
}
```

**actions/TodosActions.js**

```js
import * as types from '../types'

export const fetchTodos = () => async dispatch => {
    const res = await fetch(API)
    const data = res.json()
    
    dispatch({
        type: types.GET_TODOS,
        payload: data
    })
}
```

These were only `Redux`.

## Next.js with Redux

**_app.js**

```js
import App from 'next/app';
import React from 'react';
import { Provider } from 'react-redux';
import { createWrapper } from 'next-redux-wrapper';
import store from './store/store';

function MyApp({ Component, pageProps }) {
    return (
        <Provider store={store}>
            <Component {...pageProps} />
        </Provider>
    )
}

const makeStore = () => store;
const wrapper = createWrapper(makeStore);

export default wrapper.withRedux(MyApp);
```

## Calling fetchTodos and use TodosReducer in your component

```js
import React, { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { fetchTodos } from './store/actions/TodosActions';

export default function MyComponent() {
    const dispatch = useDispatch();
    const { todos } = useSelector((state) => state.todos)
    
    useEffect(() => {
        dispatch(fetchTodos())
    }, [])
    
    return (
        <div>
            {todos && todos.map(item => <h1 key={item}>{item}</h1>)}
        </div>
    )
}
```
