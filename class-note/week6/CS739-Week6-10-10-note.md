# CS739 Week6-1 Note (10/10, W)

`@Lecturer:`. He's an expert in NFS, and he's questions were very good for people who read the paper carefully, and also those who didn't : )

He's lecture is even better than Remzi's : ) (though Remzi's are good too)

## Summary



[TOC]



## Lecture

### What to consider when designing a FS

1. Availability :  system should work when requested  
2. Reliability  :  system should work as specified/expected
3. Serviceability: system repair/maintain themselves if possible
4. Safety   : system should work without catastrophic failure
5. Security : system should work against accidental/deliberate intrusion



### Demands for <u>File System Server</u>

1. User can easily read/write on a file (just like they do on local)
2. Server should support more than 10^5 users
3. Server should be high performance: comparable to local disk

<u>File System Server</u> is something like NFS and AFS that serves the whole cluster as a unified storage system.



### File System Server Expectation

**Availability**: servicable at *all time*, in *all place* 

**Reliability**: FS should only store *correct data*, and if the data is stored it should be the *complete data*



### Goal of the Design

1. Failure and recovery should be <u>Transparent</u> to user. A <u>failure must not force operation in progress to termination</u>.
2. Failure-free performance <u>must not be penalized to provide high availbility.</u>
3. NFS client protocol implementation <u>should not require modification to use HA-NFS server</u>.

The beauty of system: a simple, focus view to solve problem



### Failure Model - 3 sub-problem's solution in the paper

- **Disk Failure**: mirror files on different disks
- **Server Failure**: shadowing separate server
- **Network Failure**: replicate network medium



### Architecture

#### 1. Normal Operation

==//TODO: [!] Log operation (I forgot what he talked about here...)==

? What should we do for some **Non-idempotent operation**: e.g., `delete`, `unlink`

? How do we check **Server aliveness**: liveliness check message (`RPC_REQUEST`)

? How do we do **Failure detection**: `ping`

? What should a good server do when it detects its partner server is full-loaded (low-response) or  failed: **take-over**



#### 2. Take-over

? How do we distinguish between server full-loaded (low-response) and srever failed: 



#### 3. Re-Integration

Q? What about the currently requested RPC Message?

A! **Fencing**. If no fencing, the re-integration will never happen.



#### Network Failure

==//TODO: we talked a little bit about network failure==



### Changes that can be made

1. Log operation: don't have to wait until metadata flush in.

==//TODO: I forgot if there're things underneath..==



**Challenge: Can you design HA-EXT4, etc. ?**

### Performance: the Key Evaluations

Some performance test we can think of when we want to benchmark the system:

1. Speed and performance of take-over
   1. Log & cache size
   2. Number of Online Volumnes (# of disks) 
2. Speed of Reintegration
3. Accuracy & speed of failure detection
4. Performance at normal operation and when the take-over, reintegration happens.

**What they actually evaluate:**

1. (Section 4.1) The effect of disk logging
2. (Section 4.2) The effect of Mirroring
3. (Section 4.3) Take-over and Re-integration

Why? becuase



### Exercise

1. What are other way to address availability?
   1. Replicated Server: <intro> - machine state might be different
   2. Redundancy
2. What option other than mirroring? RAID0123456
3. What if the components are unreliable? **Consensus/vote**



## Questions

**Question 1.** What if both servers failed:

**Answer 1.** Then there is no hope to recover the data. This model assume the probability of two server failed at the same time to be an extremely rare event (think of )

==**Question 2.** How can you design HA-EXT4, etc.?==



## Side Note

**Idempotent**: 

erase

unlink: the count of the file will drop, so when `fsck` the file will become an orphan.



