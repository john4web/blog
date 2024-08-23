+++
title = "Stream API"
date = "2024-01-24"
description = ""
+++


# Overview

Java Streams were introduced in Java 8 and are a very powerful concept. They provide a set of functions that you can perform on certain data structures. They allow us to quickly and conveniently perform operations on them. Streams themselves are not data structures and they do not modify the underlying data structures they are operating on.

Streams do all the heavy lifting for you.
-> They "streamline" the process for you.

⚠️ Note: this article is about [java.util.stream](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html). Do not confuse it with input and output streams ([java.io](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/io/package-summary.html)). That's a totally different concept!



# How to work with streams
1.	Open the stream with the method `.stream()`.
2.	Chain as many operations as you like.
3.	Convert the stream back into a list (if you need that).

The following example filters out all people that are younger as 10 and return them as a list:
```java
List<Person> people = new ArrayList<>();

people.stream()
    .filter(person -> person.age > 10)
    .collect(Collectors.toList())
```

Basically, there are two types of operations when working with streams - **intermediate** and **terminal** operations. 

* Intermediate operations return a stream again (e.g., the methods .filter() or .map()).

* Terminal operations process the stream to perform functions on the individual elements (e.g., to return a specific value). After that, the stream is closed, and no further operations can be performed on it until a new stream is opened (e.g., the methods .collect or .forEach)."

<!-- 
Grundsätzlich unterscheidet man bei streams zwischen intermediate und terminal operations.
Intermediate operations geben jeweils wieder einen stream zurück (z.B. die Methoden .filter() oder .map())

Terminal operations durchlaufen den Stream um Funktionen auf den Einzelelementen durchzuführen (z.B. um einen bestimmten Wert zurückzugeben). Danach ist der stream geschlossen und man kann nichts mehr darauf ausführen, bis ein nueer Stream eröffnet wird. (z.B. die Methoden .collect oder .forEach)
-->

On the website "GeeksForGeeks", I found a very nice image, explaining that concept: 

{{< figure src="/images/stream-api/1.png" caption="Intermediate Operations are the types of operations in which multiple methods are chained in a row. Source: [geeksforgeeks.org](https://www.geeksforgeeks.org/stream-in-java/)" >}}

# Advantages of using stream api

When working with data, an important rule is:
_Don't change the existing value (original data)! Instead, create a new variable with the changed data!_
This is a concept that streams embrace. When working with streams, you are not working with the existing list. If you change the values of the stream, it will not affect the original data.

Once you consume the stream, you cannot reuse it:
e.g.

The following code works:
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> data = nums.stream();
data.forEach(n -> System.out.println(n)) // closes stream (forEach is a terminal operation)
```

The following code throws an exception:

```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> data = nums.stream();
data.forEach(n -> System.out.println(n)) // closes stream
data.forEach(n -> System.out.println(n)) // Exception: "Stream has already been operated upon or closed".
```

Another advantage: intermediate stream methods return a new stream so you can chain (as many as you want) together.
(Reminds me in some kind of way on the builder pattern).

# Examples

The following code sorts the array, multiplies every number with 2 and prints them to the console:

```java
List<Integer> nums = Arrays.asList(5, 3, 1, 2, 4);

nums.stream()
    .sorted()
    .map(n -> n*2)
    .forEach(n -> System.out.println(n));
    // Output: 2, 4, 6, 8, 10
```

# stream() vs. parallelStream()

When working with streams you can also use the method `parallelStream()` instead of `stream()`. What is the difference?
As the name suggests, `parallelStream()` will do the operations in parallel. It will use multiple threads to do this.
Sometimes your values are dependent on the earlier operation. In that case: don’t use parallel streams.

# List of most important stream api methods




## sorted()
`sorted()` is an intermediate operation!

Sorts your stream:
```java
List<String> names1 = List.of("Charlie", "Alice", "David", "Bob");
List<String> sortedNames = names1.stream()
.sorted()
.collect(Collectors.toList()); // sorted alphabetically
```

```java
List<Integer> numbers = List.of(5, 3, 8, 1, 4);
List<Integer> sortedNumbers = numbers.stream()
                              .sorted()
                              .collect(Collectors.toList()); // sorted ascending
```

## map()
`map()` is an intermediate operation!

Performs a certain operation on each item: 

```java
data.map(n -> n*2)
```

## mapToInt()
`mapToInt()` is an intermediate operation!

## limit(5)
`limit()` is an intermediate operation!

## flatMap()
`flatMap()` is an intermediate operation!

## distinct()
`distinct()` is an intermediate operation!

## skip()
`skip()` is an intermediate operation!

## peek()
`peek()` is an intermediate operation!

## boxed()
`boxed()` is an intermediate operation!



## filter()
`filter()` is an intermediate operation!

data.filter() filters out some items Data.filter(n  ->  n%2==1)

Creating the filter predicate with an anonymous inner class:

Predicate<Integer> predi = (Integer n) -> {
	return n%2==1;
}

Ums.stream().filter(predi)


## reduce()
`reduce()` is a terminal operation!
Data.reduce() will reduce the values

## collect()
`collect()` is a terminal operation!
Data.collect()
Will return a new list z.B. .collect(Collectors.toList())



## count()
`count()` is a terminal operation!

Gives you the size of the stream:
```java
List<String> names = List.of("Alice", "Bob", "Charlie", "David");

        long count = names.stream().count(); // 4
```


## sum()
`sum()` is a terminal operation!

## groupBy()
`groupBy()` is a terminal operation! (usually used with `Collectors.groupingBy()`)

## partitionBy()
`partitionBy()` is a terminal operation! (usually used with `Collectors.partitioningBy()`)

## allMatch()
`allMatch()` is a terminal operation!

## anyMatch()
`anyMatch()` is a terminal operation!

## noneMatch()
`noneMatch()` is a terminal operation!

## findFirst()
`findFirst()` is a terminal operation!

## findAny()
`findAny()` is a terminal operation!

## max()
`max()` is a terminal operation!

## min()
`min()` is a terminal operation!

## toArray()
`toArray()` is a terminal operation!

## generate()
`generate()` is a static factory method! (used to create streams; not an operation on streams themselves)

## iterate()
`iterate()` is a static factory method! (used to create streams; not an operation on streams themselves)

## range()
`range()` is a static factory method! (used to create streams; not an operation on streams themselves)


## parallel()
erklären

## unordered()
erklären

# Reference

https://www.youtube.com/watch?v=FWoYpM-E3EQ&t=25s

https://www.youtube.com/watch?v=tklkyVa7KZo 

https://www.youtube.com/watch?v=E-8qMRqRQ_A 

https://www.geeksforgeeks.org/stream-in-java/ 
