# CS739 Week6-1 Note

`@Lecturer:`

He's lecture is even better than Remzi's : ) (though Remzi's are good too)

## Lecture Detail

### What to consider when designing a FS:

1. Availability :  system should work when requested  
2. Reliability  :  system should work as specified/expected
3. Serviceability: system repair/maintain themselves if possible
4. Safety   : system should work without catastrophic failure
5. Security : system should work against accidental/deliberate intrusion

### Demands for <u>File System Server</u>:

1. User can easily read/write on a file (just like they do on local)
2. Server should support more than 10^5 users
3. Server should be high performance: comparable to local disk

### File System Server Expectation

**Availability**: servicable at all time, in all place 

**Reliability**: correct data, complete data



### Goals

1. Failure and recovery should be <u>Transparent</u> to user. A <u>failure must not force operation in progress to termination</u>.
2. Failure-free performance must not be penalized to provide high availbility.
3. NFS client protocol implementation should not require modification to use HA-NFS server.

The beauty of system: a simple, focus view to solve problem



### Failure Model - 3 sub-problem's solution in the paper

- **Disk Failure**: mirror files on different disks
- **Server Failure**: shadowing separate server
- **Network Failure**: replicate network medium



### Architecture

#### 1. Normal Operation

==[!] Log operation==

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



### Changes

1. Log operation: don't have to wait until metadata flush in.



**?? Can you design HA-EXT4, etc. ?**





### Performance: the Key Evaluations

1. Speed and performance of take-over
   1. Log & cache size
   2. Number of Online Volumnes (# of disks) 
2. Speed of Reintegration
3. Accuracy & speed of failure detection
4. Performance at normal operation and when the take-over, reintegration happens.



### Exercise

1. What are other way to address availability?
   1. Replicated Server: <intro> - machine state might be different
   2. Redundancy
2. What option other than mirroring? RAID0123456
3. What if the components are unreliable? **Consensus/vote**

---



**Idempotent**: 

erase

unlink: the count of the file will drop, so when `fsck` the file will become an orphan.



