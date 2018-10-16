`@author: Junda Chen`

# CS739 Week 5-2 Leases

Leases: An Efficient Fault-Tolerant Mechanism for Distributed File Cache Consistency

Cary G. Gray and David R. Cheriton, 1989

# Summary

Cache cause inconsistency. 

Lease introduce an efficient way to cache data and maintain consistency between caches and the file. 

Short leases limit the impact of non-Byzantine failure, and increase the performance especially on large scale & high processor performance system.

[TOC]



# Questions

**Q6 (due 10/5 before class):** Describe the trade-offs in setting lease length; what if the time is too short? too long?

**Pre-read questions**

1. What is **Byzantine failure** and **Non-Byzantine failure**?
2. What is the machanism of **Lease**?
3. What does **V-Syste**m talk about?
4. What problems are introduced by cache?

**In-read questions**

1. What is lease protocol? Why does he claim that leases are of benefit when introduced in the larger system?

**Post-read questions**



# Reminder

1. I strongly suggest to read the class note first before reading this note. The original paper has many ~~strange~~ math that you might want to scream.
2. Review [Poisson Process](https://en.wikipedia.org/wiki/Poisson_point_process) that the expected value of the arrival of event is actually the rate of Poisson Process. Meaning that $E(X) = \lambda $ if $X$ is  Poisson($\lambda$)
3. Review the process of Unicast, Multicast, Broadcast, Anycast and maybe Geocast. Get an understanding of how the packet transfer.



# Detail

==//TODO: add the illustration to the math parts==

## 1 Introduction

> Existing approaches to consistency for file cachesfall into two categories: 
>
> 1. Those that **assume reliable broadcast**, and so <u>do not tolerate communication failures</u>, and
> 2. Those that require **a consistency check for every read**, and so <u>fail to deliver good performance</u>.



File access characteristics? ==What was I writing here...?==



**Short-term leases provide near optimal efficiency for a large class of systems in spite of the fault-tolerance provisions**

> In this paper, **leases** are proposedasaconsistencyprotocol that handles host and communication failures using <u>physical clocks</u>. An analytic model and an evaluation using **file access characteristics** of the V system show that **short-term leases provide near optimal efficiency for a large class of systems in spite of the fault-tolerance provisions.** 
>
> We argue that leases are of increased benefit in future distributed systemsof larger scalewith their larger ratio of processor speed to network delay and larger aggregate rate of failure.



## 2 Leasesand Cache Consistency

**Lease**: holder can control the write.

**Lease holder**: The owner of the resource

**Term** of the lease: lease time.

> **Lease holder**: a lease grants to its holder control over writes to the covered datum during the term of the lease.
>
> **Grant lease**:When a lease holder grants approval for a write, it invalidates its local copy of the datum.
>
> A cache using leases requires a valid lease on the datum (in addition to holding the datum) before it returns the datum in responseto a read, or modifies the datum in responseto a write. 

The primary actions within the cache's lease

1. Lease issued (with the datum) from the server to the client (or say the cache)
2. Read cache within the lease on the cache device
3. Write cache within the lease on the cache device
4. Lease expired, and the read/write permission returns to the server

> When a datum is fetched from the server (the primary storagesite of the datum), the server also returns a lease guaranteeing that the data will not be written by any client during the lease rerm unless the server first obtains the approval of this leaseholder. 
>
> If the datum is read again within the term of the lease (and the datum is still in the cache), the cacheprovides immediate accessto the datum without communicating with the server.
>
> After the lease expires, a read of the datum requires that the cache first extend the lease on the datum, updating the cache if the datum has been modified since the lease expired. 
>
> When a client writes a datum, the server must defer the request until each leaseholder has granted approval or the term of its lease has expired.



**Write-Through cache**: no write that has been made visible to any client can be lost

> extending the mechanismtosupportnon-write-through cachesisstraightforward (why...)
>
> Write-through gives clean failure semantics: 
>
> **no write that has been made visible to any client can be lost**; applications must otherwise be prepared to recover from
> lost writes.

Example in Latex diskless workstation

> **Host approval**: When a new version of latex is installed, the write is delayed until every leaseholder has approved the write. 
>
> **Expire**: If some host holding a lease for this file is unreachable, the delay continues until the lease expires.

All kinds of write within the system

> the cache must also hold <u>the name-to-file binding</u> and <u>permission</u> information, and it needs a lease over this information in order to use that information to perform the open. 
>
> Similarly, modification of this information, such as <u>renaming the file</u>, would constitute awrite.

**Partitioning communication failure**: the communication between network partition failed.



### Advantages

**1.Cache at failure: minimize delay **

1. **Delay write at client failure**. When non-Byzantine fail happends, server must delay the write, wait for the lease to expire.
2. **Honor lease at server failure**. When a server is recovering after crashing, it must honor the leases it granted before it crashed.

**2.False Sharing: prevent** a lease conflict - 占着茅坑不拉屎

> False sharing introduce the overhead of a callback to the leaseholder(s) (thereby delaying the requesting client and loading the leaseholder andserver)

**3.Reduce storage requirement**

> The server requires a record of each leaseholder’s identity and a list of the leasesit holds; each lease requires only a couple of pointers. 
>
> **For a client holding about onehundred leases, the total is around one kilobyte per client.** 
>
> Even if this were a problem, it could be reduced by recording leasesat a larger granularity, so that each client holds few leases, at the expense of someincrease in contention. We show later how the per-client record can be eliminated for the most common class of widely-shared files.



### What's the difference between Long-term Leases and Short-term Lease?

> Longer-term leases are significantly more efficient both for the client and server on files that are accessed repeatedly and have relatively little write-sharing.



Then:

1. How do we issue the lease (elect the lease holder)?
2. How do we select the term?



## 3 Choosing the Lease Term

### Goal:Min(false_sharing, lease_extension_overhead) 

Assumption:

1. Rate of failure is low
2. On-demand extension rather than periodic extension
3. No queuing delay (footnote 3)
4. No retransmission delay




> This trade-off applies to minimizing both **server load and client response**. 
>
> **Lease spaceoverhead** is a less critical consideration and so is ignored as a Eactor.
>
> In addition, **the rate of failures is assumed to be low enough** to have no significant effect on the average response time, especially with short-term leases. 
>
> Finally, we consider here onIy **on-demand extension** of leases rather than **periodic extension** or other options such as noted in Section 4.





### 3.1 A Simple Analytic Model

**[Propagation Delay](https://en.wikipedia.org/wiki/Propagation_delay)**: time of the first bit to travel from the sender to the receiver.

**Processing Time**: time for router to process the packet header



#### Parameters

- $N$: # clients / caches
- $R$: read% for each client
- $W$: write% for each client
- $S$: # caches that share the file
- $m_{prop}$: propagation delay for message
- $m_{proc}$: processing delay. Time to process message on the router. This time is the **critical time** that does not include the time before message received or after message is sent (since the network flow will drive the message towards it).
- $\epsilon$: allowance of clock drift 
- $t_S$: lease term (at server)

Each client’s reads ($R$) and writes($W$) follow **Poisson distributions with rates $R$ and $W$**, respectively

There is **at most one lease per client** for the file.

#### Model Variables

##### Unicast

$m_{prop}+2m_{proc}$ : unicast send / reply

$2m_{prop}+4m_{proc}$ : unicast send + reply

##### Multicast

$2m_{prop}+(n+3)m_{proc}$ : multicast send + n reply 

[Explaination on piazza](https://piazza.com/class/jlo4totlie3ol?cid=40): Why $(n+3) m_{proc}$?

```
Following is my understanding of message processing sequence for multicast:
1. one message at the sender => 1
2. n receivers processing the msg in parallel => 1
3. n receivers processing reply in parallel => 1
4. sender receives n responses and have to process them sequentially one by one => n
Hence, (n+3) processing delay


```



#####  Effective Lease term

$t_C=max(0,t_S-(m_{prop} + 2 m_{proc})- \epsilon)$

##### Rate of Read

$R\cdot t_C​$: the cost of read with rate R within time $t_C​$. (By Poisson Distribution)

$1+R\cdot t_C​$: Amortized rate

#####  Rate of extension-related messages handled by the server
$$
\frac{2NR}{1+Rt_C} + \frac{2(m_{prop}+2m_{proc})}{1+RT_C}
$$

##### Write

$S-1$ : Actual approval needed for writers to hold the lease (when 1 of them is the least holder)

$t_a$ : approval time
$$
t_a = 2m_{prop} + (S+2) m_{proc}
$$
the load is at most $NSW$

#### Requirement

Message time within milisecond, lease term within seconds 

> Wethereforedonotconsider casesin which t, is a significant fraction of ts. 
>
> **The ex-ception is ts = 0**: it is important to recognize that a zero lease term is better than a very short lease term **becausea non-zero ts and zero tc means that writes are penalized but readsdo not benefit.**



##### Lease term is zero and no sharing: 

load and delay are limited to those due to extensions of lease

**Send and receive message**
$$
\frac{2NR}{1+Rt_C} + NSW
$$
**Average Delay**
$$
\frac{1}{R+W}(\frac{2R(m_{prop}+2m_{proc})}{1+Rt_C} + Wt_a)
$$




**Lease benefit factor**
$$
\alpha = \frac{2R}{SW}
$$
Then the condition 
$$
2NR > \frac{2NR}{1+Rt_C}+NSW \\\Rightarrow t_C > \frac{1}{R(\alpha - 1)}
$$
A sufficienlty long lease term will reduce server load whenever $\alpha > 1$ .




### 3.2 Expected Performance In V







### 3.3 Applicability to Future Distributed Systems







## 4 Options for Lease Management

**Choose by characteristics**

- Installed files: long lease with check
- Modification results to end of lease
- write-heavy tasks 
- long delay machine

## 5 Fault-Tolerance



> consistency is maintained in spite of messageloss (including partition), and client or serverfailures (assumingwrites arepersistentattheserver across a crash).

Leases depend on well-behaved clocks.



## 6 Related work



## 7 Conclusions



> Our simple analytical model estimates the server con-
> sistency load and consistency-induced delay to cache re-
> quests as a function of the lease term, the ratio of reads
> to writes, the degree of sharing and messagetimes



> **A key assumption is that clocks are reasonably accurate**, at least in terms of drift if not mutual synchronization, Wehavearguedthatsynchronizedphysical clocks areimportant in generalin asystemwherefiles are sharedin the manner supportedby leases.



### Limitation

1. **Simplified File Sharing: low degree of sharing.** Exception: 
   1. Parallel programming system
   2. Parallel transaction system
   3. Remote execution
2. **Approximate Analysis**, ignored
   1. Queing delay
3. **Not really a real-system tested method**: V system is well-tuned

### Other Application

1. large-scale shared memory multiprocessors

   > However, the benefitswill have to be evaluated relative to the costs of
   >
   > 1. **timers on memory and cache lines**, and 
   > 2. **the ability of the software to handle failures.**



## Acknowledgements



> **Acknowledgements**. These ideas have benefited from discussions with many members of the Distributed Systems Group, of whom JoePallas hasbeenespecially helpful. The comments of the SOS Preferees,and especially the more detailed reviews by Marvin Theimer and Doug Terry, have helped to improve the quality of this paper. 
>
> **The name “lease” was suggestedby Marry Katz.**