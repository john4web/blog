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
But what is a query and what is a command?

## Query

A query is a method that is asking some kind of question and retrieves a value back.

Let's consider a method that fetches some data from a database and returns the data back to the caller of the method. When the method is called, it does not mutate the state of the application. It just performs a read operation on the database (= a so called query).

## Command

A command is a method that is performing some kind of action. Commands mutate the state of the application but do not return any value.

Let's consider a method that stores an object into a database. When the method is called, it just performs the write operation to the database and therefore it mutates the state of the application. However the method does not return any value.

# CQS violation

So the aim of this principle is to separate methods that mutate the state of the application from methods that do not mutate the state of the application.

- If a method would store something into a database (= change the state of the system) and additionally also return some value, then the CQS is violated.
- If a method would return some data from a database and additionally also change the underlying state of the system, then the CQS is violated.

Mixing up commands and queries in one method is not allowed! There is a famous quote that summarizes this rule quite well:

> Asking a question should not change the answer!<br>
> — <cite>Bertrand Meyer</cite>

If these rules are adhered to, the following correlation automatically emerges:
If a query is executed multiple times in a row, it must always return the same result, unless a command has been executed in the meantime.

# CQS and REST Endpoints

This concept can also be effectively applied to working with REST endpoints. One could say that the following convention should be adhered to: define endpoints that either perform a query operation or a command operation, but not both at the same time. When thinking about CRUD functionality, CREATE, UPDATE, and DELETE are command operations, whereas READ is a query operation.

One could also state that with the following HTTP methods in REST:

- **GET** (Requests a resource from the server)  
- **POST** (Creates a new resource on the server)  
- **PUT** (Modifies the entire resource on the server)  
- **DELETE** (Deletes a resource from the server)  
- **PATCH** (Modifies only parts of the resource on the server)  

Only the **GET** method corresponds to a query operation. All other methods, such as POST, PUT, DELETE, and PATCH, are command operations because they change the state of the application. 

Therefore, if one wishes to apply the Command-Query Separation (CQS) principle to REST endpoints, it must be ensured that an endpoint does not both return data and modify the state in any way. This should then be split into two separate endpoints.


# Advantages of CQS

1. Makes it easier to reason about your code --> You get an intuitive sense on whether the function mutates state or not. You know immediately which methods are safe to call when you dont want to change the state of your application.

2. Isolation of mutations

3. Simpler code --> methods are smaller, simpler and more focused

4. Separation of concerns --> splitting up complex code into smaller and isolated respnsibilities

5. Easier testing --> functions that do one thing are easier to be tested

6. Clear API design --> one could use a naming convention to specify which method is a query and which one is a command. If you are designing a REST-API, I find it very crucial to design your endpoints that way.

7. Not adhering to CQS could be dangerous --> Imagine someone is calling a GET Endpoint to perform a read operation however behind the scenes the state of the application gets changed. It is not obvious for the caller that this happens and probably the caller's intent was not to change state by calling this endpoint.


# Exceptions - When can I violate the principle?

There are some edge cases where a violation of this principle could make sense.

Martin Fowler presented a popular example in his [Online-Blog](https://martinfowler.com/bliki/CommandQuerySeparation.html). He states that  the _pop_ operation on a stack violates the principle since it retrieves an item of the stack but also changes the content of the stack so it mixes up a query and a command.

Another example recently came up for me at work. I needed to create a new endpoint for saving a new object into the database. So, I created a classic POST endpoint. As usual, the resource to be saved is sent in the request body, and the logic in the backend inserts this resource into the database. Naturally, the entity in the database has an ID defined as primary key, which is auto-generated. So far, so good. However, shortly thereafter, a frontend developer from my team approached me, explaining that the application's frontend needs the ID of the newly created object. It needs the ID because it would subsequently request this specific object from the backend by calling the GetByID endpoint, for which the frontend needs the ID, obviously. The frontend developer thought, that the POST endpoint for creating the resource could also include the newly created object in the response body, including its ID. That way, it could be processed immediately in the frontend. This approach, as I found out online, is not uncommon.

However, I was not at all convinced by this suggestion to combine both functionalities in the POST endpoint, as it is a clear violation of the CQS principle. I thought a possible solution to this problem could be to create an additional endpoint that always returns the last object saved by the user. But even this solution is not ideal, as the program needs to check the `create_date`, introducing additional and unnecessary complexity.

Another question is how endpoints should generally handle exceptions and errors. If a command operation throws an exception, should the endpoint return an error message in the response? This, too, would be a violation of the principle. On the other hand, the frontend needs to inform the user when something goes wrong and also what exactly went wrong. That is not possible if the endpoint does not return an error message in the response.

As you can see, there are cases where applying the CQS principle can be questioned. In the end, each developer must individually decide in each situation which approach to take and whether to violate or apply the principle. In general, software design principles are not rules that must be followed. Rather, they are suggestions aimed at improving the code. However, there are always principles that can contradict each other. The important thing is to think carefully and make a conscious decision on which way to go. Engaging in discussions with like-minded developers can also be helpful in this process.

# Idempotent and Unidempotent Endpoints




# Command-Query-Responsibility-Segregation (CQRS)

As already explained, CQS applys to functions/methods.
CQRS on the other hand is the big brother of CQS and applys to the architectural level of applications. It treats "reading" and "writing" as entirely separate subsystems. CQRS was first defined by Greg Young in 2010.

NDC Conference in Oslo 22-26 May 2023 

Udi Dahan explains in his presentation [CQRS pitfalls and patterns](https://www.youtube.com/watch?v=Lw04HRF8ies) two different CQRS patterns.
The standard CQRS Pattern ....



cqrs is an architectural style. Architectural Styles are not best practices. They are tools. Use them when and where they are appropriate.


{{< figure src="/images/cqrs/CQRS1.jpg" caption="Standard CQRS." >}}

{{< figure src="/images/cqrs/CQRS2.jpg" caption="Eventually Consistent CQRS." >}}

# Reference

https://www.youtube.com/watch?v=zXJORh8K4Bw

https://www.youtube.com/watch?v=Lw04HRF8ies

https://martinfowler.com/bliki/CommandQuerySeparation.html

https://en.wikipedia.org/wiki/Command%E2%80%93query_separation

https://de.wikipedia.org/wiki/Command-Query-Responsibility-Segregation

https://www.youtube.com/watch?v=8-1fclzVm8w 




??? https://www.youtube.com/watch?v=85YbMEb1qkQ
??? https://www.youtube.com/watch?v=DQ3D_mplIgY