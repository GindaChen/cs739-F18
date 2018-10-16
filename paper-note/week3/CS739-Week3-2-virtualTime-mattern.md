`@Author: Junda Chen`

# CS739 - Week3-2 Note: Virtual

Virtual Time and Global States of Distributed Systems

Friedemann Mattern, Germany, 1988

---

# Summary

[Lamport] Logical clock + Time update is good. But linear relation of time mapping is not good.

To preserve causality, lattice structure and vector time is a must.

Vector time structure allows different process events with causality to propagate their time and record the time in local process structure. 

Since lattice of consistent cut is isomorphic to lattice of possible time vectors, time vectors can identify a consistent cuts, and a consistent cut can also be be identified by its timestamp using $t_X$ .



[TOC]

# Reminder

1. **Theorem 5, 9** is The most important theorem.
2. **Cut** and **Consistent Cuts** are the most confusing definitions.
3. *Minkowski Time-space invariant property* is of no importance here.
4. This paper is long. I spent an easy 45 mins reading Lamport, and spent nearly 2 hours reading this one. This one is interesting, but it's easy that you get lost in some of the concepts (if you don't know what is cut and lattice).



# Detail

## 1. Introduction

### Core problems

- Distributed agreement
- Distributed termination detection
- Symmetry breaking / Election problem



### Feasible solution

#### 1. Synchronizer: simulate a synchronous distributed system

Clock pulse: overhead is high



#### 2. Simulate global time (common clock) 

**--> Lamport 1978 -> Morgan[11]; Neiger and Toueg[12]**

Does not need additional information

Virtual time implemented by logical clock



#### 3. Simulate global state (common memory)

**Snapshot algorithm [3, Chandy and Lamport]** can solve deadlock detection, termination detection ...

**Concurrent common knowledge [14, Panangaden, Taylor]**

-> Idea: Compute the *best appoximation* of the global state



### Problems

[Lamport 1978] maps partial information to a linear structure.

<u>=> Lose information: distributed debugging, mutual exclusion etc.</u>

> Events that happen simultaneously have different time stamps as if they happen in different order. So **can we preserve the order AND the information of whether they're mutually excluding each other? ** 



### **All events which are not causally related are simultaneous.** Thus causality become isomorphic.

Idealized time (supremum of all local clock vectors: $t_i = \sup\{\vec{v_i}\}$)





## 2. Event structures

### @Category: Events

- Internal event
- Send event
- Receive event

**Message-driven Distrubuted System**

