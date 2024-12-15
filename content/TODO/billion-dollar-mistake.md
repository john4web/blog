+++
title = "NULL: The Billion Dollar Mistake"
date = "2024-11-26"
description = "The downsides of using NULL-References explained"
+++

## Overview

This blog article explores the keyword `null`, a staple in every programmer's daily work. Should we reconsider the use of null references in our software? Could encountering `null` in our code actually be harmful? This article aims to address these questions and provide insight into the implications of using `null`.

{{< figure src="/images/billion-dollar-mistake/oppenheimer.webp" width="80%" caption="Robert Oppenheimer's regret over inventing the atomic bomb serves as a fitting analogy for Tony Hoare's regret over introducing the null reference (Image-Source: Movie: [Oppenheimer (2023) by Christopher Nolan](https://en.wikipedia.org/wiki/Oppenheimer_(film))).">}}

To understand why `null` exists at all, we must travel back to 1965, when the null reference was invented by Tony Hoare. In a renowned lecture in 2009, while accepting the Turing Award, Hoare famously referred to the creation of null references as his *"billion-dollar mistake"*:

> I call it my billion-dollar mistake. It was the invention of the null reference in 1965. At that time, I was designing the first comprehensive type system for references in an object oriented language (ALGOL W). My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.<br>
> — <cite>[Tony Hoare](https://web.archive.org/web/20090628071208/http://qconlondon.com/london-2009/speaker/Tony+Hoare)</cite>

Null references are a common source of software errors, such as null pointer exceptions, which can cause system crashes and security issues. When Tony Hoare introduced null references, his motivation was practical: it seemed like a simple way to indicate that a reference didn't have a valid value. However, this seemingly straightforward choice has had lasting consequences, resulting in decades of costly bug fixes and the need for complex security measures. Looking back, Hoare expressed regret for his decision, admitting that he couldn't have foreseen the problems it would create. Today, his opinion on null references is clear:

> Null references have been a historically bad idea.<br>
> — <cite>[Tony Hoare](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare)</cite>

## The luminaries opinion

Who is Tony Hoare? Tony Hoare is a British computer scientist who gained widespread recognition for his contributions to the field. He is best known for creating the _Quicksort algorithm_, one of the most efficient sorting methods, and _Hoare logic_, which allows the correctness of algorithms to be proven. He also developed a process algebra that influenced programming languages like Ada, Occam, and Go. In 1980, Hoare received the [Turing Award](https://en.wikipedia.org/wiki/Turing_Award), often referred to as the "Nobel Prize of Computer Science", for his work on the definition and design of programming languages.

{{< figure src="/images/billion-dollar-mistake/hoare.jpg" caption="Sir Charles Antony Richard Hoare" width="30%">}}

 Why do I mention all of this? Because when a luminary in computer science like Tony Hoare declares, that null references are a bad idea, it carries significant weight — particularly given that he himself was the one who introduced them. And he is not alone in his opinion. As Hoare noted in his [Turing Award lecture](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/), Edsger W. Dijkstra also regarded null references as a bad idea.

{{< figure src="/images/billion-dollar-mistake/dijkstra.jpg" caption="Edsger Wybe Dijkstra" width="30%">}}

Dijkstra was another luminary in the field of computer science. He was a pioneer of structured programming and famous for the _Dijkstra's algorithm_. In 1972, he received the Turing Award as well. Dijkstra used humor to illustrate the problem with null references in programming. In a metaphor, he described objects as bachelors and `null` as a person: _"If you have a null reference, then every bachelor who you represent in your object structure will seem to be married polyandrously to the same person Null"_.

If you search the internet, you'll find many people who view the use of null references as bad coding practice. One of them is [Yegor Bugayenko](https://www.yegor256.com/about-me.html). In his [video on null references](https://www.youtube.com/watch?v=o3aNJX7AP3M), Yegor presents insightful, practical code examples that highlight the issues associated with working with null values, while also offering effective solutions. The following chapters summarize Yegor Bugayenko's video and his perspectives on null references:

## Nine reasons to avoid null references

The following code snippet shows a method that fetches an employee object from a database:

```java
public Employee getByName(String name){
    int id = database.find(name);
    if(id < 0){
        return null;
    }
    return new Employee(id);
}
```

If the database does not find the employee, the method returns `null` instead of returning an instance of `Employee`.
This is terribly wrong! **But why?** Here, I present you 9 reasons why this is terribly wrong:

**1. Because it may cause many nullpointer exceptions in the code.**

For example:
```java
getByName("Jeffrey").salary();
```
If there is no employee with the name "Jeffrey" in the database, then the above code throws a nullpointer exception.

There is no such thing as:
```java
null.salary();
```

**2. Because it is technically difficult to trace the causes of nullpointer exceptions.**

Most of the time, there is a lack of context in stack traces. Usually, the stack trace points to the line where the nullpointer exception happened but not to the specific variable or expression that was ``null``. Modern programs with complex and nested code structures, with delayed execution and with lambdas make it even harder to debug.

**3. Because null references destroy our trust to the objects we are working with.**

When we do not use `null` in our code, we can trust, that objects will be objects. We can trust that objects will look like objects and that objects will behave like objects. However, when we introduce null references into our code, then we cannot trust our objects anymore. They are no longer reliable or self-sufficient.

Let’s assume we have a method that claims to return an object. If this method unexpectedly returns something that isn’t an object, it disrupts our entire reasoning. We can no longer be certain about what is ``null`` and what is not ``null``, leaving us in a state of uncertainty.

Then we start to suspect all objects that they are no objects anymore, even though they might actually still be. We suspect all the variables and everything that we are working with that it is not good enough. We cannot know whether we can "touch" the variables or not because they could be `null` and then the code breaks if we try to access them. In this way objects are not real objects anymore and they are not following the contract we are expecting.

**4. Because you have to insert countless null checks in your code.**

The previously mentioned trust issues bring another disadvantage: You **always** have to check if variables are `null` before you can do something with them. You have to add those null-checks **everywhere** to make sure that your program works reliable and avoids NullpointerExceptions. In this way, the code base grows and grows and the methods/classes become longer and longer. The code becomes cluttered, unclear and hard to understand/maintain. This is the opposite of the "Clean Code" approach!

**5. Because null references violate the core idea of encapsulation.**

When a method can return ``null`` instead of an actual object, the calling code must explicitly check for ``null`` to avoid exceptions. This shifts the responsibility of managing the object's internal state to the external code. For example:

```java
Object obj = someMethod();
if (obj != null) {
    obj.doSomething();
} else {
    // Handle null case
}
```

Instead of encapsulating behavior, the caller has to manage an exceptional case (``null``) that ideally should have been handled inside the object. In this way,null references violate encapsulation. Internal states are exposed and external code is forced to handle the consequences. This undermines the very essence of objects being self-contained and reliable entities. With proper encapsulation, objects manage their behavior internally and provide meaningful defaults or throw specific exceptions if something goes wrong. Returning ``null`` avoids this responsibility, leaving the caller to guess what went wrong or what to do next.

**6. Because null references are completely against the object oriented paradigm.**

Null references break encapsulation and the trust in objects. In OOP, objects should model Real-World-Concepts. `Null` however, does not represent anything meaningful in the real world. For instance, an object like ``Car`` represents a car, but `null` doesn’t represent "no car" in an intuitive way — it’s simply the absence of a reference. 

In my opinion, polymorphism is **the key principle of OOP** (I explain why in [this](https://gersti.at/posts/polymorphism/#the-pillars-of-oop) article). However ``null`` even contradicts polymorphism. Polymorphism lets objects define how they behave using interfaces and inheritance. Null references break this because ``null`` doesn't have any behavior. ``Null`` can't inherit! ``Null`` can't implement interfaces!

Furthermore, introducing `null` often leads to procedural-style code, with if-statements checking for null scattered throughout the codebase. This undermines the OO principle of objects interacting through well-defined methods, making the code less modular and harder to maintain.

**7. Because `null` is an ancient concept that originated in low-level programming languages.**

`Null` was originally introduced in languages that use pointers, like [C](https://en.wikipedia.org/wiki/C_(programming_language)) for instance. However, when programming in Java, we don't think about pointers or memory. Instead, we deal with objects and references. In _"Object-Thinking"_, we view objects as "living organisms" or "creatures." The thinking is about objects, the messages you send to them, and the reactions they send back to us. But ``null`` doesn't have any reaction since it is not an object!

**8. Because a lot of computer viruses are designed to exploit null references.**

Those viruses hope that some software forgets to check the null references and that they can reach that part in the program.

**9. Because null spreads like a disease.** 

``Null`` behaves like a disease in our applications. If we allow it to enter our software, it grows and eventually infects the entire code. For instance, if a method returns null, you have to insert null-checks for it. These checks spread as more methods interact with the null-returning method, leading to a domino effect. The need to protect against ``null`` infects every layer of the system. When one method allows ``null`` as a valid return value or parameter, others must follow suit. 

**Conclusion**

What do these 9 reasons tell us? When it comes to `null`, there is no middle ground — it's either black or white. Using `null` means the code is flawed and needs refactoring. Avoiding `null` results in clean, reliable code.

## Three possible alternatives to null references

Now we know, that null references are really bad and that they have to be avoided at all cost. But how? What should we use instead of null references? Yegor presents two alternatives:

### Alternative #1: Throw a Custom Exception instead of returning `null`:

Given the above example, you could simply throw your own exception:

```java
public Employee getByName(String name){
    int id = database.find(name);
    if(id < 0){
        throw new EmployeeNotFoundException(
            "I'm sorry, but the employee is not found"
        )
    }
    return new Employee(id);
}
```
Yegor's suggestion of throwing an exception also reminds me on the principle _"Crash Early!"_, which was formulated in the book *[The Pragmatic Programmer](https://en.wikipedia.org/wiki/The_Pragmatic_Programmer)* (on page 120-121). When your code encounters something that should have been impossible, the program is no longer reliable. Anything it does after that point becomes questionable, so it’s better to stop it as soon as possible. A program that has crashed, typically causes less harm than one that continues to run in a broken state.

### Alternative #2: Return a Null-Object instead of `null`:

You could use the so-called _"Null Object Design Pattern"_. Given the above example, this solution could look like that:

```java
public Employee getByName(String name){
    int id = database.find(name);
    if(id < 0){
        return new NoEmployee();
    }
    return new Employee(id);
}
```

In this case, the class NoEmployee could implement the same interface as Employee but in a different way than the Employee.

So how does the null object pattern work?

{{< figure src="/images/billion-dollar-mistake/null-pattern.svg" caption="Components of Null Object Design Pattern. The Client depends on the `DependencyBase`. The `Dependency` and the `NullDependency` implement this interface in different ways. Source: [geeksforgeeks.org](https://www.geeksforgeeks.org)">}}

A code-example of this pattern in action:

```java
public interface Animal {
    void makeSound();
}

public class Dog implements Animal {
    public void makeSound() {
        System.out.println("woof!");
    }
}

public class NullAnimal implements Animal {
    public void makeSound() {
        // do nothing
    }
}
```

If you are currently working with ``Animal`` objects but there is no animal available, you can use a ``NullAnimal`` instead of ``null``. This represents the absence of an animal and simply does nothing (see the makeSound() method). The big advantage is that this approach does not trigger a NullPointerException in your code. Of course, you could also throw a custom exception in the makeSound() method of the ``NullAnimal``:

```java
public class NullAnimal implements Animal {
    public void makeSound() throws NullAnimalSoundException {
        throw new NullAnimalSoundException("A NullAnimal cannot make any sound!");
    }
}
```

In this situation you might ask yourself: "Doesn't a null object just delay the failure to a later point in the code?". And the answer is: Yes, definately! In this case we just delay the moment when the exception is thrown. But the advantage here is, that we as software engineers can "control" the process in this way. And we can throw meaningful exceptions on our own. Exceptions that contain better information on the failure scenario.

**These two alternatives (throwing custom exceptions and using Null-Objects) are the best ways to deal with null references!** In most situations probably ``Alternative#1`` is the best solution: just throwing an exception immediately.

### ⚠️ Using Optionals is not an alternative ⚠️

In Yegor's opinion, the use of `java.util.Optional` is not a solution at all. Optionals would deal with the problem scenario in the following way:

```java
public Employee getByName(String name){
int id = database.find(name);
if(id < 0){
    return new Optional();
}
return new Optional(Employee(id));
}
```

```java
Optional<Employee> opt = getByName("Jeffrey");
if(opt.has()){
    opt.get().salary();
}else{
    System.out.println("sorry");
}
```

An Optional is kind of a container including one or zero elements.

Using Optionals is not a good idea. Why? Because it is not a solution for null references. It is the same as null references. The optional container is not an Object by itself. It is just a temporary memory structure which holds a real object. You still have to check if the optional contains an employee or if it has no employee. It is something that is presented as the solution of the null reference problem but actually it is not.

### Kotlin's solution

Kotlin has attempted to solve the problem by making variables either nullable or non-nullable. Types are non-nullable by default. If you want a variable to hold a null value, you must explicitly declare it as nullable using `?`.

```java
// Non-nullable variable
val nonNullable: String = "Hello, Kotlin"

// Nullable variable
val nullable: String? = null
```

The advantage: The question mark immediately shows which object can be trusted and which object cannot. If you try to call a method on a nullable object, the compiler warns you and advises you to insert a null-check.

Kotlin introduced safe calls ``?.`` and the elvis operator ``?:`` as well:

```java
// if nullable evaluates to null, then .length is not executed and null returned instead.
val length: Int? = nullable?.length

// provides the default value 0 when nullable?.length evaluates to null.
val defaultLength: Int = nullable?.length ?: 0
```

## Refutation of the counterarguments

There might be some counterarguments against the usage of [Alternative #1](#alternative-1-throw-a-custom-exception-instead-of-returning-null) and [Alternative #2](#alternative-2-return-a-null-object-instead-of-null) in your mind. Maybe this chapter can still convince you that they are good solutions. 😉

### Counterargument #1:

**Why throw a custom exception when we could just let the NullPointerException occur? In the end, the result is the same, right?**

No! There is a big difference between a NullpointerException raised by the system or a EmployeeNotFoundException thrown manually by us developers. The NullpointerException is thrown by the JVM, which is outside of our developer's scope, which means we can't control it. But the EmployeeNotFoundException is thrown by our objects so we are in charge, we can control it, we encapsulate this behaviour. Our objects control it and not the Java Runtime Environment. Furthermore, the EmployeeNotFoundException can contain a custom error message written by the developer which is alo important for understanding the whole situation.  

However, it is important that the program flow should not be controlled via exceptions! This is a trap, that developers often fall for.

### Counterargument #2

**`Null` is fast and exceptions are slow (regarding code performance)!**

The following two code snippets illustrate this argument. The argument is that the first code snippet is significantly less performant than the second one.

```java
try{
    Employee jeff = getByName("Jeffrey");
    System.out.println(jeff.salary());
}catch(EmployeeNotFoundException ex){
     System.out.println("no jeffrey here");
}
```

```java
Employee jeff = getByName("Jeffrey");
if(jeff == null){
    System.out.println("no jeffrey here");
}else{
    System.out.println(jeff.salary());
}
```

This is true! The second code snippet is way faster than the first one! Every exception thrown requires significant effort from the compiler and runtime. The runtime must collect information about the cause of the exception and where it was thrown, build the stack trace, prepare the necessary details, halt the program's normal execution, and transition to the exceptional flow,...

However, the speed of an application is far less important in modern software than its maintainability. It is much more important to have software that is easy to maintain, understand, modify, adapt, and cleanly coded than to focus on its performance.

Using ``null`` is, in most cases, much faster than using exceptions. While ``null`` makes the computer's job easier, it complicates the developer's work. For companies, it is far more expensive to deal with an unmaintainable application than to invest in new, more efficient hardware resources. This is also exactly the thing that the whole [Clean Architecture Book](#books) taught me. Even within the first 12 pages, Robert C. Martin emphasizes the importance of maintainability and provides diagrams illustrating how much money companies have lost due to unmaintainable software.

### Counterargument #3

**Some classes are not suitable for creating a null object. Consider the ``String`` class as an example. This is a ``final`` class which means it can't be extended or reimplemented by somebody else. In this case, the null object pattern is not applicable. When you have a method that is supposed to return a ``String``, you do not have the option to return a null class, such as a null string or something similar.**

It is not possible to use null objects everywhere but if it is not possible, then make it possible. In this specific case you could wrap or decorate the ``String`` class. Introduce your own class ``Text`` for instance which internally uses a ``String``. For the representation of ``null`` you could use your ``NoText`` class and for existing texts you could use the normal ``Text`` class. But never ever use ``null``. There is always an alternative to that!

## Conclusion

I hope you enjoyed my article about null references. Whether you're a fan or a critic of `null`, I’d like to leave you with one final thought: I believe it’s incredibly important to question common development practices that we usually take for granted. Some things, like the use of `null`, are often accepted by developers as a given. But just because something exists and is used by a lot of people doesn’t automatically mean that it’s a good thing. So next time you’re at the office, grab a coffee and chat with your fellow programmers — even about the overlooked aspects of programming. ☕😊

## Reference

### Books

- Robert C. Martin: _Clean Architecture: A Craftsman's Guide to Software Structure and Design (Robert C. Martin Series)_, Publisher: Pearson, Year: 2017, ISBN: 0134494164

- Andrew Hunt, David Thomas: _The Pragmatic Programmer: From Journeyman to Master_, Publisher: Addison-Wesley, Year: 2000, ISBN: 020161622X

### Videos

- Video from Yegor Bugayenko: _What is Wrong About NULL in OOP? (webinar #3)_, URL: https://www.youtube.com/watch?v=o3aNJX7AP3M, Last Access: 15th December 2024.

- Speech from Tony Hoare: _Null References The Billion Dollar Mistake_, QCon London 2009, URL: https://www.youtube.com/watch?v=YYkOWzrO3xg, Last Access: 15th December 2024.

### Blog Articles

- Blog-Article from Maximiliano Contieri: _NULL: The Billion Dollar Mistake_, URL: https://hackernoon.com/null-the-billion-dollar-mistake-8t5z32d6, Posted on April 14th 2020, Last Access: 15th December 2024.

- Blog-Article from Aphinya Dechalert: _How null references became the “Billion Dollar Mistake”_, URL: https://medium.com/madhash/how-null-references-became-the-billion-dollar-mistake-bcf0c0cc72ef, Posted on August 23rd 2023, Last Access: 15th December 2024.

- Blog-Article from Amanda Hinchman: _Null Pointer References: The Billion Dollar Mistake_, URL: https://hinchman-amanda.medium.com/null-pointer-references-the-billion-dollar-mistake-1e616534d485, Posted on April 2nd 2018, Last Access: 15th December 2024.

- Blog-Article from Yegor Bugayenko: _Why NULL is Bad?_, URL: https://www.yegor256.com/2014/05/13/why-null-is-bad.html, Posted on May 13rd 2014, Last Access: 15th December 2024.

- Blog-Article from Yegor Bugayenko: _Try. Finally. If. Not. Null._, URL: https://www.yegor256.com/2016/03/22/try-finally-if-not-null.html, Posted on March 22nd 2016, Last Access: 15th December 2024.

### Websites

https://en.wikipedia.org/wiki/Null_object_pattern 

https://en.wikipedia.org/wiki/Tony_Hoare

https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/ 

https://web.archive.org/web/20090628071208/http://qconlondon.com/london-2009/speaker/Tony+Hoare

https://en.wikipedia.org/wiki/Null_object_pattern#Java

https://www.geeksforgeeks.org/null-object-design-pattern/
