### Types

#### B-Tree indexes
- B-tree nodes can have multiple children - typically hundreds in practice. It is balanced tree.
- [[[TODO] B-tree indexes]]
- Maintain sorted order so range queries and order by operations are efficient. Also, equality seach.
- Self balancing, so performance is always stable.
- Orders the results.

### LSM Trees
- [[Log Structured Merge Trees (LSM trees)]]
- For a lot of writes.
- Append only approach.
- Remember LSM tree's tradeoff is better write performance for basic read performance.

### Hash Indexes
Do not use them in interviews unless you have a specific case.
- For exact match queries.
- Collision can happen, we can chain.
- B-trees can handle equality comparisons almost as efficiently as hash indexes.
- Rarely used in practice.
- But we can use these for in-memory based retrieval, for disk b-trees are better.

### Geospatial Indexes
- More on: [[Geo Spatial Indexes]]

- Proximity queries: Used for queries like find all locations in 5km radius.
- B-tree indexes won't help here.



Say this in interviews:

"Traditional indexes like B-trees don't work well for spatial data because they treat latitude and longitude as independent dimensions. To efficiently search for nearby locations, we need an index that understands spatial relationships. Geohash is a hash-based approach that converts 2D coordinates into a 1D string, preserving proximity. This allows us to use a regular B-tree index on the geohash strings for efficient proximity searches. However, tree-based approaches like R-trees can offer more flexibility and accuracy by grouping nearby objects into overlapping rectangles, creating a hierarchy of bounding boxes."

### Inverted Indexes
- B-trees can help with prefix matches but not with the match anywhere in a text field.
- [[Elastic Search]], [[Postgres#GIN indexes (Generalized Inverted Index)|Postgres GIN]]
- *Tradeoffs:* Storage, when the update in a doc happens, need to update all affected indexes. On the brighter side, better text searchability.


### Composite indexes
- Needed for composite queries on b-trees. Ordering applied too.

### Tradeoffs
- Extra storage of index.
- Write performance can take a hit.
- 