# Java Code Snippets

##### Count digits, sum digits in integer

```java
//count using loop
count = 0;
for (; num != 0; num /= 10, count++);

//log-10
count = (int)Math.floor(Math.log10(num) + 1);

//sum
sum = 0
for (; num > 0; sum += num % 10, num /= 10 );

//together
count = 0;
sum = 0;
for (; num > 0; count++, sum += num % 10, num =/ 10);
```

##### Arrays

```java
// Create Array with random values within a range
// 100 elements in the range of -10 to 20
Random r = new Random();
int[] arr = r.ints(100, -10, 20).toArray();

//print
Arrays.toString(arr);

// find max
int max = Arrays.stream(arr).max().getAsInt();
```

##### Streams

```java
// stream of random numbers
Random r = new Random();
r.ints(5).sorted.forEach(...);
r.doubles(5)...;
r.longs(5)...;
```

String to Stream and back is not easy. Although String is collection of characters, String does not implement iterable.

```java
string.toLowerCase().codePoints() //returns IntStream
    .collect(StringBuilder::new, //supplier
    		StringBuilder::appendCodePoint, //biconsumer accumulator
            StringBuilder::append) //biconsumer combiner
    .toString();
```

Stream into a collection

```java
List<String> lists = Stream.of(...).collect(Collectors.toList);
Set<String> sets = Stream.of(...).collect(Collectors.toSet());
List<String> linkedLists = Stream.of(...)
    .collect(Collectors.toCollection(LinkedList::new));
String[] arrs = Stream.of(...).toArray(String[]::new);
```

Counting

```java
long count = Stream.of(...).count();
long count = Stream.of(...).collect(Collectors.counting());
Map<Boolean, Long> numberLengthMap = strings.stream()
    .collect(Collectors.partitioningBy(s -> s.length() % 2 == 0, //predicate
                                       Collectors.counting()));  //collector
```

findFirst, findAny

```java
Optional<Integer> firstEven = Stream.of(...)
    .filter(n -> n % 2 == 0)
    .findFirst();

Optional<Integer> firstEvenGT10 = Stream.of(...)
    .filter( n -> n > 10)
    .filter(n -> n % 2 == 0)
    .findFirst();

Optional<Integer> any = Stream.of(...)
    .unordered()
    .parallel()
    .findAny();
```

Concatenating

```java
Stream<String> first = Stream.of(a,b,c);
Stream<String> second = Stream.of(x,y,z);
List<String> strings = Stream.concat(first, second)
    .collect(Collectors.toList());
```

Sorting

```java
strings.stream(...).sorted();
strings.stream(...).sorted((s1,s2) -> s1.length() - s2.length());
strings.stream(...)
    .sorted(Comparator.comparingInt(String::length))
    .collect(Collectors.toList());
```
