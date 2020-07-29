# LitElement tutorial starter

This is the starting point for a tutorial series teaching you how to build an application with LitElement, Redux, Webpack, and Vaadin router.

You can find the text and video tutorials here:
https://vaadin.com/tutorials/lit-element

## Getting Started

```
npm i
npm run dev
```

## Tutorial Notes

### Template with lit-html

| text | description |
| ---- | ----------- |
| `value="${}"` | Set an attribute (string) |
| `.value="${}"` | Set a property (object) |
| `?value="${}"` | Toggle a boolean attribute |
| `@event="${}"` | Bind an event |

### Using Redux to maintain State

[Redux site](https://redux.js.org/introduction/core-concepts)

Create a `reducer.js`, i.e.

```js
const INITIAL_STATE = {
  todos: [],
  filter: VisibilityFilters.SHOW_ALL
};

export const reducer = (state = INITIAL_STATE, action) => {
  switch (action.type) {
    default:
      return state;
  }
};
```

To use the reducer and access the state we need to set up a store. It allows our views to subscribe to state changes and dispatch actions to update the state.

Create `store.js`

```js
import { createStore } from 'redux';
import { reducer } from './reducer.js';

export const store = createStore(
  reducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__() 
);
```

The `REDUX_DEVTOOLS_EXTENSION` for Chrome is helpful for inspecting and understanding the state of your application. You can download it from the [Chrome Web Store](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)

Then, update the template file:

```js
import { connect } from 'pwa-helpers';
import { store } from '../redux/store.js';

-class TodoView extends LitElement
+class TodoView extends connect(store)(LitElement) {

-  constructor() { 
-    this.todos = [];
-    this.filter = VisibilityFilters.SHOW_ALL;
-    this.task = '';
-  }
+  stateChanged(state) { 
+    this.todos = state.todos;
+    this.filter = state.filter;
+  }
```

### Dispatching the Action

Now, we dispatch actions to update the state.  Sample action Object:

```
{
  type: ADD_TODO,
  task: 'Learn Redux'
}
```

There are two parts to dispatching:

1. Creating the action. We will do this in a new file `src/redux/actions.js`
2. Updating the state based on the action. We will do this in `src/redux/reducer.js`


By using Redux, we arguably increased the complexity of our application quite significantly without adding any new functionality. It’s reasonable to ask why you would want to do this.

The advantages of Redux are more apparent when your application grows, and several components update the state. Using distinct actions to update a central state makes it easy for us to ensure that all parts of the application are in sync.


