## Introduction

Most bread-and-butter algos and data structures we work on a daily basis have been developed with the assumption that all data fits in the main memory of one’s computer, and with fast and easy access to any elements of it, the ultimate efficiency goal is to maximize productivity while using fewest cpu cycles.  :warning: The reality is that data scientist work with TB and PB sizes of data, and such data structures become too large to efficiently work with.

Look at *external-memory algorithms* that neglect the computational cost of program in favor of optimizing far more time-consuming data-transfer costs. Eg. back in olden days, you have a working memory of 4MB, but need to sort 800MB worth data within 12 hours - this requires figuring out how to do so with smallest number of trips to the disk. :warning: This problem has grown now since data has grown at much faster rate than the average size of RAM memory.== Data intensive in constrast to CPU-intensive means that the bottleneck of the application comes from transferring data back-and-forth and accessing data, rather than doing computation on the data.

### Example

3 billion user comments totalling 600GB in data size. You want to determine most popular comments and articles, classifying articles into themes and common keywords ocurring in the comments and so on. Dataset has duplicate comments :weary:. How to solve it?

```json
{
    comment-id: 2833908010
    article-id: 779284
    user-id: 9153647
    text: this recipe needs more butter   
    views: 14375
    likes:  43 
}
```

- Common way is to leverage key-value structure with key being element’s distint id, and value representing the its frequency. So use comment-id as the key and the number of occurrences of that comment-id as the value will help us store distinct comment-ids and their frequencies, and effectively eliminate duplicates.  :warning: But in this case teh key-value pair will use 8 bytes per pair, and 3 billion comments means we will need atleast 24GB, usually 1.5x to 2x space to account for empty slots, pointers etc
- Similarly, mapping article-id to keyword frequency such as an article has 25 politics-related keywords. :warning: Depending on number of topics such as politics, the topic hashmap could easily be 30GB and another 20GB for mapping articles to topic frequencies. 
- Tree dictionary apart from read, write, delete in log time, also offers previous / next operations to explore data using lexicographical ordering, whereas key-value structures offer constant-time read, writes, and deletes. Also hash tables use asymptotical minimum of space required to store data correctly, but with large dataset sizes, hash tables can not fit into the main memory.

#### How to solve it?

- If we settl with a small margin of error, we can build data structures similar to hash tables, only more compact. *succinct data structures* use less than the lower theoritical limit to store data.  Key use cases include the following:
  - Does user x exist?
  - How many times the user X comment? What is the most popular keyword?
  - How many distinct comments / users do we have?
- Bloom filter, Count-Min sketch, HyperLogLog are data structures that are succinct.
- Processing events as stream, say *someone clicks on Like* on a particular comment. We receive the comment, but we only provide approximate solutions such as average number of likes per comment in the past week. We can also use random sampling such as Bernoulli sampling algorithm.
- If we prefer accuracy over speed, we might want to store all comment data in a persistent format - in this we would use external-memory algo which deals with read-optimized or write-optimized or read-write-optimized systems.

### Why is massive data challenging?

- Many parameters in computers and distributed system architecture shape the application. 
- Large asymmetry between the CPU and the memory speed
- Different levels of memory and tradeoff
- Issue of latency vs bandwidth

#### The CPU-memory performance gap

- Year over year speed of processor is 1.5x, while speed of memory access is 1.1x. Computation is much faster than data. 
- Different characteristics of memory hierarcy
  - fast is expensive and small
  - slow is cheaper and large
  - Registers - 20ps
  - L1 cache - 1ns
  - L2 cache - 3 ns
  - L3 cache - 10 ns
  - Main memory - 80 ns
  - SSD - 100 µs
  - HDD - 10 ms
- Bandwidth has improved, but latency hasn’t. To overcome latency issues, data transfer between different levels of memories is done in chunks of items - called cachelines or pages or blocks such as 8 - 64 bytes. This is called *spatial-locality* where we expect the program to access memory locations that are in vicinity of each other close in time

#### Design algos with hardware in mind

- tradeoff between speed and size of memories are not going away anytime soon.
- To store a lot of data, we need a lot of space, and the speed of light sets the physical limits to how fast data can travel.
- Disk-based applications can benefit from optimized patterns of disk access such as how we lay out and organize data and caching mechanisms that enable smallest number of memory transfers.

## Hash tables and modern hashing

- Hashing is the backbone of all succinct data structures - also the abstract data structure is the dictionary
- Classical hash tables are irreplaceable in modern systems. There has been innovative work addressing algo issues with growing data, such as efficient resizing, compact representation, space-saving tricks.
- Hash tables are also served in massive peer to peer systems where the table is split among servers - key challenges being assignment of resources to servers, and load-balancing as servers join and leave the network.

#### Crash course

Linear structures:

- Unsorted array offer O(1) for inserts as new elements are appended, however lookup requires full linear scan O(n).  This can work as a dictionary where we want fast inserts and lookups are rare.
- Sorted  array offers O(logn) lookups - kind of same as constant time for small arrays. However we pay the price to maintain the sort.
- Linked list offer O(1) for inserts and deletes, however reads are linear
- Using well-designed hashing, we can bring dictionary operation costs down to O(1) in theory. However hash tables are poorly suited where data order is important. 
- Usually there is atleast one operation that costs O(n) and to avoid it, we need to break out from this linear structure

Tree structures:

- Balanced binary search tree have all dictionary operations depenent on depth of the tree and all inserts, deletes, and reads take O(logn), and balanced binary search trees also maintain sort

#### Usage scenarios

- Deduplication in backups - eg. File is broken down into 8KB, hashed into 20-bytes SHA-1 fingerprint
- Plagiarism-detection service - MOSS - measure of software similarity, Karp-Rabin string-matching algo supports k-gram fingerprinting

#### Collision resolution

- Two common hashing mechanisms:linear probing and chaining 
- Chaining associates with each bucket of the hash table an additional data structure such as linked list or BST
- Linear probing, also called open addressing, stores items inside hash table slots. To insert, we hash it to a corresponding bucket. If the bucket is occupied, we look for first available open slot position scanning downwards in the table, wrapping around the end of the table. The search begins from the slot determined by the bucket we hashed to, scanning downward until we either find the element or encounter an empty slot
- In collision resolution, the assumption is that hash function is random. This is like throwing n balls into n bins uniformly and randomly. The probability is that the fullest bin will have O(logn / log logn) balls - this is the longest chain, giving an upper bound on lookup and delete performance
- Remember hashring
- 