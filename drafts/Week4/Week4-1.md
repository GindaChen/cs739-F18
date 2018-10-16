# CS739 - Week 4 - 1 FLASH & SEDA

Flash: An efficient and portable Web Server

**Summary**

Server needs to cache frequently-requested web content to boot the speed. Among the methods of SPED, MP, MT, AMPED, the paper show that AMPED is superior. AMPED behaves like SPED when request cached document and MP when from disk.

**Section 2 Background**



- **Static content / Dyamic content**
- **Basic algorithm for serving request for static content**



**Section 3 Server Arch**



**3.1 MP: Multi-process** 

​	**Good: No sync is needed to handle differnet HTTP request**

​	**Bad: No optimization for global info. overhead context switch.**



**3.2 MT: Multi-Thread**

​	**Good: Shared info**

​	**Bad: Not many support** **kernel threads,** **Hard to sync. overhead context switch.**



​	**Kernel thread: differnet cpu kernel.**

​		**Hard to assert the mapping of threads**

​		**Hard to prevent the “truly blocked” event**



**3.3 SPED: single-process event-driven (using non blocking system call for I/O)**

​	**Good:  Simplify the model as just a state machine. No overhead context switch.**

​	**Bad: Hard to really get async disk I/O support for most arch.**



​	**select, poll (stat, read, write)**

​	



**3.4 AMPED: Asymmtric multi-process event-driven**

​	**=> P3**

​	**Add helper processes / helper threads (kernel)**

​	**Good:** 

​	**Challenge:** 



​	**mmap, mincore (check file in main memory)**







**Section 4 Design Comparison**



**4.1 Performace Characteristics**



|                      | **AMPED**                                                    | **SPED** | **MP**     | **MT**                                          |
| -------------------- | ------------------------------------------------------------ | -------- | ---------- | ----------------------------------------------- |
| **Disk Performance** | Server process no block. Interprocess communication is main overhead |          | Block      | Block                                           |
| **Memory Effecets**  |                                                              | Smal     | Small      | Additional memory consumption & kernel resource |
| **Disk Utilization** | =helper                                                      | 1        | =processes | =threads                                        |



**? (Memory) AMPED has “small application—level memory demand” ?**



**4.2 Cost/Benefits of Optimization’s & Features**





|                             | **AMPED**                  | **SPED**                   | **MP**                             | **MT**                                           |
| --------------------------- | -------------------------- | -------------------------- | ---------------------------------- | ------------------------------------------------ |
| **Information Gathering**   | Still good (no sync)       | Still good (no sync)       | Bad (inter-process), need sync     | Good                                             |
| **Application-level Cache** | Single cache, without sync | Single cache, without sync | May have own cache, less efficient | Single cache but data access/update must be sync |
| **Long-lived connection**   | File descriptor            | File descriptor            | Extra process                      | Extra thread                                     |





**Section 5 Flash Implementation**



*CGI Application: Common Gateway Interface (1993, NCSA,* 通用网关接口，其实就是链接参数的通用接口）



**5.1 Overview**



**Cache maintained:** 

​	**- File translation**

​	**- Response headers**

​	**- File mapping**



**Helper process:**

​	**- pathname translation**

​	**- bring disk blocks into memory**

​	**- operate sync, dynamically spawn**

​	**- return completion notification to server (async message, rather than block to wait for context switch) to minimize interprocess communication**





**5.2 Pathname Translation Caching**

**5.3 Response Header Caching**

**5.4 Mapped Files:** 

​	**Chunks of files, lazy map,  LRU**



**5.5 Byte Position Alignment:** 

​	**32 bytes boundary, padding**



**5.6 Dynamic Content Generation:** 

​	**Allow long-connection, forward to auxiliary process (CGI-bin),** 



**5.7 Memory Residency Testing**



**SEDA: An Architecture for Well-Conditioned Scalable Internet Services**



**Keywords:**

​	**Staged EDA, dynamic resource controllers, auto tuning, loading condition**





**Summary**







**Staged**







**Section 1 Intro**





**Section 2**



**Well-conditioned:** if it behaves like *simple* *pipeline*, where the **depth** of the pipeline is determined by the path through the network and the processing stages within the service itself.

As load approaches saturation, the queuing delay dominates.



**Graceful Degradation**: maintain highthroughput when at capacity, give linear penalty to everyone equally.





**2.1 Thread-based Concurrency**



​	**Bad: virtualization & metaprogramming purpose denied application to manage data in memory - cache, TLB miss, context switch, scheduling, lock ...**



**2.2 bounded Thread Pools**



​	**Set maximum of threads available.**

​	**Good: prevent rapid degradation.**

​	**Bad: Unfairness (massive request force client queue up and wait),** 

​	         **Hard to tune and load condition  (arbitrarily reject work without knowledge of bottleneck)**



**2.3 Event-driven Concurrency**



​	**Flash, Zeus, JAWS, Harvest**

​	**Good: robust to load, little degradation in throughput**

​	**Bad: assume threads does not block, scheduling, ordering of event affect significantly, modularity is very difficult**



**2.4 Structured Event Queues**

​	

​	

**Section 3 The Staged Event-Driven Architecture**



**3.1 Goals**



**3.2 Architecture Overview**



**Components:**

​	**stage**

​	**incoming event queue**

​	**thread pool**

​	**controller**

​	

**Idea:**

​	**Organize stages as easy as connecting a graph**

​	**Separate logic and from thread management & scheduling**

​		**scheduling: controller, external environment**



**3.3 Network of Stages**



​	**Finite queue: back pressure, load shedding, ...**

​	**Decouple sequential codes: communicate by queue or a call?**

​	**proxy stage: debugging, event tracing**



**3.4 Dynamic Resource Controllers**



​	**Actual CruxL**

​		**Thread pool controller: adjust performace**

​		**Batching Controller: tradeoff between throughput and latency**

​			**=> Observe and try: like the downloader** 



​	**Potential Drawback:**

​		**Naive to OS resource management policies.**

​		**Focus on Thread management.**

**3.5 Sandstorm**