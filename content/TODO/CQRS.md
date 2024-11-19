+++
title = "CQS and CQRS: Splitting the read from the write"
date = "2024-11-20"
description = ""
+++

This blog article mainly explains the concept of Command and Query Separation Principle (CQS). The last section of the article sheds a light on the Command-Query-Responsibility-Segregation (CQRS) and describes two patterns that adhere to this concept.

# Command and Query Separation Principle (CQS)

CQS was first formulated by Bertrand Meyer in 1994. Bertrand Meyer is the creator of the programming language Eiffel and therefore inventor of "Design by Contract" (DbC). In one of my recent blogposts I explained this concept in further detail - See: [Here](https://gersti.at/posts/liskov-substitution-principle/#design-by-contract-preconditions-postconditions-invariants).

CQS essentially says: you should separate your queries from your commands.
Or in other words: methods should not mix command operations and query operations.
What is a query and what is a command?

## Query

A query is a method that is asking some kind of question and retrieves a value back.

Let's consider a method that fetches some data from a database and returns the data to the caller of the method. When the method is called, it does not mutate the state of the application. It just performs a read operation on the database (= a so called query).

## Command

A command is a method that is performing some kind of action. Commands mutate the state of the application and do not return a value.

Let's consider a method that stores an object into a database. When the method is called, it just performs that write operation to the database and therefore mutate the state of the application but does not return any value.

# CQS violation

So the aim of this principle is to separate methods that mutate the state of the application from methods that do not mutate the state of the application.

- If a method would store something into a database (= change the state of the system) and also return some value, then the CQS is violated.
- If a method would return some data from a database and additionally change the underlying state of the system, then the CQS is violated.

Mixing up commands and queries in one method is not allowed! There is a famous quote that summarizes this rule quite well:

> Asking a question should not change the answer!<br>
> — <cite>Bertrand Meyer</cite>

If these rules are adhered to, the following correlation automatically emerges:
If a query is executed multiple times in a row, it must always return the same result, unless a command has been executed in the meantime.

# CQS and REST Endpoints

endpoints should also follow this rule.


# Advantages
makes it easier to reason about your code
that means: you know which methods are safe to call when you dont want to change the state of your application
the programmer gets an intuitive sense on whether the function mutates state or not. 

isolation of mutations
simpler code - methods are smaller, simpler and more focused
separation of concerns - splitting up complex code into smaller and isolated respnsibilities
easier testing - functions that do one thing are easier to be tested
Clear API design - one could use a naming convention to specify which method is a query and which one is a command. If you are designing a REST-API, I find it very crucial to design your endpoints that way.


# Exceptions - When can I violate the principle?


there are some edge cases where it makes sense to violate this principle.
For example Martin fowler says in his blog https://martinfowler.com/bliki/CommandQuerySeparation.html that the pop operation on a stack violates the principle since it retrieves an item of the stack but also changes the content of the stack so it mixes up a query and a command.


create operation in rest endpoint saves an object to the database but the frontend needs to know also the ID of the newly created resource in the database. so the endpoint sends back the id in the response body. this violated the principle.
Maybe you could write an extra endpoint which sends back the id of the latest added object from the database.

or

an endpoint who creates a new resource fails and sends back an error message to the client.

# Idempotent and Unidempotent Endpoints




# Command-Query-Responsibility-Segregation (CQRS)

CQS applys to functions/methods
CQRS applys to the architectural level


CQRS was first defined by Greg Young 

Talk from Udi Dahan: https://www.youtube.com/watch?v=Lw04HRF8ies

the first pitfall is not knowing cqrs and the second pitfall is overusing cqrs.

cqrs is an architectural style. other architectural styles are REST or MVC. ARCHITECTURAL STYLES ARE NOT BEST PRACTICES!! They are tools. Use them when and where they are appropriate.

# Reference

https://www.youtube.com/watch?v=zXJORh8K4Bw


https://martinfowler.com/bliki/CommandQuerySeparation.html

https://en.wikipedia.org/wiki/Command%E2%80%93query_separation

https://de.wikipedia.org/wiki/Command-Query-Responsibility-Segregation

https://www.youtube.com/watch?v=8-1fclzVm8w 


