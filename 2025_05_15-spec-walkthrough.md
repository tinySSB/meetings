# TinySSB | 2025-05-15

Walked Kevin through the protocol / spec arcs

## Kevin's reflections

- overloaded terminology
    - Feed
        - social media: combination of multiple users input into a channel
        - this caused a lot of confustion
        - mix: may want to lean back towards Log
        - we need to give a reason for feed
            - if you give the lineage (class scuttlebutt), that's fine
        - is a feed a thing I publish to and I have for my whole life? can I have multiple
            - mix: haha, multi-device support D: (shares context)
    - Message
        - social media: we think about messaging a lot in this context
        - what is a message in tinySSB
            - mix: it's like a node
        - does the message point to the main-chain thing, or the whole extended content?
            - mix: yeah I think this is a problem
        - we need to really clearly define this
- really like the the "main chain" and "side chain"
    - ability to skip content we don't want

## Mix's reflections

Concepts I could not convey quickly/ easily:
- feed
    - what is a feed?
    - what's the feed_id?
    - confusion about feeds being single-writer (not multi-writer)
- distributed ledger
    - introducing this confused things more than helped
    - *how* there are multiple feeds and how they are single-write AND can interact was surprising
- message
    - knowing if it's the main chain or the main-chain plus side content?
    - logical / virtual messages?
    - what's a message_id?
        - where is it used? is it validation?
- sequence
    - need to mention this is an incrementing integer
    - need to communicate what it's used/ useful for


What went well:
- tested the arc I wrote up in last meeting notes and it went over well for the most part
- DMX seemed to go over no problem!
- sketching diagrams worked fairly well to disambiguate a bunch of the above problems
    - ⇒ make more diagrams for Feeds, Messages, Chunks etc.

Didn't cover:
- replication details (GOSET, claims, wants)
