##### Count digits in integer

```java
//loop
count = 0;
for (; num != 0; num /= 10, count++);

//log-10
count = (int)Math.floor(Math.log10(num) + 1);
```

##### Sum digits in integer

```java
sum = 0
for (; num > 0; sum += num % 10, num /= 10 );
```

##### Count and Sum digits in integer

```java
count = 0;
sum = 0;
for (; num > 0; count++, sum += num % 10, num =/ 10);
```

