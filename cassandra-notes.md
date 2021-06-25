# Cassandra

## Misc

- Partitioned row store, supports AP
- Cluster contains nodes; a single instance running cassandra is a node; 
- Node contains set of rows; each node is assigned set of rows; each row is stored in specific node based on the hashed value of the partition key - called a token; 
- a key space is like a database and maps to collection of tables; key space has a replication factor; 
- each node is primary for some token range, and may also be a replicated node for some ancillary token range - for example a replication factor of 3 means three copies of the same row - in primary node, secondary node, and a tertiary node
- Partition key is the first part of the primary key, and determines the row’s token or placement in the cluster
- Component responsible to determine which node has what data is the Partitioner, and different types of partitioners are available based on needs of efficiencies and data distribution strategies
  - Partitioner is established at a cluster level, not lower than that (i.e. not at a keyspace or table level)
  - Changing of partitioner requires reloading all the data
  - RandomPartitioner - default based on MD5
  - ByteOrderedPartitioner - sorts rows in a cluster by the partition key
  - Murmur3Partitioner - improved hashing algo of the RandomPartitioner - with token range of 2^-63 to 2^63 - 1
- Nodes can be assigned continuous token ranges, an improved concept of vnode can be assigned non-continuous token ranges. Vnodes provide better data distribution control, easier recalculation of token ranges when new vnodes are added to the cluster
- cassandra cluster manager to install cassandra
- cql - cassandra query language

## Basic installation

```bash
tar -zxvf apache-cassandra-3.11.2-bin.tar.gz
# conf in conf/cassandra.yaml
```

- Use cql scripts `cqlsh`
- Create admin user credentials
- Create keyspace to put tables
- Connect to the keyspace
- Create table(s)
- Shutting down a node means the following activities:
  - disable gossip - keeps other nodes from communicating with this node while we are bringing it down.
  - disable binary - prevents this node from serving client requests
  - drain - prevents this node from accepting new writes, and forcing all in-memory data to be written to disk
  - kill the pid

## Write path

- When a write operation reaches a node, it is persisted in two places:
  - in-memory structure known as a memtable. On a flush event of the memtable, the data stored in the memory is written to the sorted string table file (SSTables) on-disk in a sequential order - this is by C* is called a log-based, append-only storage engine. The sorted key is the token value built in the memtable. 
  - commit log on-disk
- SSTables are immutable, and data is sorted by the token value, and then by clustering keys.
- Data written to the same partition key and column values doesn’t update. The existing row is made obsolete (not deleted or overwritten) in favor of the timestamp of the new data.

## Read Path

- Its more complex. The structures in-memory and on-disk are examined and then reconciled.
- Node handling a read operation will send the request on two separate paths:
  - If row-caching is enabled, then Memtables (in-memory) 
  - Using bloom filter - probabilistic-based structure in RAM, speed up reads from disk by determining which SSTables are likely to contain the requested data



## More reading

- https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archTOC.html
- https://www.linkedin.com/pulse/gossip-protocol-inside-apache-cassandra-soham-saha/

- http://cassandra.apache.org/doc/latest/operating/topo_changes.html