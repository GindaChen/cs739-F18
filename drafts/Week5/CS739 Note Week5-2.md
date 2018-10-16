# CS739 Note Week5-2

# Summary

[TOC]

**Review of the Course Structure**

1. Fundamentals of Distributed System
   1. Design Pattern 
   2. System Failure
   3. Network Communication
   4. Time
2. *[Now]* Techniques: 
   1. Local node
   2. `here ->` Distributed Systems

**Today**: 

1. NFS & idempotency: How do we design simple, scalable, network file system  with idempotency guarantee?
2. Leases: we have dealt with concurrency, but if one acquired a lock and died? **What do we do about accidents and fail prevention during concurrent events?**



# NFS

Distributed System back in the 1980s-1990s : client-server system



### **How to sacle server-side nodes?**

1. Scale out: add more server nodes (introduce many complexities, more challenging)
2. Scale up: make the server more powerful (easy, not worry about replicate objects, caches, async, etc.)



### **Main Goal**

1.Transparency: performance, semantics

2.Easy failure handling: server-side failure 

​	**Statelessness**

​	(Why not talk about client: just retry, and the impact is limited) 

​	=> unifies packet loss and server crase

​	When we think of **availability** = <avil>/<recover>+<avil>

​	`open` introduce the fs descriptors, which is stateful.



### Protocol

**Files Handle**: [Volume #, Inode #, *Generation #*]

Why Generation # important: useful when across file deletion and reuse.



Think of the [idempotency](https://en.wikipedia.org/wiki/Idempotence):

- `read/write`: should not have an implicit offset tracked by the os
- `lookup`: should not implicitly obtain a previous state
- `stat/getattr`: should not alter the orginal content, should return the newest result.




### From protocol to FS

**Confusion**: FS API is not FS Protocol -- protocol design decoples the API with the system functoin support.



|      | API   | Protocol |
| ---- | ----- | -------- |
|      | open  | lookups  |
|      | read  | reads    |
|      | write | writes   |



### How to handle Failure

A uniform way: **RETRY**



Case 1. Client downed / server crash (slow)

Just retry.



Case 2. Server reply lost: server will do someting twice

Cache the recent result and retry.





Becareful with RETRY: 

1. Retry too quickly.
2. Retry never end. Infinite retry loop

(Remember everything is good. Remember non is good. Remember half is bad.)



### Performance Problem

NFS: Client-side caching

**Write cache helpful: Write intensive examples**

1. Quick delete and create
2. Quick rewrite
3. Aggregation

**Write cache not helpful: Cache consistency required scenario**

1. Vedio streaming
2. Computation that requires consistency

-> ? Things at the server changed and the client has written a new version of it.



**Solution:**

1. Check-before-use: Before use the cache, client ask if it's modified in server (getattr() request from client to server to get the update)
2. Flush-on-close: client close file will force flush all the update to the server





**Another problem: `getattr` calls are too often**

**Solution: attribute cache on client, timeout after 3 sec**



### Conclusion

NFS: **idempotency** - Powerful protocol design 





# Lease

> **Old man rambling about**
>
> Do not travel to the south in Feburary.
>
> We aim for suffering in class, but not maximum suffering.
>
>  -- Remzi



**Lease = locks with timeout**



## Background Example: [AFS](http://pages.cs.wisc.edu/~remzi/OSTEP/dist-afs.pdf)

**Read/write all local**

1. **On Open**: Client fetch the whole file and do operation on local disk cache.
2. **On close**: If file change, flush to server

Better scenario: capacity, client restart



**Andrew: How do I know if my cache is valid?**

V1 `TestAuth`: is my file still valid? -- wasting time (dominant traffic)

V2 callback: server will notify the client when the cache is available.



Problem V2:

1. Server crash: since not stateless, when server is up, the client will have to revalidate the info by notifying the server.
2. Client crash: how to notify? => **Leases: a lock with a time out at server side**

**LEASES**: you only have an exclusive access to a file within a time period





### Common operation

acquire_lease

use_lease

renew_lease

release_lease (or let it timeout)





### Tricky cases

#### **1.When can the lease by safely granted to another clinet?**

**Story**

Client 1 obtain the lease;

Client 2 says to Server she wants the lease;

Server tell Client 1 release;

Client 1 don't respond;

Server try to time it out.



**What will be happening**

1. client down
2. **The Clock** is different.
   1. C1 > S1: good case
   2. C1 < S1: bad case -- client still thinks she has the lease, but server actually recall the lease back.



**Solution**

**1.2 Server should wait for $T+\Delta T$ to avoid clock skew**







#### **2. Use leases to make sure operations to block server are serialized**

Client:

Lock server:

Block server:



[C1, C2] get lock from LS;



LS grant C1 a lease;

C1 issue op to BS;

BS reply to C1;

C1 release end



LS grant C2 a lease;

...



1. **Time skew:** **Race condition** – Client still think they have the lease, but lock server will issue the lease to another client 





**Solution**

**Fence - Extra protection to get something reliable**: include info about the lease in a request. BS can reject the request based on the infor she knows.