# TinySSB task planning | 2024-11-27

Present: Mix, Nanomonkey

## tiny-ssb protocol

- design decisions
  - broadcast only
  - you should know if you've missed messages
  - packets must fit into LoRa
  
- feed 
  - feed format
  - message format
    - cryptographic signature
    - DMX header (computed, deterministic backlink?)
    - payload
  - side-chain/ off-chain
  
- peer interaction
  - how to listen
  - broadcasting your state / wants??
  - broadcasting responses??

## tiny-ssb application requirements (opinionated/ flexy)

### storage

- append-only-log file
- inMemory
- browser localStorage
  
Q: are feed storage and side-chain storage same/ different? 
  
### transport

- Bluetooth
- LoRa
- WebSocket


## Code map

- Android
  - Kotlin backend
    - feed format
    - peer interaction
    - persitence
  - JS frontend
    - Q: how does it read/ write? API?
    
- micro-controller C code
  - implements "feed"
    - NOTE: cannot write it's own feed
  - implements "peer interaction"
  - transports: LoRa, Bluetooth

- Python server
  - what is it, what can it do?
  - 

- Browser based (idea)
  - talks across tabs
  - implements "feed" + "peer interaction"
  - should play with other implementations (e.g. python relay)

## Test vectors

We should build some test vectors for e.g. message creation, DMX, signing etc.
In scuttlebut envelope-spec, we made these as JSON objects which different languages could ingest and test against.

```json
{
  "type": "box",
  "description": "box for 2 recipients (with different key management scheme)",
  "input": {
    "plain_text": "c3F1ZWFtaXNoIG9zc2lmcmFnZSDwn5io",
    "feed_id": "AACv6zOVZsd3N5mVYJs7MnmMRu08DfGmqG70+0mL0SfHUQ==",
    "prev_msg_id": "AQBP+SmjFm1B7PJ7bSaIa3JhkqMYdmlIKUtodYfE9o8/qw==",
    "msg_key": "Sio94NHxP3k+Svx7d1VReGxHdh2T9wssofb1v7Lt9ao=",
    "recp_keys": [
      {
        "key": "gVv33+Jo5348A1XJrA+hoMxYiee13QlxNpm88yHwzRY=",
        "scheme": "envelope-large-symmetric-group"
      },
      {
        "key": "beAVypscgWbDyQw6oDUwX9Huf/5dwrlhE/OrStRsU0g=",
        "scheme": "envelope-id-based-dm-converted-ed25519"
      }
    ]
  },
  "output": {
    "ciphertext": "l0dEDrXwMx1VcKbnhrRy51jAtWyg5F23/6TZjWu00FyaNA+yTiRvO16ht93L18NmNWy1/KyjgKm++aiVqlE7sCqRT62OxLopHXVes/lWH9S97fhZHKCZg/vxZgdIkpefvNl/qsHerry0SnS7m5OoffpyE0wYzKqUbcMHnUyFAbtmgFvj69f+Ng=="
  },
  "error_code": null
}
```

Source: https://github.com/ssbc/envelope-spec/blob/master/vectors/box1.json


## Actions

Mix
- [ ] post these to meetings repo
- [ ] create issues
  - [ ] document the Android JS APIs 
- [ ] create repo: tiny-ssb/tiny-ssb-test-vectors
- [ ] track your time  

Nano
- [ ] create repo: tiny-ssb/tiny-ssb-protocol
- [ ] create issues
  - [ ] design decisions
  - [ ] some smallish patch chunks e.g.
    - [ ] base feed
    - [ ] side chains
    - [ ] peer interaction
    - [ ] ...
  - [ ] name
- [ ] track your time  

