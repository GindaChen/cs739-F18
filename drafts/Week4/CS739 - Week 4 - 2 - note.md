# CS739 - Week 4 - 2 ALICE

[Alice](/Users/mike/Documents/GitHub/alice)

[The vedio](https://atscaleconference.com/videos/your-storage-is-broken-lessons-from-studying-modern-databases-and-key-value-stores/)



Examples: `pwrite` itself is not atomic. 



**Update protocols: 4 examples**

Only work at ext3(journal=data) not in ext(journal=ordered) just metadata is logged

- journaling: data -> update metadata
- ordered: 
- writeback mode: no order between data/metadata

**Some problems occur**

1. data written is not ordered
2. data might be reordered
3. mount will make things behave different
4. You might not update the dir entry

**Why this is reordered?**

```c
create(/log);
write(/log, ...);
pwrite(/log,...);
unlink(/log);

```



When something doesnot work, add `fsync`

```c
create(/log);
write(/log, ...);
fsync(/log)
pwrite(/log,...);
fsync(/log)
unlink(/log);

```



## BOB & Alice, OptFS & one-more-things

**Atomicity**: update all at once (can even break down into several separate steps) ?

​	(Alignment (early vm) - miss align right might read 2 sectors, might not atomic)

**Ordering**: code order == persistence order? 



## Persistence Model ( a weak file syste )



## OptFS

Async Durability Notification









