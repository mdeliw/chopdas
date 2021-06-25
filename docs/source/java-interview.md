

### java.nio package

- What is the difference between Java NIO and IO package?

### MappedByteBuffer

Article: https://www.baeldung.com/java-mapped-byte-buffer

- Efficient reads of a file or a section of the file - when we know we’ll need to read the content of a file multiple times, optimize the costly process by saving that content in the memory. Subsequent lookups of that part of the file will go only to the main memory without the need to load the data from the disc.

## Unique

### Advanced

The internal class `sun.misc.Unsafe` provides low-level mechanism that were designed to be used only by core Java library and not by standard users.  

Article: https://www.baeldung.com/java-unsafe

- Instantiate a class without calling its constructor? - Yes
- Alter a private field from outside the package? - Yes

---

### JVM Internals





