# CS739 - Week3-2 Class Note (9/21, F)

# Digest of the class

[TOC]

We talked about some questions about the two papers:

1. Time, Clocks, and the Ordering of Events in a Distributed System (Leslie Lamport, 1978)
2. Virtual Time and Global States of Distributed Systems (Friedemann Mattern, Germany, 1988)



# Discussion Questions

1. Define "happens before"; give 2 events in a distributed system does one happen before the other?
2. What does it mean for two events to be "concurrent"?
3. What are Lamport's clocks; how do they work on message send and receive? What is the clock condition?
4. How can a total order be established among clocks? Why might this be useful?
5. Describe the mutual exclusion algorithm in Lamport paper. How does it work? Under what assumptions?
6. Describe vector clocks and how they work; what additional properties do they give you beyond Lamport clocks?
7. What are the downsides of using vector clocks instead of Lamport clocks?
8. What is a consistent cut, and how might it be useful?



# Class Start

**Time is difficult to think of in concurrent world.**

**Author note**: a short note about [Lesile Lamport](https://amturing.acm.org/award_winners/lamport_1205376.cfm), his ACM page.

**Lamport**: https://amturing.acm.org/award_winners/lamport_1205376.cfm



### Answering the Questions

(Most of the answers are in paper, if not all of them)

#### Question 1: Define "happens before". Give 2 events in a distributed system does one happen before the other.

Happen before:

1. In a single process setting, events follow the ordering defined by the statements or instructions defining the process.
2. When a message is sent between two processes, the “send” event happens before the “receive” event.
3. For two events across multiple processes, the ordering is defined by transitive application of rules (1) and (2).



#### Question2: What does it mean for two events to be "concurrent"?

If an event $a$ cannot be <u>causally</u> related to another event $b$ with respect to the “happens before” relation they are said to concurrent.

causality (因果): An event $A$  is said to be happend before event $B$.  



#### Question3: What are Lamport's clocks? How do they work on message send and receive? What is the clock condition?

**Lamport’s clocks** work by **assigning numeric values to events** to define an ordering between them. 

1. Within a process, values are assigned from an auto-incrementing counter.
2. When a process receives a message from another process (and the message carries a timestamp $t$), then the process updates its clock to the maximum between its own clock and $t$ (which is when the message actually originated in the sending process) and adds 1 to it.

<u>Clock Condition</u>: When an event $a$ happens before an event $b$, then $C(a) < C(b)$, $C$ is the clock function.



#### Question4: How can a total order be established among clocks? Why might this be useful?

(p561. left, top) 

Lamport clocks can be used to define a partial ordering of events. When two events are concurrent, ties can be broken with rules (say process_id ordering etc) to define a total ordering of events. This finds application in many problems like mutual exclusion etc.



#### Question5: Describe the mutual exclusion algorithm in Lamport paper. How does it work? Under what assumptions?

**Idea**: As long as we maintain a queue

**Algorithm**:

(p.561. right) Each process maintains its own request queue which is never seen by any other process. We assume that the request queues initially contain the single message T0:P0 requests resource, where P0 is the process initially granted the resource and T0 is less than the initial value of any clock. 

1. To request the resource, process Pi sends the message Tm:Pi requests resource to every other process, and puts that message on its request queue, where Tm is the timestamp of the message. 

2. When process Pj receives the message Tm:Pi requests resource, it places it on its request queue and sends a (timestamped) acknowledgment message to Pj.

3. To release the resource, process Pi removes any Tm:Pi requests resource message from its request queue and sends a (timestamped) Pi releases resource message to every other process. 

4. When process Pi receives a Pi releases resource message, it removes any Tm:Pi requests resource message from its request queue.

5. Process Pi is granted the resource when the following two conditions are satisfied.

6. 1. There is a Tm:Pi requests resource message in its request queue which is ordered before any other request in its queue by the relation.
   2. Pi has received a message from every other process timestamped later than Tm.

**Assumptions**:

1. Guaranteed delivery of messages.
2. In order delivery of messages.
3. Ability to communicate between any two processes.



#### Question 6: Describe vector clocks and how they work. What additional properties do they give you beyond Lamport clocks?

Vector clocks store one element per process approximating the clock of that process. 

By seeing the vectors of two events, it can be inferred if one happened before other or they are concurrent.



#### Questions 7 What are the downsides of using vector clocks instead of Lamport clocks?

**Vector clocks assume the knowledge of number and membership of processes.** Therefore it does not consider:

- **Scalability**. In reality, processes come and go, machines up and down. It is pretty hard to let every node keep track of everyone’s status in the network.
- **Trust boundary**. Processes have to get full knowledge of each other’s time. 
- **Recovery**. 

Classical problems like
- [Bayzentine failure](https://en.wikipedia.org/wiki/Byzantine_fault_tolerance): when can you trust the system

One solution: [Google Spanner](https://cloud.google.com/spanner/) . The idea is simple: <u>It’s ok as long as the clocks doesnot drift too much</u>. (e.g. liek a red-black tree, as long as the balance factor maintain a reasonable level)



#### Question 8: What is a consistent cut, and how might it be useful?

**Cut**: Dividing the space of time such that all events are partitioned into `PAST` and `FUTURE` set gives rise to consistent cuts. 

**Consistent Cut** (this is a very confusing concept): a cut where cut events are no causality relation. 

In other words, if we take a cut where every cut event is **concurrent** to each other, then it is a consistent cut.

```
Quick Question:
Q: If the process never communicate, is it hard to make a consistent cut?

A: Very simple --- any cut is a consistent cut. 
Since there is **no communication between the process**, 
these processes are **concurrent**. 
```

**Usefulness**:

- **Snapshot**: take a snapshot of every process in their consistent event. They form a great snapshot to trace current situation.
- **Distributed Debugging**



# Side Notes

1. How Google Spanner solve the problem

> About Google Timer and True Time API
>
> http://static.googleusercontent.com/media/research.google.com/en/us/archive/spanner-osdi2012.pdf

> https://www.wired.com/2012/11/google-spanner-time/
>
> Rather than rely on outside clocks, Google equips its Spannerized data centers with its own atomic clocks and GPS (global positioning system) receivers, not unlike the one in your iPhone. Tapping into a network of satellites orbiting the Earth, a GPS receiver can pinpoint your location, but it can also tell time.
>
> These time-keeping devices connect to a certain number of master servers, and the master servers shuttle time readings to other machines running across the Google network. Basically, each machine on the network runs a daemon – a background software process – that is constantly checking with masters in the same data center and in other Google data centers, trying to reach a consensus on what time it is. In this way, machines across the Google network can come pretty close to running a common clock.