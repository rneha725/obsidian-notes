
[ByteByteGo Reference](https://www.youtube.com/watch?v=I6jB0nM9SKU)
### Writes
1. Write comes in, we have a disk based commit log file
2. Simultaneously, write to a memtable, which looks like [[Skip List]] or a balanced binary tree
3. Once mem table fills up, write to the disk as the immutable SSTable(Sorted String table) 
4. Also, a comapaction algo runs periodically, to merge the SSTables.
5. Once a commit log is flushed into SSTables, it is removed.

![[LSM tree.png]]

### Deletes
1. Always a tombstone
2. Written in the memtable and sorted set as tombstone.

### Updates
1. Write with a new ts

### Reads
1. Check in memtable(Skip list) first
2. Check in SSTables using [[[TODO] Bloom Filters]]
3. The start from the newest SSTable to oldest

As reads could be heavier, to optimize we have:
- **Bloom Filters**: [[[TODO] Bloom Filters]] For each SSTable.
- Skip List or Sparse indexes
- Compaction

### Why this trade is worth it for Cassandra's use case
|                      | LSM tree (Cassandra)                                                               | B-tree (Postgres/MySQL)                                                                      |
| -------------------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Writes**           | Sequential, append-only — no seek, no in-place update, no read-before-write        | Often requires seeking to the right page, possibly reading it first, then rewriting in place |
| **Write throughput** | Very high — this is the whole point                                                | Lower — random I/O, lock contention on the tree structure                                    |
| **Reads**            | Potentially slower — may need to check memtable + multiple SSTables, merge results | Fast — one structure, always current, one seek path (with index)                             |
| **Space**            | Needs compaction to reclaim space from overwrites/deletes                          | Reclaimed immediately (page is overwritten in place)                                         |
| **Deletes**          | Tombstones — deferred cleanup, can cause read slowdown until compacted             | Immediate — row physically removed/marked free                                               |

Remember LSM tree's tradeoff is better write performance for basic read performance.