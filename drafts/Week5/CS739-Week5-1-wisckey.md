# CS739 Week 5-1 WiscKey

WiscKey: Separating Keys from Values in SSD-Conscious Storage

# Summary



# Questions

**Pre-read questions**

1. What is LSM-tree ? How LSM helps us in the HDD? Is LSM still helpful in SSD? How shall we design new strategy to cooperate with SSD?
2. What's essential in LevelDB? What is `memtable, Imuable memtable, sstable, log, filter` ?



# Reminder

1. Review [LSM-tree](https://en.wikipedia.org/wiki/Log-structured_merge-tree) (in fact please know more about LSM before reading the article). 
   1. [LSM vs B+ tree performace in SSD](https://github.com/wiredtiger/wiredtiger/wiki/Btree-vs-LSM)
   2. [(Zh) Zhihu - What is LSM](https://www.zhihu.com/question/19887265)
   3. [(Zh) LSM笔记](https://kernelmaker.github.io/lsm-tree), [(en) A note about LSM](https://blog.acolyer.org/2014/11/26/the-log-structured-merge-tree-lsm-tree/)



# Detail

==//TODO: OK... My notes all disappeard... sad : (==

## 1. Introduction



## 2. Background and Motivation

### 2.1 Log-Structured Merge-Tree

### 2.2 LevelDB

### 2.3 Write and Read Amplification

### 2.4 Fast Storage Hardware



## 3. WiscKey

### 3.1 Design Goal



### 3.2 Key-Value Separation



### 3.3 Challenges

#### 3.3.1 Parallel Range Query

#### 3.3.2 Garbage Collection

#### 3.3.3 Crash Consistency



### 3.4 Optimizations

#### 3.4.1 Value-Log Write Buffer

#### 3.4.2 Optimizing the LSM-tree Log



### 3.5 Implementation



## 4 Evaluation

### 4.1 Microbenchmarks

#### 4.1.1 Load Performance

#### 4.1.2 Query Performance

#### 4.1.3 Garbage Collection

#### 4.1.4 Crash Consistency

#### 4.1.5 Space Amplification

#### 4.1.6 CPU Usage



### 4.2 YCSB Benchmarks



## 5 Related Work



## 6 Conclusions

