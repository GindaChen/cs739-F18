# CS739 Week6-1 Note



1. Availability :  when requested  
2. Reliability  :  as specified/expected
3. Serviceability: repair/maintain
4. Safety   : without catastrophic failure
5. Security : against accidental/deliberate intrusion

File System Server:

1. Read/write
2. Support 10^5 users
3. High performance: comparable to local disk

**File System Server Expectation**

Availability: all time servicable, all place 

Reliability: correct data, complete data



**Goals**

1. Failure and recovery should be Transparent to user. A failure must not force operation in progress to termination.
2. Failure-free performance must not be penalized to provide high availbility.
3. NFS client protocol implementation should not require modification to use HA-NFS server.

The beauty of system: a simple, focus view to solve problem



**Failure Model - 3 Sub Problem**

Disk Failure: mirror files on different disks

Server Failure: shadowing separate server

Network Failure: replicate network medium





**Normal Operation**

==[!] Log operation==

? Non-idempotent operation: delete, unlink

? Server alive: liveliness check message (`RPC_REQUEST`)

? Failure detection: `ping`

? Server loaded / failed: take-over



**Take-over**

? Really failed?



**Re-Integration**

Q? What about the currently requested RPC Message?

A! Fencing. If no fencing, the re-integration will never happen.



**Network Failure**



**Changes**

1. Log operation: don't have to wait until metadata flush in.



**?? Can you design HA-EXT4, etc. ?**





**Key Evaluation**

1. Speed and performance of take-over
   1. Log & cache size
   2. Number of Online Volumnes (# of disks) 
2. Speed of Reintegration
3. Accuracy & speed of failure detection
4. Performance at normal operation and when the take-over, reintegration happens.





**Exercise**

1. What are other way to address availability?
   1. Replicated Server: <intro> - machine state might be different
   2. Redundancy
2. What option other than mirroring? RAID0123456
3. What if the components are unreliable? **Consensus/vote**





---



**Idempotent**: 

erase

unlink: the count of the file will drop, so when `fsck` the file will become an orphan.



