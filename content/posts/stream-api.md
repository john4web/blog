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
        List<Integer> numbers = List.of(1, 2, 3, 4, 5);

        List<Integer> multipliedNumbers = numbers.stream()
                                             .map(n -> n*2)
                                             .collect(Collectors.toList());
// Output: [2, 4, 6, 8, 10]

```

## mapToInt()
`mapToInt()` is an intermediate operation!


## flatMap()
`flatMap()` is an intermediate operation!

Is used to transform each element of a stream into another stream and then flatten those streams into a single stream. It is particularly useful when dealing with collections of collections, such as a list of lists, and you want to create a single unified stream of elements.

```java
List<List<Integer>> listOfLists = Arrays.asList(
            Arrays.asList(1, 2, 3),
            Arrays.asList(4, 5),
            Arrays.asList(6, 7, 8, 9)
        );

        List<Integer> allNumbers = listOfLists.stream()
            .flatMap(list -> list.stream()) // Jeden Unterlisten-Stream zu einem einzigen Stream zusammenführen
            .collect(Collectors.toList());

            // Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]

```

In simple terms, flatMap allows you to:

1. Transform: Convert each element of a stream into another stream.
2. Flatten: Merge all those generated streams into a single continuous stream.

## limit()
`limit()` is an intermediate operation!
Restricts the number of elements in a stream to a specified number:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

        List<Integer> firstThreeNumbers = numbers.stream()
                                                 .limit(3)  // Limit the stream to the first 3 elements
                                                 .collect(Collectors.toList());  // Output: 1, 2, 3
```

## distinct()
`distinct()` is an intermediate operation!

Removes duplicate elements from a stream and retain only unique elements:

```java
List<Integer> numbers = List.of(1, 2, 3, 2, 4, 5, 1, 6, 4);

        List<Integer> uniqueNumbers = numbers.stream()
                                             .distinct()
                                             .collect(Collectors.toList()); // Output: [1, 2, 3, 4, 5, 6]
```

## skip()
`skip()` is an intermediate operation!

Skips a specified number of elements at the beginning of a stream:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        List<Integer> result = numbers.stream()
                                      .skip(3)
                                      .collect(Collectors.toList());
                                      // Output: [4, 5, 6, 7, 8, 9, 10]
```

## peek()
`peek()` is an intermediate operation!

 is used to apply an operation to each element of a stream without modifying the stream itself. It is especially useful for debugging or logging, allowing you to inspect the state of elements during stream processing.

This method exists mainly to support debugging, where you want to see the elements as they flow past a certain point in a pipeline. Since Java 9, if the number of elements is known in advance and unchanged in the stream, the `.peek ()` statement will not be executed due to performance optimization. Using peek without any terminal operation does nothing.

```java
 List<Integer> numbers = List.of(1, 2, 3, 4, 5);

// Create a stream, apply the Peek operator to each element, and collect the processed elements
List<Integer> result = numbers.stream()
                              .peek(n -> System.out.println("Processing: " + n))
                              .map(n -> n * 2)
                              .peek(n -> System.out.println("Doubled: " + n))
                              .collect(Collectors.toList());

```
Output:

```yaml
Processing: 1
Doubled: 2
Processing: 2
Doubled: 4
Processing: 3
Doubled: 6
Processing: 4
Doubled: 8
Processing: 5
Doubled: 10

Result List: [2, 4, 6, 8, 10]
```

## boxed()
`boxed()` is an intermediate operation!

The method boxed() is designed only for streams of some primitive types (IntStream, DoubleStream, and LongStream) to box each primitive value of the stream into the corresponding wrapper class (Integer, Double, and Long respectively).

This throws an Exception:
```java
IntStream.of(1, 2, 3, 4, 5)
.collect(Collectors.toList()); // Compilation Error!
```

This works:
```java
IntStream.of(1, 2, 3, 4, 5)
.boxed()
.collect(Collectors.toList());
```

## filter()
`filter()` is an intermediate operation!

Filters out items based on a given criteria:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

        List<Integer> filteredNumbers = numbers.stream()
                                             .filter(n  ->  n > 3)
                                             .collect(Collectors.toList()); // Output: [4, 5]
```

