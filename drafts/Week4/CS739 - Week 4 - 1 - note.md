# CS739 - Week 4 - 1

## How to learn system behavior - To Break The System



## How to construct a server?

### Concurrency Architecture

#### Process - Architecture

#### Thread - Architecture

- User-level thread: looks like a process to the OS
- Kernel-level thread: OS schedules each thread

#### Thread Pool Model `(CS537, P3)`

- Prevent the overload: when latency and bandwidth become significantly bad in the world of too many requests => Bounded requests to handle.
- Control scheduling:
  - Big customer first
  - Shortest job first: static pages
  - ...

#### Events-based programming

Always write non-blocking code in the main space.

- We really don't controll the whole proecss:
  - Page fault
- Harder to program: continuation accross events
- Easier in **Single CPU model** for concurrency: when I'm running, no one is running with me (but in reality we have multiple cpus)
- Multi-cpu: still sync

- Full control over scheduling





### Server Implementation

- 1.Process per request
  - Positive: isolation, no sync / concurrency bug, straight-line code  (linear logic)
  - Problem: 
    - 1.Overhead - context switch, memory, address space ...
    - 2.Inter-process communication (hard to share -- hard to realize), 
    - 3.**Control** (priority, scheduling) `read Yang's paper about the controlling, whic h is wrong`. *[a really buggy point for OS dev]*


- 2.Thread (kernel thread) per request
  - Positive: isolation, straight-line code  (linear logic), save cache (address space)
  - Problem:
    - 1.sync / concurrency bug.
    - 2.overhead (less cache, but still)
    - 3.Control
- 3.Thread pool
  - Positive:
    - 1.Avoid overload
    - 2.Give some scheduling: priority class
    - 3.Easy to implement...
  - Open quesions:
    - 1.Thread pool size? 
      - CPU intensive: match core
      - I/O intensive: more to prevent the sleep
    - 2.Scale?
- 4.Events-based programming
  - Positive:
    - 1.Full control over scheduling
    - 2.Non-block in the events
  - Negative:
    - Hard to implement, hard to really get these events



## Flash & SEDA

**Flash**: let's choose a better alternative.

**SEDA**: maybe we can formalize it a little bit more into **stages**. 

<u>Becareful of the bias when reading the comparison.</u>



## A good Question

Can you really implement your schedulling policy in SEDA?

Multiple different services are sharing the storages/resources. 

**Isolation** is very important (at a performace standpoint, scheduling).

**Scheduling in shared storage matters.** Then how do we implement a policy for that?

- **Fair-share**: how to do the controller to do the fair share?



What we've done: `schedulable`. Analysis of existing storage servers and how to fix the scheduling.