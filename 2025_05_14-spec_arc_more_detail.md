# tinySSB - spec writing | 2025-05-14

From [last time](https://github.com/tinySSB/meetings/blob/main/2025_05_01-spec_arc.md)

## Proposal D
 
1. intro
  - peers replicating state
  - the state is ...
  - the replications is dance like...
2. replication of main feed
  - GOSET concept
  - `claim` messages => building a shared GOSET
  - `want` messages => setting up replication / request
3. feeds
  - simple pass: append only log
  - medium pass: we support some side chains for some stuff!
  - complex pass:
    - here's how you build it up given incoming wire packets
    - details on all the packet types, chunks


## Proposal D (more detail)

This is just Mix having a go at adding some more detail / ideas.
To be discussed!

### 1. Intro

Goals:
  - solarpunk infra - very light/ cheap, self-hosted
  - delay + partition tollerance
  - veracity / trust

Important concepts:
  - p2p
    - peer: Feed, Feed_Id
    - replication
    - signing
  - tiny size constraint
    - designed for LoRa
  - properties to expect
    - small messages only (mostly text-based apps)
    - async, "eventually consistent" (not real-time)
    - slow

Not covered here:
  - how feeds are constructed
  - signing details
  - how replication is done


### 2. Replication

Goals:
  - coordinate the replication of messages with neighbouring peers
  
Simple pass:
  - we want to minimize the amount of redundent messages sent
  - replication is a two part dance:
    1. Replication Setup Dance
    2. Sending + Receiving Messages
  - the "Replication Setup Dance" we also has two parts:
    - a) agree who we're talking about (Feed_Ids)
    - b) express our needs/ wants
  
Important concepts:
  - Feed (basic)
    - Feed_Id
    - sequence - incrementing number
  - "GOSET"
    - purpose: make want coordination easier
    - the set of feeds we're coordinating around
    - Claims (wire packet)
  - Wants (wire packet)
    - DMX
  
Not covered here:
  - feed message format
  - feed construction (other than that it's a linear-linked list)
  - sending messages - wire format
  - receiving messages - what to do when you receive a message

Limitations:
  - current GOSET only supports 256 feeds!
  - spammable
  
  
### 3. Feeds

Goals:
  - the details of a feed of messages is presented as/ constructed from wire-packets

Simple pass:
  - a feed is linear, linked-list of signed messages
  - we have a "main chain" of signed messages
  - we store content that doesn't fit in the main chain in "side chains"
  - we don't have space for all the header data, but we have a fancy way to avoid that! (DMX)

Important concepts:
  - DMX
  - ...


Not covered here:



## Future Wonderings

Feed_Id could be decoupled slightly from Public_Key
  - e.g `Feed_ID = <Public_Key>/<Channel>`  where `Channel: u8`