Creating the filter predicate with an anonymous inner class:

```java
Predicate<Integer> predi = (Integer n) -> {
	return n > 3;
}

List<Integer> filteredNumbers = numbers.stream()
                                             .filter(predi)
                                             .collect(Collectors.toList());
```

## count()
`count()` is a terminal operation!

Gives you the size of the stream:
```java
List<String> names = List.of("Alice", "Bob", "Charlie", "David");

        long count = names.stream().count(); // 4
```


## sum()
`sum()` is a terminal operation!

 Is used to calculate the sum of elements in a stream. This function is available in the primitive streams like IntStream, LongStream, and DoubleStream. The sum() method returns the sum of the elements as a primitive value (int, long, or double depending on the stream type).

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Use mapToInt to convert List<Integer> to IntStream and then sum all the numbers
int sum = numbers.stream()
        .mapToInt(Integer::intValue) // Convert Integer to int
        .sum(); // Sum all the int values
// Output: 15
```


## allMatch()
`allMatch()` is a terminal operation!

checks if all elements in a stream match a given predicate. The method returns a boolean value:

- true if all elements satisfy the provided predicate.
- false if at least one element does not satisfy the predicate.

```java
List<Integer> numbers = Arrays.asList(3, 7, 10, 15, 2);

        // Check if all numbers in the list are positive
        boolean allPositive = numbers.stream()
                                     .allMatch(num -> num > 0); // Predicate: Check if all numbers are positive
```

This output is `true` because all the numbers in the list are positive. If any number in the list was negative or zero, the result would be `false`.

## anyMatch()
`anyMatch()` is a terminal operation!

checks if at least one element in the stream matches a given predicate. It returns a boolean value:

- true if at least one element in the stream satisfies the provided predicate.
- false if no elements match the predicate.

```java
List<Integer> numbers = Arrays.asList(3, 7, 10, 15, 2);

        // Check if any number in the list is greater than 10
        boolean anyGreaterThanTen = numbers.stream()
                                           .anyMatch(num -> num > 10); // Predicate: Check if any number is greater than 10
```

This output is `true` because there is at least one number (15) in the list that is greater than 10.

## noneMatch()
`noneMatch()` is a terminal operation!

checks if no elements in the stream match a given predicate. It returns a boolean value:

- true if no elements in the stream satisfy the provided predicate.
- false if at least one element matches the predicate.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Check if none of the numbers in the list are negative
boolean noneNegative = numbers.stream()
        .noneMatch(num -> num < 0); // Predicate: Check if each number is negative
```

This output is `true` because none of the numbers in the list is negative.

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




## groupBy()
`groupBy()` is a terminal operation! (usually used with `Collectors.groupingBy()`)

## partitionBy()
`partitionBy()` is a terminal operation! (usually used with `Collectors.partitioningBy()`)

## reduce()
`reduce()` is a terminal operation!


## collect()
`collect()` is a terminal operation!
Data.collect()
Will return a new list z.B. .collect(Collectors.toList())





## parallel()
erklären

## unordered()
erklären

# Reference

- https://www.youtube.com/watch?v=FWoYpM-E3EQ&t=25s

- https://www.youtube.com/watch?v=tklkyVa7KZo 

- https://www.youtube.com/watch?v=E-8qMRqRQ_A 

- https://www.geeksforgeeks.org/stream-in-java/ 

- https://www.geeksforgeeks.org/stream-peek-method-in-java-with-examples/

- https://dev.to/imanuel/the-java-boxed-method-571n

- https://www.youtube.com/watch?v=Nz6TdVivRVA&t=38s

- ChatGPT