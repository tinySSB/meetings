# TinySSB Spec call | 2025-04-07

Repo: https://github.com/tinySSB/tiny-ssb-spec

## Issues

https://github.com/tinySSB/tiny-ssb-spec/issues
Reviewed issues, commentend on these


## Terminology

- append-only log, or "log" in short form
  - a "log" has "log entries", of variable size. Even with a side chain, it's one entry
  - NOTE: in a log, there is only the main chain (sidechains are an implementation detail of storing entries of arbitrary size?)
  - has "log_id" / LID = public key used for verifying the signature

- replication of tinySSB append-only logs is, in this wire format, via fixed-sized packets (120B)
  - a "log entry" is mapped to one or more "wire packets"
    - one for a main chain (wire) packet
    - zero or more side chain (wire) packets
  - 
  
- "replication spec"/"peering protocol"
  - purpose is to replicate append-only logs, the content of the exchanged coordination packets is ephemeral (i.e., is not content of any log)
  - this is one way how peers might coordinate which log packets are sent
  - the replication (coordination) protocol might be it's own spec
  - coordination packets are mapped to the same 120B packet constraint, but the internal format is different,
  - "coordination packets" live in harmony along side with the "log packets", thanks to the DMX values which allows the receiving peer to demultiplex them
  
  
### deprecated

- feed, feedId, FID => replace with LID
  - background: "feed" stresses that one wants to subscribe to log extensions, but is used as a synonym for "log"
  
- blobs => we just have variable length (log) entries
