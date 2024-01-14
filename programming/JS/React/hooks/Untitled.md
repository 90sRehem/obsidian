# A Custom useStateMachine React Hook

[

![Asís García](https://miro.medium.com/v2/resize:fill:88:88/1*P40OcXy3OsXTQEB5rbBR6g.jpeg)









](https://medium.com/@asis.garcia?source=post_page-----5cd1dd9947c1--------------------------------)[

![Trabe](https://miro.medium.com/v2/resize:fill:48:48/1*SFCMAgAttT7air4aUZAO5g.png)











](https://medium.com/trabe?source=post_page-----5cd1dd9947c1--------------------------------)

[Asís García](https://medium.com/@asis.garcia?source=post_page-----5cd1dd9947c1--------------------------------)

·

Follow

Published in

[

Trabe

](https://medium.com/trabe?source=post_page-----5cd1dd9947c1--------------------------------)

·

4 min read

·

Apr 6, 2020

205

[

](https://medium.com/plans?dimension=post_audio_button&postId=5cd1dd9947c1&source=upgrade_membership---post_audio_button----------------------------------)

![](https://miro.medium.com/v2/resize:fit:700/1*rHkQmQ8PW8BQbcqdUPEKDw.jpeg)

Photo by [Chad Kirchoff](https://unsplash.com/@cakirchoff?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/engine?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

In his post [Stop using isLoading booleans](https://kentcdodds.com/blog/stop-using-isloading-booleans), 

[Kent C. Dodds](https://medium.com/u/db72389e89d8?source=post_page-----5cd1dd9947c1--------------------------------)

 tells us to… well, to stop using booleans to keep track of binary states 😅. The problem with using a boolean flag to model our component’s state is that, usually, the flag is just part of a bigger, more complex state.

When you encode your component’s state in a bunch of binary flags, you end up rendering with a lot of conditional checking. That leads to code that is really difficult to maintain and extend.

Instead of using boolean values, Kent recommends using a [state machine](https://en.wikipedia.org/wiki/Finite-state_machine). In his post he uses [xstate](https://github.com/davidkpiano/xstate), a fully-fledged state machine library.

In this story I will offer a simpler alternative implemented as a Custom Hook using `useReducer`.

Of course, because it is simpler, this alternative is also less powerful, so if you are building a complex component, give xstate a try!

# Defining useStateMachine

Quoting the Wikipedia, a state machine (or finite state machine, FSM) is

> […] an abstract machine that can be in exactly one of a finite number of _states_ at any given time. The FSM can change from one state to another in response to some inputs; the change from one state to another is called a _transition_. An FSM is defined by a list of its states, its initial state, and the inputs that trigger each transition

So, in order to build our own state machine we need:

1. A list of states.
2. A list of events (inputs).
3. A list of transitions (current state + event => next state. 🤔 this one looks familiar…).

We are going to start designing the API for our `useStateMachine` custom Hook. What we want is something like this:

const [currentState, sendEvent] = useStateMachine(machineSpec);

The Hook accepts a machine specification object defining the states, inputs and transitions, and returns the current state and a method to send an event to the machine, to make it change (transition) to its new state.

Say we want to use a state machine to handle the connection to some remote service. Our user starts disconnected, and we want to let her connect to the service and disconnect from it. We can represent that machine like this:

The spec encodes all the information needed to run the machine:

1. The list of states, corresponding to the keys of the `states` property.
2. The list of transitions, defined for each state using `EVENT_NAME: “nextState”` pairs.
3. The initial state, so we know what to do when the first event is received.

We can use that information to define our custom Hook:

As you can see, our custom Hook is just a special case of `useReducer`: given the spec, we use the state transitions in the `states` property to build a reducer function, and the `initialState` as the reducer’s initial state. The return value of `useReducer` meets our API requirements: we get the current state and a function to send (dispatch) events that will, in turn, update the machine’s state 😀.

# Using useStateMachine

Now we can use our custom Hook to “drive” our application rendering and interactions:

For each possible state, a different component is rendered. Each component receives a set of callbacks to send events to the state machine and uses those callbacks to handle the user interaction.

![](https://miro.medium.com/v2/resize:fit:500/1*QvMoMEsfh75GvPCq44jHug.gif)

# Testing

`useStateMachine` is just a glorified `useReducer`, so it’s pretty easy to test. You can extract your state machine to your own custom Hook and test it using testing tools like [react-hooks-testing-library](https://github.com/testing-library/react-hooks-testing-library). Or you can test the reducer function itself:

# Maintenance

The great thing about using a state machine is that the spec encodes every possible state of your component, and also every valid transition from state to state. This makes adding new states and transitions really simple.

Let’s modify our example to take into account possible connection errors. Now our state machine looks like this:

Now, while we are in the `connecting` state, we can receive a `CONNECTION_ERROR` event and transition to the `error` state. When in the `error` state, we can try to `CONNECT` again.

We update our components to handle the new state:

![](https://miro.medium.com/v2/resize:fit:500/1*s4g2VeYDebBv6Ch7Z9A34w.gif)

State machines are a very powerful tool for designing UI components. As we’ve seen in this story, even a naive implementation can be really helpful. You can start with something like this, and then, if you need to, go the full-blown-state-machine way and add transition guards, effects, nested machines, and all the other goodies that libraries like [xstate](https://xstate.js.org/docs/) offer.