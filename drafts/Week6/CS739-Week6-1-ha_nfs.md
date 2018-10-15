# CS739 Week 6-1 HA-NFS

A Highly Available Network File Server

1989, Anupam Bhide, Elmootazbellah N. Elnozahy

# Summary

**Network FS Reliability  = Server Reliability + Disk Reliability + Network Reliability** 

Solution:

- **Server Reliability**: dual-ported disk, impersonation
- **Disk Reliability**: disk mirroring
- **Network Reliability**: optional network replication

HA-NFS provide high availability without resource overhead / performance downgrade 

# Questions



# Detail

## 1 Introduction

**HA-NFS: High-Availability NFS**

### Keywords / Feature

Disk Failure



Server Failure

1. Server Fail: Dual-ported disk
   1. Volatile state reconstruction information
2. Periodic liveness-checking message (machine-wide heartbeat)
3. Impersonation
4. Fast recovery = mirroring
   1. Only use mirror for continuous availability
   2. Otherwise, archive backup

Network Failure

1. Network failure toleration: Optional Replication of network component (==transmission medium?==, but not packet)

Goal

1. Failure and recovery should be **Transparent** to user. A failure must not force operation in progress to termination.
2. Failure-free performance must not be penalized to provide high availbility.
3. NFS client protocol implementation should not require modification to use HA-NFS server.

Machine

1. IBM RISC System/6000
2. AIXv3
3. Ethernet 10 Mbit/s
4. Token Ring network 4Mbit/s

## 2 Background

AIXv3: 

1. Serializable & Atomic Modification == transaction lock + logging 
2. Logical volume: can have up to three copies on different physical disk.
3. Reply cache: Most operation are idempotent (RPC), except some ==erasing file==.



## 3 HA-NFS Architecture

HA-NFS node = primary server + secondary server

1. Connected by SCSI bus/disk
2. Primary server balace the total load between the two
3. Backup to each other
4. Client see the node as *TWO SEPERATE SERVER*





Normally: ==? disks are served only by their corresponding primary server, the other server does not access the buss==

