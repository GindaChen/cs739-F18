# Midterm

Cheat sheet (2 side), [Old Exam](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/OldExams/), and good luck?

[TOC]



## Articles menu

- [x] :fire: Week1.1 - Building Large-Scale Internet Services - Jeff Dean
- [ ] :fire: Week1.1 - On Designing and Deploying Internet-Scale Services - James Hamilton
- [x] :fire: Week2.1 - Disk failures in the real world: What does an MTTF of 1,000,000 hours mean to you? - Bianca Schroeder, Garth A. Gibson
- [x] :fire: Week2.1 - Why Do Computers Stop and What Can Be Done About It? - Jim Gray
- [ ] 🔥 Week2.2 - Datacenter RPCs can be General and Fast - Anuj Kalia, Michael Kaminsky, David G. Anderson
- [ ] 🔥 Week2.2 - U-Net: A User-Level Network Interface for Parallel and Distributed Computing - Thorsten von Eicken, Anindya Basu, Vineet Buch, and Werner Vogels
- [x] Week3.1 - Time, Clocks, and the Ordering of Events in a Distributed System - Leslie Lamport
- [x] Week3.1 - Virtual Time and Global States of Distributed Systems 􏰁- Friedemann Mattern
- [ ] Week4.1 - Flash: An Efficient and Portable Web Server - Vivek S. Pai, Peter Druschel, and Willy Zwaenepoel
- [ ] Week4.1 - SEDA - SEDA: An Architecture for Well-Conditioned, Scalable Internet Services - Matt Welsh, David Culler, and Eric Brewer
- [ ] Week4.2 - ALICE: All File Systems Are Not Created Equal: On the Complexity of Crafting Crash-Consistent Applications 
- [ ] Week5.1 - WiscKey: Separating Keys from Values in SSD-Conscious Storage
- [ ] Week5.2 - Leases: An Efficient Fault-Tolerant Machanism for Distributed File Cache Consistency - Cray G. Gray and David R. Cheriton
- [x] Week5.2 - The Sun Network Filesystem: Design, Implementation and Experience - Russel Sandberg
- [x] Week6.1 - HANFS - Anupam Bhide, Elmootazbellah N. Elnozahy, Stephen P. Morgan
- [x] Week6.2 - Guest Lecture: Machine learning in the 
- [x] Week7.1 - 2PC - Butler Lampson, David Lomet
- [ ] Week7.1 - Scalable, Distributed Data Structures for Internet Service Construction - Steven D. Gribble, Eric A. Brewer, Joseph M. Hellerstein, and David Culler
- [ ] Week7.2 - Elections in a Distributed Computing System - Hector Garcia-Molina
- [ ] Week7.2 - Probabilistically Bounded Staleness for Practical Partial Quorums



## Question Lists

- Week1.1 - Building Large-Scale Internet Services - Jeff Dean
  - General thing on distributed system
- Week1.1 - On Designing and Deploying Internet-Scale Services - James Hamilton
  - Not a hard paper
- Week2.1 - Disk failures in the real world: What does an MTTF of 1,000,000 hours mean to you? - Bianca Schroeder, Garth A. Gibson
  - [ ] What is Figure 1 talking about ? (The legend of the figure is not associate with what he's talking about...)
- Week2.1 - Why Do Computers Stop and What Can Be Done About It? - Jim Gray
- Week2.2 - Datacenter RPCs can be General and Fast - Anuj Kalia, Michael Kaminsky, David G. Anderson
  - Basic Introduction (limited time)
- Week2.2 - U-Net: A User-Level Network Interface for Parallel and Distributed Computing - Thorsten von Eicken, Anindya Basu, Vineet Buch, and Werner Vogels
- Week3.1 - Time, Clocks, and the Ordering of Events in a Distributed System - Leslie Lamport
- Week3.1 - Virtual Time and Global States of Distributed Systems 􏰁- Friedemann Mattern
- Week4.1 - Flash: An Efficient and Portable Web Server - Vivek S. Pai, Peter Druschel, and Willy Zwaenepoel
- Week4.1 - SEDA - SEDA: An Architecture for Well-Conditioned, Scalable Internet Services - Matt Welsh, David Culler, and Eric Brewer
- :fire:Week4.2 - ALICE: All File Systems Are Not Created Equal: On the Complexity of Crafting Crash-Consistent Applications 
- :fire:Week5.1 - WiscKey: Separating Keys from Values in SSD-Conscious Storage
- Week5.2 - Leases: An Efficient Fault-Tolerant Machanism for Distributed File Cache Consistency - Cray G. Gray and David R. Cheriton
- Week5.2 - The Sun Network Filesystem: Design, Implementation and Experience - Russel Sandberg
- :fire:Week6.1 - HANFS - Anupam Bhide, Elmootazbellah N. Elnozahy, Stephen P. Morgan
- Week6.2 - Guest Lecture: Machine learning in the 
- Week7.1 - 2PC - Butler Lampson, David Lomet
  - Introduction of the Concepts 
- Week7.1 - Scalable, Distributed Data Structures for Internet Service Construction - Steven D. Gribble, Eric A. Brewer, Joseph M. Hellerstein, and David Culler
- Week7.2 - Elections in a Distributed Computing System - Hector Garcia-Molina
- Week7.2 - Probabilistically Bounded Staleness for Practical Partial Quorums
  - Introductions of the quorum (maybe not detail)



## What to review

### Overall

- Write a summary of the article and list of keywords
- Explain ( possibly ) every graph / table in the article

### In Particular

- Back of the envelope calculation [Fall17, #2]

- LSM algorithm [Fall 17, #2]
  - how much write amplification is tolerable in LSMs on hard drives (i.e., how many sequential writes can one do to replace some random ones?) 
- RCP [Fall17, #3]



## What will be on the exam

