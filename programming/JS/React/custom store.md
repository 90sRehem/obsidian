```js
import { useSyncExternalStore } from "react";

  

export function createStore<Shape>(initialState: Shape) {

let currentState = initialState;

const listeners = new Set<(state: Shape) => void>();

const subscribe = (listener: (state: Shape) => void) => {

listeners.add(listener);

return () => listeners.delete(listener);

};

const useStore = <SelectorOutput>(

selector: (state: Shape) => SelectorOutput,

): SelectorOutput =>

useSyncExternalStore(

subscribe,

() => selector(currentState),

() => selector(initialState),

);

  

return {

getState: () => currentState,

setState: (newState: Shape) => {

currentState = newState;

listeners.forEach((listener) => listener(currentState));

},

subscribe,

useStore,

};

}

  

const store = createStore({ count: 0 });import { useSyncExternalStore } from "react";

  

export function createStore<Shape>(initialState: Shape) {

let currentState = initialState;

const listeners = new Set<(state: Shape) => void>();

const subscribe = (listener: (state: Shape) => void) => {

listeners.add(listener);

return () => listeners.delete(listener);

};

const useStore = <SelectorOutput>(

selector: (state: Shape) => SelectorOutput,

): SelectorOutput =>

useSyncExternalStore(

subscribe,

() => selector(currentState),

() => selector(initialState),

);

  

return {

getState: () => currentState,

setState: (newState: Shape) => {

currentState = newState;

listeners.forEach((listener) => listener(currentState));

},

subscribe,

useStore,

};

}

  

const store = createStore({ count: 0 });
```
