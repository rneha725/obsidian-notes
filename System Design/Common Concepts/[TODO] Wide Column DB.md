- A wide column db can have millions of columns and it is generally partitioned on columns.
- A row looks like this: 
```java
Map<RowKey, SortedMap<ColumnKey, Value>> //columns are sorted according to the column key

//row key determines which node data lives on
```