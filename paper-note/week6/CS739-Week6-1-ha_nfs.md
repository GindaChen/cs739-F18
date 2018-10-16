# CS739 Week 6-1: HA-NFS

A Highly Available Network File Server

1989, Anupam Bhide, Elmootazbellah N. Elnozahy



# Summary

**Network FS Reliability  = Server Reliability + Disk Reliability + Network Reliability** 

Solution:

- **Server Reliability**: dual-ported disk, impersonation
- **Disk Reliability**: disk mirroring
- **Network Reliability**: optional network replication

HA-NFS provide high availability without resource overhead / performance downgrade 

[TOC]

# Reinder

1. The article about NFS should be read before reading this one.



# Detail



## 1. Introduction

**HA-NFS**: High-Availability NFS

### Fail Model (three not-so-easy pieces)

#### Disk Failure

==//TODO: I don't even remember if he actually said anything about disk failure...==

#### Server Failure

1. **Dual-ported disk**: a bunch of disks (say 10~1000 disks in the shelf) are shared by a pair of connected server, each acting as a backup to the other. (The 2 servers are equal in their position, until someone failed)
   1. Disks are devided by two sets, each server will use one set in it.
   2. Server store its current volatile state in disk. When a server fail, the other server can use this information to store the volatile information on the disks that the failed server owned.
2. **Periodic liveness-checking message** (machine-wide heartbeat)
   1. RPC packet
   2. Ping
   3. Bridge (communicate through SCSI bus)
3. **Impersonation**
4. **Fast recovery** = mirroring
   1. Only use mirror for continuous availability
   2. Otherwise, archive backup

#### Network Failure

1. (Optional) **Replication of network component**. We can have more transmission medium to avoid network failure, but we will not recover packet on the net.



### Design Goal

1. Failure and recovery should be Transparent to user. A failure must not force operation in progress to termination.
2. Failure-free performance must not be penalized to provide high availbility.
3. NFS client protocol implementation should not require modification to use HA-NFS server.



### Used Machine 

1. IBM RISC System/6000
2. AIXv3
3. Ethernet 10 Mbit/s
4. Token Ring network 4Mbit/s



## 2. Background

### AIXv3ss

1. **Serializable & Atomic Modification** == transaction lock + logging 
2. **Logical volume**: can have up to three copies/replicates on different physical disk.
3. **Reply cache**: Most operation are idempotent (RPC), except `erase`, `unlink`, etc.



### Deal with Non-Idempotent Operations

Idempotent: https://www.restapitutorial.com/lessons/idempotency.html

Maintain a **reply cache** so that we can remember the successful, non-idempotent RCP.

==**Question**: What operations are non-idempotent? How does a reply cache should work ideally?==





## 3. HA-NFS Architecture

HA-NFS node = primary server + secondary server

1. Connected by SCSI bus/disk
2. **Primary server* for the disk is selected so taht  the total load is balanced between the two
3. Backup to each other
4. Client see the node as *TWO SEPERATE SERVER*
5. Normally, disks are served only by their corresponding primary server, and the other server does not access the buss

`*: `The *Primary Server* does not mean *one of the two server is the primary server and the other is slave*. This concept is telling us that <u>the disk recognize one of the server as the primary</u>, and for balancing purpose, *the primary server for a disk is selected so that the total workload is balanced between the two servers.*

==//TODO: These contents are written in the class note. I'm clearly sloppy here since some of the content below are even shown in the beginning of the pages... I will add them later I guess.==

### 3.1 Normal Operation

### 3.2 Take-Over

### 3.3 Re-Integration

### 3.4 Network Failure



## 4. Performance

### 4.1 The Effect of Disk Logging

### 4.2 The Effect of Mirroring

### 4.3 Take-over and Re-Integration



## 5. Related Works



# Questions



