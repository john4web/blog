+++
title = "NULL: The Billion Dollar Mistake"
date = "2024-11-26"
description = "The downsides of using NULL-References explained"
+++

## Overview

This blog article explores the keyword `null`, a staple in every programmer's daily work. However, should we reconsider the use of null references in our software? Could encountering `null` in our code actually be harmful? This article aims to address these questions and provide insight into the implications of using `null`.

To understand the issue, we must travel back to 1965, when the null reference was invented by Tony Hoare. In a renowned lecture in 2009, while accepting the Turing Award, Hoare famously referred to the creation of null references as his *"billion-dollar mistake"*:

> I call it my billion-dollar mistake. It was the invention of the null reference in 1965. At that time, I was designing the first comprehensive type system for references in an object oriented language (ALGOL W). My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.<br>
> — <cite>[Tony Hoare](https://web.archive.org/web/20090628071208/http://qconlondon.com/london-2009/speaker/Tony+Hoare)</cite>

Null references are a common source of software errors, such as null pointer exceptions, which can cause system crashes and security issues. When Tony Hoare introduced null references, his motivation was practical: it seemed like a simple way to indicate that a reference didn’t have a valid value. However, this seemingly straightforward choice has had lasting consequences, resulting in decades of costly bug fixes and the need for complex security measures. Looking back, Hoare expressed regret for his decision, admitting that he couldn’t have foreseen the problems it would create. Another famous quote from him: 

> Null references have been a historically bad idea.<br>
> — <cite>[Tony Hoare](https://web.archive.org/web/20090628071208/http://qconlondon.com/london-2009/speaker/Tony+Hoare)</cite>

{{< figure src="/images/billion-dollar-mistake/oppenheimer.webp" caption="Robert Oppenheimer's regret over inventing the atomic bomb serves as a fitting analogy for Tony Hoare's regret over introducing the null reference (Source: Movie: Oppenheimer (2023) by Christoph Nolan).">}}

## Who is Tony Hoare?  

Tony Hoare is a British computer scientist who gained widespread recognition for his contributions to the field. He is best known for creating the _Quicksort algorithm_, one of the most efficient sorting methods, and _Hoare logic_, which allows the correctness of algorithms to be proven. He also developed a process algebra that influenced programming languages like Ada, Occam, and Go. In 1980, Hoare received the [Turing Award](https://de.wikipedia.org/wiki/Turing_Award), often referred to as the "Nobel Prize of Computer Science," for his work on the definition and design of programming languages. What do I wanna tell you with that? 

When someone of his stature in the computer science world, and the very person who introduced null references, calls it a mistake, it’s a statement that carries significant weight.

Tony Hoare is not the only famous person to view null references critically. As Hoare noted in his famous [2009 Turing Award lecture](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/), Edsger W. Dijkstra also regarded null references as a bad idea.

> If you have a null reference, then every bachelor who you represent in your object structure will seem to be married polyamocursly to the same person Null.<br>
> <cite>- Edsger W. Dijkstra</cite>

HIER DIESES ZITAT NOCH NÄHER ERKLÄREN

If you search the internet, you’ll find many people who view the use of null references as bad coding practice. One of them is [Yegor Bugayenko](https://www.yegor256.com/about-me.html).

In his [video on null references](https://www.youtube.com/watch?v=o3aNJX7AP3M), Yegor presents insightful, practical code examples that highlight the issues associated with working with null values, while also offering effective solutions. The following chapters summarize Yegor Bugayenko's video and his perspectives on null references:

-------------------

Yegor Bugayenko

Null references are really bad and they have to be avoided at all cost.

What is a null reference?

Example:
```java
public Employee getByName(String name){
int id = database.find(name);
if(id < 0){
    return null;
}
return new Employee(id);
}
```

instead of returning an instance of employee, we return null. This is terribly wrong.
Why?
1. it may cause many nullpointer exceptions in the code
For example:
```java
getByName("Jeffrey").salary();
```
If there is no employee with the name jeffrey in the database. then the above code throws a nullpointer exception.

2. It will be technically difficult to trace the causes of these exceptions.


3. null was introduced for languages that used pointers like C for instance. But in Java we are dealing with objects. In Object thinking you are not thinking about pointers and memory. You think about Objects which are living organisms/creatures. Object thinking is about Objects and calls to them and messages to them and their reaction to you - but null does not have any reaction since it is no object.


4. A lot of viruses are designed to exploit null references. That some software forgets to check the null pointers and they can go somewhere, where there is nothing in it. 

# Three possible alternatives to null references

1. Throw an Exception instead of returning NULL.

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

This also brings to mind the principle "Crash Early!" from the book *The Pragmatic Programmer* (page 120-121). The idea is that when your code encounters something that should have been impossible, the program is no longer reliable. Anything it does after that point becomes questionable, so it’s better to stop it as soon as possible. A program that has crashed typically causes less harm than one that continues to run in a broken state.

2. Return a Null-Object instead of returning NULL.

```java
public Employee getByName(String name){
int id = database.find(name);
if(id < 0){
    return new NoEmployee();
}
return new Employee(id);
}
```

The class NoEmployee implements the same interface as Employee but in a different way.

3. Using Optionals

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
    print "sorry"
}
```

An Optional is kind of a container including one or zero elements.

Using Optionals is not a good idea. Why? Because it is not a solution for null references. It is the same as null references. The optional container is not an Object by itself. It is just a temporary memory structure which holds a real object. You still have to check if the optional contains an employee or if it has no employee. It is something that is presented as the solution of the null reference problem but actually it is not. Es verschiebt das problem nur an einen späteren Ort im Code.


However the first two options (Custom Exceptions and Null-Objects) are the perfect solutions to the null reference problem. In most scenarios, just throwing a custom exception would be the best way to deal with it.


Null references violate the entire idea of encapsulation and it breaks/destroys the trust to the objects we are working with. It is a disease in the entire application. If we allow it to enter our software, then it grows and eventually will infect the entire code. Null references are telling me that I should not trust my objects. By using the word trust, I mean that i can rely on them to be self sufficient. I can rely on them that they will be objects for me. They will look like objects and they will behave like objects. When I am getting something that is not an Object, then the entire thinking is changed. Then I don't know anymore what is null and what not. I start to suspect all the object, all the variables, everything that I am working with that they are not good enough. That they are not objects anymore. I cannot know whether i can touch the variables because it could be null and then it breaks if i try to access it. objects are not real objects. they are not following the contract I am expecting. So you always have to check if an object is null or not.
So the code grows and the methods will become longer and longer because you have to check everywhere if an object is null before you can do something with it.
Null References are completely against the Object oriented mentality/thinking/paradigm.

what does "Null-Safe" mean?

Kotlin hat zumindest das Problem versucht zu lösen indem Variablen Nullable oder nicht nullable sein können. Durch das Fragezeichen sieht man dann sofort welchem Objekt man trauen kann und welchem Objekt man nicht trauen kann.


Counter Argument #1 

Some classes are not suitable for making null object. for example the string class. it is final. When you have a method that is supposed to return a string, then you have no option to return a null class. Like a null string or something like that. In this case you have to either return a string or something else.

Answer: This is a problem. It is not possible to use null objects everywhere. but if it is not possible - make it possible. for instance with a wrapper. You could wrap/decorate the String class. Introduce your own class "Text" for instance. For the representation of Null you could use your NoText class and for existing texts you could use the normal Text class.
But never ever use Null. Just because you could use null it does not mean that you have to do it. Dont let the designers of the java language change your way to think about objects and clean code. Even if java encourage you to use null - dont do it. Be stronger than java! 

Counter Argument #2 
Null is fast and exceptions are slow (Regarding code performance)

So the argument is that the first code snippet is way less performant than the second code snippet.

```java
try{
    Employee jeff = getByName("Jeffrey");
    print jeff.salary();
}catch(EmployeeNotFoundException ex){
    print "no jeffrey here"
}
```

```java
Employee jeff = getByName("Jeffrey");
if(jeff == null){
    print "no jeffrey here"
}else{
    print jeff.salary();
}
```
Answer:
That is true. Every exception thrown is a lot of effort for the compiler/runtime. To collect information about what was the cause of the exception, where it was thrown, to build the stack trace, to prepare this information, to stop the program execution, to go to the exceptional flow, ...

However the speed of the application is way less important of a modern software than the maintainability of it.

That is also exactly the thing that the Clean Architecture Book taught me. It is way more important to have a software that is easy to maintain, easy to understand, easy to form, easy to adapt, clean coded than the performance.

Using null is in most cases way faster than using exceptions. Using null will make the life of the computer more easy but it makes the life of the developer more harder and complexer. For companies it costs way more money to deal with an unmaintainable application than to buy new hardware resources that are more efficient/performant.


Counter Argument #4: Warum eine eigene Exception werfen wenn wir auch gleich die Nullpointerexception auftreten lassen können? Kommt ja auf das selbe raus, oder?

Antwort: Nein!
But whats the difference between a nullpointer exception raised by the system or a EmployeeNotFoundException thrown by us manually? There is a big dirreferenc ebecause the nullpointerexception is thrown by the machine by the jvm which is outside of our developer scope which means we cant control it. But the EmployeeNotFoundException is thrown by our objects so we are in charge, we can control it, we encapsulate this behaviour. Our objects control it and not the runtime environment. The EmployeeNotFoundException can contain a custom error message written by me which is alo important for understanding the whole situation. But it is important that you should not control the flow via exceptions! This is a trap that developers often fall for.

---------- 

So do I now have to define a null object for each domain entity in my system?

Yegor says: yes in some cases we have that and in some not. It depends on the situation. There may be scenarios where you dont need a null representation of an object because it is always there. 



There is only black and white. There is no grey color in between. If you use null, then the code is wrong and needs to be refactored. If you do not use null, then the code is clean.




https://en.wikipedia.org/wiki/Tony_Hoare






https://hackernoon.com/null-the-billion-dollar-mistake-8t5z32d6

https://medium.com/madhash/how-null-references-became-the-billion-dollar-mistake-bcf0c0cc72ef

https://hinchman-amanda.medium.com/null-pointer-references-the-billion-dollar-mistake-1e616534d485

https://en.wikipedia.org/wiki/Null_object_pattern 

Zusammenfassung aus diesem Video schreiben:
https://www.youtube.com/watch?v=o3aNJX7AP3M 

The billion dollar mistake (Tony Hoare)
https://www.youtube.com/watch?v=YYkOWzrO3xg 

Article:
https://www.yegor256.com/2014/05/13/why-null-is-bad.html

https://www.yegor256.com/2016/03/22/try-finally-if-not-null.html

https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare/ 