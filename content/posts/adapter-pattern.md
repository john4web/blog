+++
title = "The Adapter Pattern"
date = "2024-07-15"
description = "Article explaining the Adapter Design Pattern by Gang of Four."
+++

This blog post offers an explanation of Adapter Design Pattern by Gang of Four. The examples are written in Java.

{{< figure src="/images/adapter-pattern/adapter.png" caption="Visualization of the Adapter Pattern (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/adapter))" >}}

# Intent
The original GoF Design Pattern book defines the Adapter Pattern as follows: **_Converts the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces._**

**In other words:** The adapter pattern is making two interfaces compatible, that aren't compatible.
 

# The problem

The behaviour of the Adaptee is the actual behavior that I want. But I cant get to that behavior using the actual interface that I have. How do I solve that?


# The pattern

{{< figure src="/images/adapter-pattern/plug.jpg" caption="Sketch of the adapter pattern. Two interfaces (the plug and the wall outlet) are incompatible. An adapter is used to make them compatible." >}}

{{< figure src="/images/adapter-pattern/adapter_pattern.jpg" caption="Example of the adapter pattern. The Client uses an Adapter to call the specificRequest() method of the Adaptee." >}}

In this example, the method `request()` is the method that the client **expects**. This method has a signature and the client expects to call that signature. The Adapter is taking in a `request()` call but it delegates it to another class (Adaptee) that follows a completely different contract.


The client (=user of the code) has something of a type `ITarget`. And this ITarget has a method called `request()`. And request is the standard that the client is used to using. But because the client wants to use something that has a completely different interface (the Adaptee), we can't just call `request()` because Adaptee does not have a method called `request()`. Adaptee has a method called `specificRequest()`. So what we do is to invoke the `request()` method but we invoke it on an Adapter so the Adapter follows the signature `request()` that we are used to using. So we can call `request()` and then count on the adapter to call the adaptee because the adapter has an adaptee so it is responsible for delegating the request down to the adaptee and then the adaptee can perform that specific action that we were originally interested in.

So in other words:  
The client for some reason wants to call the method `specificRequest()` but for some other reason it wants to call it using the particular signature of `request()`.

The client couples to an interface called `ITarget` that follows the particular signature of `request()`. And then we have a concrete implementation of this interface called Adapter which follows this signature and when the method `request()` is called, then we call (stupidly and blindly) the method `specificRequest` using this other signature. So we stick the Adapter in between the Client and the Adaptee so that they are compatible.

The Client calls the `specificRequest()` method **via** the request method. We call the method `request()` on the adapter but eventually that means that we'll call the method `specificRequest()` on the Adaptee.

# Intention

the intention of the Adapter pattern is to not change the underlying behaviour. The intention is not to add behaviour or remove behaviour or alter behavior. The intention is to somewhat _"blindly"_ pass on the request.  

You have two different types of signatures/interfaces and they do not connect. The intention is to make them interoperable.

This design pattern does not have intelligent logic. It just stupidly delegates the calls. It is kinda like a "Vermittler" or "Nachrichtenüberbringer" that delegates the messages 1:1.

# A simple wrapper
An Adapter is also known as a wrapper.
It wraps something so that you can adapt to a different interface.

Why it is a wrapper? You can see it perfectly in the code of the client who uses the adapter to execute the behaviour of the adaptee:
```java
class Client {
    ITarget target = new Adapter(new Adaptee());
    target.request();
}
```
In the second line of the above code snippet, the Adapter wraps the Adaptee.


# A practical example

Let's imagine in one of your projects, you are using a third-party library. In your code your are calling a specific method of that library with some specific method parameters. Suddenly the library gets updated and in the new version of the library, the order of the method parameters is changed. What you could do now is to write an adapter. You don't have to use the new method parameters order in your code. But you can use an adapter that calls the method with the new parameter order. In this way the client code does not have to change (btw. this reminds me on the open-closed principle).

Think of the word _"indirection"_. The adapter pattern is introducing a level of indirection. Instead of calling the thing that we want to call directly, we call a thing that calls the thing.

# An Interpreter
The Adapter Pattern can also be compared to an interpreter or mediator. Let’s assume there are two people who speak different languages. Because of this, they cannot understand each other. However, if you insert an interpreter between them, he/she can translate the languages so that both parties can communicate with each other. In this scenario, the interpreter would be the adapter, and the two people (who speak different languages) represent the client and the adaptee.

# Reference

## Video
- Video from Christopher Okhravi: _Adapter Pattern – Design Patterns (ep 8)_, URL: https://www.youtube.com/watch?v=2PKQtcJjYvc, Last Access: 12th October 2024.

## Website
- Refactoring Guru Website: _Adapter_, URL: https://refactoring.guru/design-patterns/adapter, Last Access: 12th October 2024.

