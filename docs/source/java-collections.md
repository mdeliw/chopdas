[toc]

### Deque

- Double ended queue
- Supports element insertion and removal at both ends
- Operations to access at both ends of the deque (Head, Tail, First, Last)
- Use as a FIFO (Queue) - elements are added at the end of deque and removed from the beginning
- Use as a LIFO (Stack) - elements are pushed and popped from the beginning of the deque.

### List

#### Interface

https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/List.html

- Ordered collection - you have precise control on where in the list an element is inserted.
- Access elements by their index, and search for elements in the list.
- Allows duplicates (`e1.equals(e2)`)
- Special iterator `ListIterator` allows insertion, replacement, and bidirectional access, and can obtain an `Iterator` that starts at a specific position in the list.
- Unmodifiable Lists
  - `List.of`, `List.copyOf` provide a way to create unmodifiable lists. 
  - Unmodifiable - no adds, removes, replaces, can’t call any mutator method, however objects within the list can be mutable.
  - No nulls, value-based, and order of elements matches the order of provided arguments
- Few operations:
  - `List.of`
  - `remove(Object o)` - removes the first occurrence of the specified element if it is present.
  - `removeAll(Collection<?> c)` - removes from this list all of its elements that are contained in the specified collection.
  - `retainAll(Collection<?> c)` - retains in this list only those elements that are contained in the specified collection.
  - `replaceAll(UnaryOperator<E> operator)` - replaces each element of this list with the result of applying the operator to that element
  - `sort(Comparator<? super E> c)`
  - `subList(int fromIndex, int toIndex)`
  - `Object[] toArray()` and `<T> T[] toArray(T[] a)`

#### LinkedList

https://www.baeldung.com/java-linkedlist

- Doubly-linked list implementation of `List` and `Deque`
- Not synchronized, but iterators wll throw a `ConcurrentModificationException` if the list is modified. 
- Get a synchronized linked list:
  - `List list = Collections.synchronizedList(new LinkedList(...))`
- Doesn’t provide random access, search is O(n)
- Insertions and removals are fast
- Consumes more memory

#### ArrayList

https://www.baeldung.com/java-arraylist

https://www.baeldung.com/java-immutable-list

https://www.baeldung.com/java-copy-on-write-arraylist

https://www.baeldung.com/java-multi-dimensional-arraylist

- Usually the default implementation for `List`
- Provides random access to its elements with performance of O(1).
- Adding is O(1)
- Insertions and removals are O(n)
- Searching is O(n)

### Set

#### TreeSet

#### HashSet

### Map

#### HashMap

#### TreeMap

### Queue

