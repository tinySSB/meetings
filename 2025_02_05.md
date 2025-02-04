# Tiny-SSB - JS API design | 2025-02-05

Purpose: get together and design the next steps for a clear JS API

Attendees: Andrew, Mix, Nanomonkey, TyChi

Source: https://github.com/tinySSB/tiny-shed-design/pull/5
Source 2: https://github.com/ssbc/tinySSB/tree/main/android/tinySSB/app/src/main/assets/web


## Backend API

this is where we send commands for the backend to "execute"

```js
function backend (cmdStr) { // send this to Kotlin (or simulate in case of browser-only testing)

      // >> BEGIN INSERT VIRTBACKEND
  if (cmdStr == 'ready' && window.location.search == '?virtualBackend=true') { return }
  if (virtBackEnd != null) {
    virtBackEnd.postMessage(['f2b', cmdStr], '*')
    // NOTE: cmd is posted to backend, which processes storing, broadcasting, and sometime other actions.
    // This can also include reformatting + bouncing back a "new_event" message to the frontend (here)
    return
  }
  // << END INSERT VIRTBACKEND

```

https://github.com/ssbc/tinySSB/blob/main/android/tinySSB/app/src/main/assets/web/tremola.js

https://github.com/ssbc/tinySSB/blob/main/android/tinySSB/app/src/main/assets/web/tremola.js

cmdStr array:
- [0]:
    - ready
    - publ:post
    - priv:post
    - kanban
    - IAM
    - add:contact
    - invite:redeem
    - dump:
    - process.msg
    - importSecret
    - exportSecret
    - conf_dlv
- [1]:
    - ???
    - `val` [reference](https://github.com/ssbc/tinySSB/blob/main/android/tinySSB/app/src/main/assets/web/tremola.js#L92)
    - `json['secret']` ([link](https://github.com/ssbc/tinySSB/blob/main/android/tinySSB/app/src/main/assets/web/tremola.js#L297))
- [2]:
    - prev
        - array?
        - format?
- [3]:
    - content?
        - format?

- [4]:



```ts

function backend('ready')
function backend('exportSecret')
function backend('wipe')
function backend('publ:post', _: unknown, draft: Array<unknown>)
function backend('kanban')


```

potential insight for message payloads from webview to native (kotlin): https://github.com/ssbc/tinySSB/blob/main/android/tinySSB/app/src/main/java/nz/scuttlebutt/tremolavossbol/WebAppInterface.kt#L98


## Frontend API

here we can listen to events that are emitted


from backend:

```js
var e = {
  header: {
    tst: Date.now(),
    ref: Math.floor(1000000 * Math.random()),
    fid: IDs[myname],
    seq: myseqno
  },
  confid: { type: 'post', text: draft, recps: cmd },
  public: {}
}
frontEnd.postMessage(['b2f', 'new_event', e], '*')
```

```js 
var e = {
  header: {
    tst: Date.now(),
    ref: Math.floor(1000000 * Math.random()),
    fid: IDs[myname],
    seq: myseqno
  },
  confid: { type: 'post', text: draft, recps: cmd },
  public: {}
}
frontEnd.postMessage(['b2f', 'new_event', e], '*')
```

```js 
// cmd = ['KAN', ...??]


// kanban
var e = {
  header: {
    tst: Date.now(),
    ref: Math.floor(1000000 * Math.random()),
    fid: IDs[myname],
    seq: myseqno
  },
  public: cmd
}
frontEnd.postMessage(['b2f', 'new_event', e], '*')
```


```js
event.source.postMessage(['b2f', 'exportSecret',
  'secret_of_id_which_is@AAAA==.ed25519'], '*')
```


## Observations / Questions

1. seems like this area needs a refactor for readability
2. named arguments would help a lot
    - object with parameters
3. we could read Kotlin to see how backend
    - https://github.com/ssbc/tinySSB/blob/main/android/tinySSB/app/src/main/assets/web/tremola.js#L306 -> https://github.com/ssbc/tinySSB/blob/main/android/tinySSB/app/src/main/java/nz/scuttlebutt/tremolavossbol/WebAppInterface.kt#L98
5. Lets move away from 3-letter function names, variable
6. all the formatting / encoding should happen in only one place
7. no magic strings
    - extract into a file and document
    - different sorts
        - backend commands
        - message types
8. for events emitted, need
    - documentation on the different fields
    - standardisation 
9. the b2f API has 2 parts
    - top level: `[b2f, "new_event", e]`
        - this could be an object with named params too
    - the event: `e`

## Synthesis

Docs driven API
- write the docs / README for the new API
- get feedback
- implement it



