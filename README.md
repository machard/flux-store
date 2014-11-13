# flux-store

The missing store for vanilla Flux.


## Install

```js
$ npm install flux-store
```


## Usage

```js
var FluxStore = require('flux-store'); // that's us, such meta
var Dispatcher = require('./Dispatcher');
var Constants = require('./Constants');


var ActionTypes = Constants.ActionTypes;


var Store = FluxStore.extend({
    stateStuff: {
        loading: false,
        objectFoo: {},
        arrayOfBaz: []
    },
    getStateStuff: function () {

        return this.stateStuff;
    },
    onDispatcherAction: function (payload) {

        var action = payload.action;

        if (ActionTypes.SEND_REQUEST === action.type) {
            Store.stateStuff.loading = true;
            Store.emitChange();
        }

        if (ActionTypes.RECEIVE_RESPONSE === action.type) {
            Store.stateStuff.loading = false;
            Store.stateStuff.objectFoo = action.data.objectFoo;
            Store.stateStuff.arrayOfBaz = action.data.arrayOfBaz;
            Store.emitChange();
        }
    }
});


Store.registerDispatcher(Dispatcher);


module.exports = Store;
```


## The basic interface

#### `onDispatcherAction(payload)`

This is the handler for your `Dispatcher`'s action events.

#### `registerDispatcher(Dispatcher)`

This connects your `Store` to your `Dispatcher` and populates your `Store`'s
`dispatchToken` property so Flux stuff (like `waitFor`) continues to work.


## React change listeners

Two functions are included in your `Store` that you can use with your React
controller-views.

#### `addChangeListener(callback)`

You can use this in `ControllerView.componentDidMount` like:

```js
componentDidMount: function () {

    Store.addChangeListener(this.onStoreChange);
},
```

#### `removeChangeListener(callback)`

You can use this in `ControllerView.componentWillUnmount` like:

```js
componentWillUnmount: function () {

    Store.removeChangeListener(this.onStoreChange);
},
```


## License

MIT


## Don't forget

What you make with `flux-store` is more important than `flux-store`.