# Notes from CS739

Notes from [Remzi's CS739 F18](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/) paper / class. 



## Published Notes



### Week3.2 (9/21,F) - Clock

[Week3.2: Lamport 1978](paper-note/CS739-Week3-1-clock-Lamport.md): You can establish a total order by forcing each event to have a time on linear relation.

[Week3.2: Mattern 1988](paper-note/CS739-Week3-1-virtualTime-mattern.md): Linear relation is not enough. To preserve causality, why not use vectors to preserve the relation?

[Week3.2 Class Note](class-note/week3/CS739-Week3-09-21-note.md): The sum of the above



## Drafts

Drafts, as you can see, is put into the `draft` folder. They are unfinished, sloppy jot-downs that no one will like them (except perhaps me : ).



## TODOs

Notes Update

- [ ] Upload Week 1 Notes
- [ ] Upload Week 2 Notes
- [ ] Upload Week 3 Notes
- [ ] Upload Week 4 Notes
- [ ] Upload Week 5 Notes
- [ ] Upload Week 6 Notes
- [ ] Upload Week 7 Notes

Additional Writing

- [ ] Update week 5.2 Leases Note: Math interpretation.



## Calender-Paper

| Week | M               | W                                                            | F                                                            |
| ---- | --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | **09/03** Labor | **09/05** First Class                                        | **09/07** [Hamilton](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/hamilton-deploying-services-07.pdf)  [Dean](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/dean-socc10.pdf) |
| 2    | **09/10**       | **09/12** Fail: [Gray ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/gray-why-do-computers-stop-85.pdf)[alt ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/gray85-easy.pdf)  [Disk](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/bianca-diskfail-07.pdf) | **09/14** Net: [U-net ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/unet-95.pdf)  [eRPC](http://www.cs.cmu.edu/~akalia/doc/erpc/preprint.pdf) |
| 3    | **09/17**       | **09/19** No Class                                           | **09/21** Time: [LClocks ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/clocks.pdf)  [VClocks](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/mattern89.pdf) |

| Week | M                                                            | W                                                            | F                                                            |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 4    | **09/24**                                                    | **09/26** [Flash](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/flash99.pdf)  [SEDA](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/SEDA-sosp.pdf) | **09/28** [Alice](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/alice-osdi14.pdf) |
| 5    | **10/01** [P1:Due](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Projects/p1.html) | **10/02** [WiscKey ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/wisckey-fast16.pdf)[slides](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/070-Wisckey-Slides.pdf) | **10/05** [NFS ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/nfs.pdf)  [Leases](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/leases89.pdf) |
| 6    | **10/08**                                                    | **10/10** [HA-NFS](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/bhide91highly.pdf) | **10/12** [Visitor](https://www.cs.wisc.edu/events/4031)     |
| 7    | **10/15**                                                    | **10/17** [2PC](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/2pc.pdf) | **10/19** [Elections](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/elections.pdf)- [Quorums](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/pbs-vldb2012.pdf) |
| 8    | **10/22** Midterm                                            | **10/24** [Paxos](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/paxos-simple.pdf) | **10/26** [Raft](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/raft.pdf) |
| 9    | **10/29** [P2:Due](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Projects/p2.html) | **10/31** [Bayou ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/terry95managing.pdf)[bball ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/baseball-13.pdf)  [VVs](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/parker83detection.pdf) | **11/02** [LBFS ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/lbfs-01.pdf)- [Jumbo](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/jumbostore-fast07.pdf) |
| 10   | **11/05**                                                    | **11/07** [Lard ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/lard98.pdf)- [DistHash](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/gribble-dht.pdf) | **11/09** [CFS ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/cfs.pdf)- [Chord](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/chord01.pdf) |
| 11   | **11/12**                                                    | **11/14** [Needham](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/needham.pdf) | **11/16** [Petal](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/petal96.pdf) |

| Week | M                                                            | W                                                            | F                                                            |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 12   | **11/19** It's                                               | **11/21** Thanks                                             | **11/23** Giving!                                            |
| 13   | **11/26**                                                    | **11/28** [Frangipani](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/frangipani97.pdf) | **11/30** [GFS](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/gfs.pdf) |
| 14   | **12/03**                                                    | **12/05** [Dynamo ](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/dynamo.pdf)[(blog)](http://www.allthingsdistributed.com/2017/10/a-decade-of-dynamo.html) | **12/07** [BigTable](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/bigtable-osdi06.pdf) |
| 15   | **12/10** [Ramcloud](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/ramcloud.pdf) | **12/12** [Blockchain](http://pages.cs.wisc.edu/~remzi/Classes/739/Fall2018/Papers/blockchain.pdf) | **12/14**                                                    |



## Credit