[Actor Model](https://zh.wikipedia.org/wiki/參與者模式): 一切皆是参与者 (everything is actor [== everything is object] )

Originally in Carl Hewitt; Peter Bishop and Richard Steiger. [A Universal Modular Actor Formalism for Artificial Intelligence](https://web.archive.org/web/20091229084457/http://dli.iiit.ac.in/ijcai/IJCAI-73/PDF/027B.pdf) (PDF). IJCAI. 1973.



### Events are related

1. Events are totally ordered in process
2. Each receive event has a corresponding send event

 **=> Causality relation: Future cannot influence the past**



### @Define: Event Structure $(E, <)$: 

E := set of events; 

< := causality relation (irreflexive partial order) holds if any:

1. e and e' in same process
2. e is send event, e' is the corresponding receive event
3. $\exist e''$ st $e < e'' < e'$



**Timing digram**: a poset-digram

**Equivalent** causality relation:**Equivalence transfomration on time digrams** leaves causality relation invariant



## 3. Consistent Cuts

### @Define: Consistent Cut = a set of concurrent events

<u>Consistent cut = a set of concurrent events</u> *(this is a very confusing definition. Make sure you know what is cut and what is consistent cut)*.

All local actions triggered by the message are not guaranteed to perform in the same future "physical" time.



**Cut, cut line**: a series of cut events that seperate the whole graphs into two parts: `PAST` and `FUTURE`.



**Notation**

$c_i$: cut event

$E$: event set

$P_i$: $E' = E + \{c_i|i=1,2,...,n\}$

$<_l$: $<_{local}$ meaning *local event order* 



### **Definition 1: cut (in local event order)**

Fix an arbitrary event in $e\in C$, then $e' \in C \ \forall \ e' <_l e$

Mind that we are using $<_l$ local relation. The *consistent cut* will use the global relation $<$.

**??  => a set of concurrent events is a cut**



### **Definition 2: Define cut's *later than* order**

 $C_1$ is later than $C_2$ iff $C_1 \supseteq C_2$

- Transitive
- Reflexive



### **Theorem 1: Lattice**

inf $= C_1 \cap C_2$

sup $= C_1 \cup C_2$



### **Definition 3: Consistent cut: a cut that obeys causality**

**i.e., every received message was sent (in this set)** 

$e\in C$ =>$e' \in C \ \forall \ e' < e$

left-closed under causality relation $<$



### **Theorem 2: Sublattice relation of consistent cut**

**? Consistent cuts are closed under $\cup$ and $\cap$**

**.  => There will be a lattice later than us, and a lattice former than us.**



### **Theorem 3: No <u>cut event</u> within a consistent cut is causally related**



### **Theorem 4: Consistent cut events are concurrent in real-time digram**





## 4. Global States of consistent cuts

**Correct**: a global state computed along a consistent cut.

**Snapshot is consistent** when taken at same (real time).

**Global State**:= all local states at a cut event + all messages sent but not received.

**Snapshot problem**: yield only consistent cuts + collect local state info + capture send-event

(Lamport 1978: FIFO message transmission)



## 5. The concept of Time

How do we define what is

- **Real time**
- **Causality-represented time**



**Notation**

$t(\cdot)$ : timestamp



### Distributed algorithm factorization 

Morgan[11]:

1. Global time available to all process
2. virtual time clock sync to approximate global time

### Questions arised to define virtual time

1. What is virtual time? What is approx of real time?
2. Do vitural time exist when real time is assumed?
3. What is essential structure of real time?

### Time: instances + temporal precedence order



**Property of the Time:**

1. Transitivity
2. Irreflexivity 
3. Linearity
4. Eternity (well...)
5. Density (but we have "Discreteness" in CS...)



**CURX: a logical clock + sync mechanism - real time/physical clock**

Lamport

​	=> Good: sync + logical

​	=>  Bad: ignore causality independence (force causality upon events).



## 6. Virtual Time

vitural time = *succession of events*

**logical clock**: $C(e): E \rightarrow T$

1. e < e' :=> C(e) < C(e')
2. e-send < e-recv :=> C(e-send) < C(e-recv)

### Local clock protocol

1. At internal & send events: clock ticks $C_i := C_i + d$
2. Message has timestamp
3. At Recv event: clock ticks $C_i := max(C_i, t) + d$ 

### **Mutually independent** $e || e'$

Looking at timestamp of events is not possible to assert precedence.



**Order homomorphism preserves $<$**



## 7. Vector Time

### **Global time:** 

an idealized external observer store things in **process-time vector**, which comprises a sequence of global time vectors.



**Receiver's approximation**: 

$C_i := \sup{(\textbf{C_i}, \textbf{t})} = sup(C_i[k], t[k])$



### **Theorem 5: local time is always the most up-to-date**: $C_i[i] \geq C_j[j]$



**Definition 4: $<, \geq, ||$**



**Definition 5: Global time of cut $t_X = sup(C(c_1), ..., C(c_n))$**



### **Theorem 6: $X$ is consistent iff $t_X = (C(c_1)[1] ... C(c_n)[n])$**



## 8. The Structure of Vector Time

**Theorem 7: $(\textbf{N}, \leq)$ is a lattice**



**Theorem 8: Possible time vectors of E is a sublattice of $(N, \leq)$**





### Main Theorem

#### **Theorem 9: lattice of consistent cut ~ lattice of possible time vectors**



#### **Theorem 10: Timestamps can identify consistent cut** 

$\forall e, e' \in E: e < e' \Leftrightarrow C(e) < C(e');\  e || e' \Leftrightarrow C(e) || C(e')$



#### **Theorem 11: Method of Causality check**

e happends in i, then $e < e' \Leftrightarrow C(e)[i] \leq C(e')[i]$. Meaning, a path will propagate the time in process i to the successive event's clock. Also, if e' knows e about its time, there should be a path of propagation.



**interleaving**: a linear sequence of events which consists with the causality relation. The set of paths get a **possible development of the global virtural time** => Time cone



## 9. Minkowski Space Time

Causality preserving transformations (rubber band transformation)

= Lorenz Transformation, leaving the light cone invariant.



2D space-time forms a lattice: the supremum and the infimum being the causality intersection



But 3D space-time does not form a lattice!!



## 10. Application of Vector Time

Trace checkers

Potential concurrency: degree of parallelism

Detect mutual inconsistencies of multiple file copies [15]: **version vector, version conflict**







## 11. Computing Global States on Systems without FIFO Channels



Idea: It's never too late to take a snapshot of "now" as long as you choose a future time and force everyone agree on that.

When time comes, do the **time-leap**.

==**@Question**: a counter modulo 2 is sufficient?? In the paper it mentioned that a counter modulo 2 is sufficient to store the time. Why?==



This assumption is wierd:

> **Notice that the algorithm is correct even if messages**
> **are not received in the order sent􏰚.**
>
> It is easy to see that
> the cut induced by the snapshot algorithm 􏰗which con􏰙
> sists of all 􏰍white events􏰕􏰘 is consistent􏰚 There do es
> not exist a message sent by a red pro cess which is re􏰙
> ceived by a white pro cess b ecause such a message would
> color the receiving pro cess red immediately b efore the
> message is accepted􏰚



---

# A little bit English

a priori: 演繹（えんえき）的な[に]; 先天[先験]的な[に] ([↔ a posteriori](x-dictionary:r:WDEJ_a0015990:com.apple.dictionary.ja-en.WISDOM)).





# Questions



==**@Question 1**: a counter modulo 2 is sufficient?? In the paper it mentioned that a counter modulo 2 is sufficient to store the time. Why?==

==**@Question 2: **Is version control and distributed system version vector of the same origin ?==