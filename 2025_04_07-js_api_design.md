# TinySSB - JS API, State | 2025-04-07

Present: mix + adam

Purpose
- [x] align on API's JS uses to coordinate back/front-end
- [x] discuss state management in JS
- [x] get to know each other some!


## JS APIs

Previous call: https://github.com/tinySSB/meetings/blob/main/2025_02_05-js_api_design.md

TL;DR
- refactoring
  - modularization!
- documentation

### Backend to Frontend

#### Kotlin calling JS functions

right now the Kotlin code is calling `eval` of JS in the webUI
- is this the best pattern/ safe?
- can Kotlin not just sent messages to the frontend (with a listener set up there)

Kotlin docs seem to suggest you can declare some JS functions as external, then call
- seems safer than raw eval `eval`
- https://kotlinlang.org/docs/wasm-js-interop.html#kotlin-functions-with-javascript-code


#### Events

The backend communicates to the front end by emitting "events"

```
{
  type: String
  payload: Object?
}
```

type:
- initialization
- new_log  (new logId)
- new_log_message
- my_secret
- my_settings

```js
{
  type: "new_log_message"
  payload: {
    header: {
      logId: PublicKey,
      seq: Intenger,
      timestamp: UnixTime, // time received?
      // ref:  what is this, the msgId or some uuid?
    },
    confid: {
      type: "post",
      text: "hello world",
      recps: [logIdA, logIdB]
    },
    public: {}
  }
}
```

```js
// mini-app example
{
  type: "new_log_message"
  payload: {
    header: {
      logId: PublicKey,
      seq: Intenger,
      timestamp: UnixTime, // time received?
      // ref:  what is this, the msgId or some uuid?
    },
    confid: {
      type: "mini-app",
      subtype: "chess" // ??
      subpayload: {
        "chessMoveA": "knight b4 to c6"
      }
    },
    public: {}
  }
}
```

[backend] ---> [pre-defined frontend] --> [MiniApp Chess]
type: "mini-app", 
subtype: "chess"
subpayload: {

} --> Chess

flow of data
1. backend receives a new message (from another peer)
2. backend emits "new_message"
3. front end asks "does anyone know this type?"
4. ok, chess (mini-app), here
5. chess mini-app validates the format of the incoming message/ payload
6. if valid, gets passed in to update chess mini-app state (if not, it's dropped by chess mini-app)



### Frontend to Backend

This is where we currently have this "cmdString", and the proposal was to change towards an object

```js
{
  type: "publish_message",
  payload: {
    type: "min-app",
    subtype: "chess",
    subpayload: {
      move: "knight b4 to c6"
    }
  }
}
```

```js
{
  type: "ready",
  // payload: {}
}
```

Types:
- ready            // no payload
- IAM              // no payload, rename: who_am_i?

- publ:post
- priv:post

- add:contact      // rename: register_log_id
- invite:redeem

- dump:
- process.msg

- importSecret
- exportSecret
- conf_- 
- recps

- kanban           // To be deprecated


---


## State management in JS

Frameworks for state:
- [redux](https://redux.js.org/)  (react state management - may have been superceded?)
- [pinia](https://pinia.vuejs.org/)  (vue state management)
- [xstate](https://xstate.js.org/)?


Common pattern
- reducer - a pure function - deterministic, no side effects
- current_state `{ count: 1 }`
- action - `{ type, payload }`

```js
const next_state = reducer(current_state, action)
```



Persistence / recovery of state?
- didn't get ot this



