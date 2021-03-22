# Configure-Redux-for-Next-App

You need these 3 packages for redux to be added to the next app.

`$. yarn add next-redux-wrapper redux react-redux redux-thunk redux-devtools-extension`


Then add your folder structure for `redux`


    .
    ├── ...
    ├── redux        
    │   ├── reducers        
    │   ├── actions      
    │   └── types.js                
    └── ...

`type.js`

```js
export const ADD_TODO = 'ADD_TODO'
```
