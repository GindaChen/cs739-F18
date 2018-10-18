# CS739 Week 7-2: elections

[Elections in a Distributed Computing System](http://vis.usal.es/rodrigo/documentos/papers/BullyAlgorithm.pdf)

1982, HECTOR GARCIA-MOLINA

https://www3.nd.edu/~dthain/courses/cse66771/summer2014/

# Summary

Election and reorganization is essential after leader crashes.

Two types of failure environment: what are the assertions, how election is defined, what is the election algorithm.



[TOC]



# Reminder

1. 



# Questions

1. 



# Detail

## 1. Introduction

**Strategy - Adapt to failure** using example of a simple distributed system: sensor

1. **Software supports failure**: Neighbor nodes should collect data for the area not covered by the failed node.
2. **Reorganize system**: remaining node detect failure, switch to different algorithm or abort mission.
   - **Reorganize**: choose suitable algorithm, evaluate situation, maybe halt the system for a while.
   - **Coordinator**: the *leader* who recover the system
   - **Election**

[1] is better when failure is often, or the job has to be done continuously.

[2] is a better solution when system are able to halt, and when system is bound to fail but not often. Also it reduces the complexity of software design.

**Election**

- **Election in failure**
- Election during initialization, add/remove node

**Idea**: a synchronization of parallel processes across network

1. Nodes <u>talk</u> to each other and <u>choose</u> a coordinator
2. Coordinator <u>reorganize</u> the system
3. System <u>resume</u> task

**Essential Problem**: a synchronization of parallel processes across network 

1. Mutual Exclusion
2. Failure
   1. **Cohorts (constituents) failed**: coordinator don't know whom is being coordinated.
   2. **Coordinator failed during / after election**
   3. Sometimes more than one coordinator is needed.

**Core Questions**

1. What it means to become a coordinator?



## 2. Assumptions

### 7 + 2 Assumptions

<u>Rare and out-of-protection failure is not considered.</u>

**Assumption 1**: All nodes cooperate and use the same election algorithm

**Assumption 2**: Election algorithm are supported by software facilities. Software runs well and no bug, and should include:

- a Local OS
- Message handler

**Assumption 3**: Message send-recv obey causality. == Message is sent at earlier than than when message is received 

<u>Failure that can be avoid or is accpetable is not considered</u> 

**Assumption 4**: **Safe cell** store information permanently. **Info Update** obeys atomicity. **State Vector** is kept in safe cell.

**Assumption 5**: When node fails, it halt all processes. After recovery, processes are reset to some *fixed state* and execution resume. Failure cannot deviate a node from its algorithm and behave in an unpredictable way. Any hardware failure that does not stop a node entirely must be detected and must be converted into a "full fledged" crash before the node is able to affect any other system component ==(Didn't really understood)== 

**Assumption 6:** If a packet is received, the correct receipien will receive it. It doesnot guarantee that the packet will not lost, but it guarantees that as long as the packet is received, the right guy gets it.

**Assumption 7**: Message arrives in order from node `i `to` j`. A later message sent from node `i` will not arrive earlier than a previous packet. If i send `[a b c]` to `j`, then eventually `j` can not receive `[a c b]` or `[c a b]`. It can only receive the original order.

<u>Election is similar to what we understood (will be relaxed in Section 4)</u>. 8 and 9 are assumed in Ethernet (or high connectivity network) with accurate timer added.

**Assumption 8**: Communication subsystem does not fail. Specifically, a message is guaranteed to be delivered within a bounded time $T$ . If after $T$ that the message from `i` is not received by `j`, then j knows that `i` is dead.

**Assumption 9**: A node never pause, and respond to incoming message without delay. Not responding "fast enough" is also a failure.If `i` doesnot receive message from `j` within $T$ , then `j` must be failed, and we assume the recovery of `j` already started. 



> *Assumption 2*: Election algorithm at each node must make use of certain **software facilities**. These facilities include **a local operating system and a message handler**. Software runs properly, and no software bug (failure) in these facilities exist.
>
> *Assumption 3*: Communication subsystem will not spontaneously generate messages.



### Notation

`S(i)`:  **State vector** of node `i`. `S(*)` is a global view of every state vector in the cluster. `S(i)` means a particular vector in node `i` . A collection of safe storage cells which contains the crucial data for election and application algorithms.

`S(i).s`: Status of Node `i`. `{"Down", "Election", "Reorganization", "Normal"}`

- Reorganization: know the coordinator, but not the task. ==then when will they know the task? Normal?==

`S(i).c`: Coordinator according to node `i`. ==Why according to node i?==

`S(i).d`: Task (definition of the task being performed). = application + participating nodes + other state info. ==What kind of task?==

`S(i).g`: (section 4) Group number.



## 3. Elections with no communication failures and no node pauses

**Good election protocol**

**Assertion 1: At Reorganization, nodes agree on the coordinator, and (except coodinator) nodes perform the same tasks**. 

Coordinator knows that:

- Once `Node(i)` know its a coordinator, it know that any other nodes will recognize it as the coordinator. 
- No other nodes will become coordinator during the reorganization.

```python
# 1.a Once Node(i) know its a coordinator, it know that any other nodes will recognize it as the coordinator. 
if S(i).s in [Reorganization, Normal] and 
   S(j).s in [Reorganization, Normal]: 
    assert S(i).c == S(j).c
# 1.b ?????
if S(i).s == Normal and S(j).s == Normal:
    assert S(i).d == S(j).d
```

> ==Why is it not the whole set of nodes ....???== The fact that S(i).d must equal `S(j) .d` under normal operation **doesnot mean that all nodes are assigned exactly the same set off functions.**

Problem:

- Coordinator can not know for sure which nodes are down.
  - Solution: Every action must use **2PC** to ensure the action is performed by all nodes.

**Assertion 2: Eventually a leader will be elected when no failure occurs.**  If no failure occur during the election, the election protocol will eventually transform a system in any state to a state where

(a) node `i` knows that it is a leader
$$
\exist i : S(i).s = Normal \and S(i).c = i
$$
and (b) all other non-failed nodes recognize `i` as the leader
$$
\forall j \neq i, j \text{ not failed}:S(j).s = Normal \and S(j).c = i
$$


**Mutual Exclusion helps, but**

1. Election is not fair. We need not guarantee fairness. (in mutual exclusion problem, we have to guarantee that every process eventually enter the critical region)
2. Coordinator fail must be considered. (most of the mutual exclusion algorithm assume process will not fail).
3. Coordinator will inform all active nodes of its identity. (processes in mutual exclusion problem need not know each others)



### Bully Algorithm

**Idea: Node with larger ID force node with smaller ID to accept it as coordinator.**

**Theorem 1: Bully algorithm satisfies Assertion 1-2 as long as Assumption 1-9 holds.** 

- *Assumption 8*: No communication failure is assumed.
- *Assumption 9*: Node will not pause.

**Explaination of the algorithm**

- Assume we have n nodes marked `1-n`
- Assume a failed node will not come up. (later relax)
- If `i` failes at any point of election/ after election, it lose the job.

<u>First Part: Become a Coordinator</u>

1. `i` want to be the coordinator
2. **[Try higher rank]** `i` contact all nodes higher than itself `i+1, i+2, ..., n` and see if they respond. 
  3. **[Retrieve or restart]** If one of them respond within a *the timeout time* $T$ , `i` will give up leadership and wait until one of them to become the coordinator. 
           1. if within time $T$ `i` receives message that another node claims a leadership, protocol <u>success</u>; 
           2. otherwise, `i` will <u>restart</u> the election protocol. 
  4. **[Bully leader]** If all not respond within timeout time $T$, `i` becomes the coordinator. Since no higher rank node will respond (or recover, at this situation), and no lower rank node can become the leader (`i` is the highest rank and supress any node to become the leader given the time $T$), it is the bullier leader.
       - **[Bullier comes back]** *[Recover `HALT` from higher rank]* If a higher rank node recovers, then it will attempt to become the coordinator by doing the protocol. At step [5] it will `HALT` anyother lower rank node and reorganize the party. Hence it gracefully bullies node `i` and become the new leader. What bloody reality!

<u>Second Part: Propaganda</u>

5. **[Election]** `i` send `HALT` to lower rank nodes and make sure they are in `S(k).s = Election` state. (without violating assertion 1)
6. **[Reorganize]** `i` send `I am elected` to lower rank nodes. Then lower rank node `k` will     ==But how can you send this message when you are not a leader (permission) -- security ensure ?==
   1. Check if the `HALT` and the `I am elected ` message is from the same node `i`.
   2. Set `S(k).s = Reorganization`
   3. Set `S(k).c = i`: recognize `i` as the coordinator.
7. `i` then set `S(i).c = i`, `S(i).s = Reorganization`
8. **[Task start/resume]** `i` distributes the new algorithm `d` to all nodes and 
   1. Node `k` receives the algorithm and set `S(k).d = d`
   2. Node `k` change its state `S(k).c = Normal` and do its job.



*($T$ is the time mentioned in Assumption 8-9, where a communication is fast within time T and a node will respond quickly. So if nodes don't respond withinn in $T$, they are failed, and assumed to be remain its Down status.)*



## 4. Elections with communication failures and node pauses

Only assumption 1-7 is needed to be held in this section.

**Failure Types without assumption 8 and 9**

- No guarantee for a unique coordinator.
- No guarantee for a group of member to coordinate (or *migration*, after the fail the node listens to another coordinator).

**Solution: Group Number**. Define `S(i).g`: Group number

- Message exchanged contains group number. Node *can* ignore message from foreign group. *(but not all should be ignored)*
- Group number is unique, and node remember the number.

> Any group that has somehow gotten together to elect a coordinator and works on a common task or goal is defined with a unique group number.



**Assertion 3: All nodes in the same group have the same coordinator**. At any instance for any node `i` and `j` in the smae group, the following must hold:

```python
for each i,j:
    # 1.a All nodes in the same group have the same coordinator
    if S(i).s in [Reorganization, Normal] and 
       S(j).s in [Reorganization, Normal] and
       S(i).g == S(j).g: # i and j are in the same group
        assert S(i).c == S(j).c
    # 1.b and every normal node in the group performs the same task
    if S(i).s == Normal and S(j).s == Normal and 
       S(i).g == S(j).g: 
        assert S(i).d == S(j).d
```

Assertion 1 can be obtained by assertion 3. ==how==

**Assertion 4: All two-way communicative nodes in a group fits the model in Section 3**. 

$R$ : A subset of nodes within a group that has two-way communication. 

(a) node `i` knows that it is a leader
$$
\exist i \in R : S(i).s = Normal \and S(i).c = i
$$
and (b) all other non-failed nodes in $R$ recognize `i` as the leader
$$
\forall j \in R - \{i\}, j \text{ not failed}:S(j).s = Normal \and S(j).c = i \and S(j).g = S(i).g
$$


### Invitation Election Algorithm

**Theorem 3: Invitation Algorithm satisfies Assertion 3 and 4 if Assumption 1-7 holds**

**Idea: (Merge group) Invite other nodes to join the coordinator's group**. A node can accpet or decline the invitation; when it accpets, it alters its state to join the new group

- A node has 
  - `nid`: a unique id  (node id) 
  - `cnt`: a counter for the number of new groups generated by the node (kept in safe storage)
  - `gid() := return "${cnt++}_${nid}"`: a generator (also a storage) of the unique group id (like a simplified timestamp)
- **Idea1:** Recovering nodes form a new single node group
- **Idea2:** Coordinator periodically try to merge groups for a larger one.



<u>Algorithm</u>

1. Receiving node `k` form a new group with itself the coordinator and only member
   - A group will ensure that members in it can communicate without a coordinator. 
   - e.g.: if a group of 6 nodes is formed with node 6 as its coordinator, and node 6 failed, then the rest of the members will each form a new group.
2. Coordinator (or each group) periodically looks around the system and see if there are <u>other groups</u> which might want to join.
   - Lower priority nodes have longer sending cycle (to avoid all nodes send invitation at once).
   - Different groups will ultimately tend to merge. e.g. if group `a`  and group `b` both send invitation to `1,2,3,4`, and `1,2` accept `a` and into group `a` , `3,4` into `b`, then sometimes later, `a` anb `b` will send message to each other to invite a group merge.



<u>Comments</u>

- **Optimizations**: Coordinator could decline invitation if it does not trust the new coordinator or if it simply "not interested" in forming a new group. Etc.
- **Problem 1 Not-allowed Simultaneouls operatoin **: Certain operations should not be performed simultaneously by isolated groups of nodes. ==how an isolated group is formed?==
  - e.g.: Data update. Data will diverge in nodes that are isolated.
  - **Solution**: Require that if a group is going to perform the restricted operation it must have as members a majority of the nodes in the entire system (where the total number of nodes is known and constant). *This guarantees that at most one group performs the restricted operation.*
- **Problem 2 Restricted operation**: If some operation should be restricted, a newly elected coordinator should cound the members of the group and decide if the operation is allowed. If allowed, then distribute the algorithm `S(i).d`.
  - A group of non-functionable nodes are allowed.
  - Restricted operation must be performed with 2PC to ensure group still contains majority of nodes when the operation is actually performed.
- **Problem 3 History track**: 
  - History record for a group `gid`: `{gid: (cid, [n1id, n2id, ...]) }` group number, id of coordinator, all member nodes
  - History graph can be compiled by coordinator of the group and distributed to new group for safe keeping (in `S(i).d`).
  - Useful to clean up pending or conflicting work at reorganization. (e.g. Detect conflicting modifications to files)



## Appendix 1: Bully Algorithm

```Fortran
CALL proc (node: i, params: parameters), ONTIMEOUT (int: t) : stmt
```





## Appendix 2: Invitation Election Algorithm