---
layout: post
title:      "redux-thunk"
date:       2020-08-03 01:01:06 +0000
permalink:  redux-thunk
---


![State](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/abf88135-c431-43f2-b70d-720318ba2255/redux-state-manage-power.png)

## What is a thunk?

A "thunk" is a term for a function that is returned by another function

```javscript
function normal_function() =>{
    function thunk()={
      console.log('I am the thunk');
    }
}
```

## Redux

In Redux, we have **actions**, **action creators**, and **reducers** which we use to manage the state in our app.

### Actions

Actions are simple objects. Ironically, although they are called actions, they don't do anything. ***They are just objects***

```javascript
// 1. plain object
// 2. has a type
// 3. anything else you need

{
  type: "USER_LOGGED_IN",
  user: "rukshan.uddin"
}

```

### Action creators

Action creators are used to create **actions**. They are functions that will return an **action**.

```javasript
// 1. function
// 2. returns an action (object)
// 3. easier than writing out an action every single time
//    we can just call the function

function userLoggedIn() {
  return {
    type: 'USER_LOGGED_IN',
    user: 'rukshan.uddin'
  };
}

```

### Reducers

**Reducers** specify how the application's **state** changes in response to **actions** sent to the **store**.

**Actions** are just objects. They describe what happened, but don't describe how the application's state changes. We need the **reducers** to do that for us.

```javascript
import { UserLoggedIn } from './actions'

const initialState = {
    loggedIn: false
    user: {}
}

function userReducer(state, action) {
  switch(action.type){
	case "USER_LOGGED_IN":
	  return {
		   loggedIn: true,
			 user: {...action.payload},
}
```

### Store

All of the above is nice, but alone useless. The **store** is the object that brings them together. The **store** has the following responsibilities:

- Holds application **state**;
- Allows access to state via ```getState();```
- Allows state to be updated via ```dispatch(action);```
- Registers listeners via ```subscribe(listener);```
- Handles unregistering of listeners via the function returned by ```subscribe(listener)```.

## Middleware
Middleware, in Redux, provides a third-party extension point between **dispatching an ***action***, and the moment it reaches the **reducer**.

People use Redux **middleware** for logging, crash reporting, routing and in the case of **thunk**, talking to an asynchronous API among other uses.


### What is redux-thunk?

**redux-thunk** is an extremely small library:

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }
    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

### What it does...

When you install and apply the thunk middleware in you application. Every action you dispatch will pass through the code above. Since **Redux** needs an action to be an object, we use thunk to be able to make an asynchronous API call as a Redux action. Every action goes through thunk. Thunk will check to see if it is a function in the 3rd line:

```
 if (typeof action === 'function')
```

If it is, it will return an action based on the next lines:

```
{
      return action(dispatch, getState, extraArgument);
    }
		
```
		
It will call the function and return whatever is returned back to Redux.

### Uses

We can use this to fetch data from an API in **Redux**

```
export const logUserIn = (userInfo) => {
  return (dispatch) => {
    return fetch(`http://localhost:3000/login`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
      },
      body: JSON.stringify({ auth: userInfo }),
    })
      .then((res) => res.json())
      .then((data) => {
        if (data.error) {
          return console.log(data.error);
        } else {
          localStorage.setItem("token", data.jwt);
          dispatch({ type: "USER_LOGGED_IN", payload: data.user });
        }
      });
  };
};
```

Here, we do not have to worry about **Redux** needing an object as an action. **thunk** will let us use a function and let the return value from our async function be what we dispatch to the reducer.
