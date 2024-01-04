```typescript
import { useEffect, useState } from "react";

type Machine = {
initialState: string;
states: {
[key: string]: {
actions: {
onEnter?: () => Promise<void> | void;
onExit?: () => Promise<void> | void;
};

transitions: {

switch: {

target: string;

action: () => Promise<void> | void;

};

};

};

};

};

  

export interface StateMachine<S extends string, A extends string> {

initialState: S;

states: {

[K in S]: {

onEnter?: () => Promise<void> | void;

onExit?: () => Promise<void> | void;

transitions: {

[K in A]?: S;

};

};

};

// matches: (state: string) => boolean;

}

export const useStateMachine = <S extends string, A extends string>(

machine: StateMachine<S, A>,

) => {

const [currentState, setCurrentState] = useState(machine.initialState);

  

const transition = async (action: A) => {

const nextState = machine.states[currentState].transitions[action];

if (!nextState) {

console.error(`Invalid transition: ${currentState} -> ${action}`);

return;

}

  

try {

if (machine.states[nextState] && machine.states[nextState].onEnter) {

await machine.states[nextState].onEnter?.();

}

} catch (error) {

console.error(`Error in onEnter for state ${nextState}:`, error);

}

  

setCurrentState(nextState);

};

  

useEffect(() => {

const currentStepObject = machine.states[currentState];

currentStepObject?.onEnter?.();

  

return () => {

currentStepObject?.onExit?.();

};

}, [currentState, machine.states]);

  

const current = {

state: currentState,

matches: (state: S) => currentState === state,

};

  

return [current, transition] as const;

};

  

export function createMachine<S extends string, A extends string>(

machine: StateMachine<S, A>,

): StateMachine<S, A> {

return machine;

}

  

const stepsMachine = createMachine({

initialState: "data",

states: {

data: {

onEnter: () => console.log("onEnter data"),

onExit: () => console.log("onExit data"),

transitions: {

abs: ""

}

},

loading: {},

},

});

```