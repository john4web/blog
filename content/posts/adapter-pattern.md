+++
title = "The Adapter Pattern"
date = "2024-01-24"
description = "Article explaining the Adapter Design Pattern by Gang of Four."
+++




{{< figure src="/images/adapter-pattern/adapter.png" caption="Visualization of the Adapter Pattern (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/adapter))" >}}

# Intent
Converts the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

In other words: Making two interfaces compatible, that aren't compatible.
 

# The Problem

The behaviour of the Adaptee is the actual behavior that I want. But I cant get to that behavior using the actual interface that I have.


# The pattern


{{< figure src="/images/adapter-pattern/adapter1.jpg" caption="" >}}

{{< figure src="/images/adapter-pattern/adapter2.jpg" caption="" >}}




In this example, the method `request()` is the method that the client **expects**. This method has a signature and the client expect to call that signature. The Adapter is taking in a `request()` call but it delegates it to another class (Adaptee) that follows a completely different contract.


The client (=user of the code) has something of a type `ITarget`. And this ITarget has a method called `request`. And request is the standard that the client is used to using. But because the client wants to use something that has a completely different interface (the Adaptee), we can't just call `request()` because Adaptee does not have a method called `request()`. Adaptee has a method called `specificRequest()`. So what we do is to invoke the `request()` method but we invoke it on an Adapter so the Adapter follows the signature (`request()`) that we are used to using. So we can call request() and then count on the adapter to call the adaptee because the adapter has an adaptee so it is responsible for delegating the request down to the adaptee and then the adaptee can perform that specific action that we were originally interested in.

So the client for some reason wants to call this method specificRequest() but for some other reason it wants to call it using this particular signature of request().

The client wants to call the method specificRequest but it wants to do so by using the signature request().

The client couples to an interface called ITarget that follows the particular signature of request(). And then we have a concrete implementation of this interface called Adapter which follows this signature and when this method `request()` is called, then we call (stupidly and blindly) the method `specificRequest` and then use this other signature. So we Stick the Adapter in between the Client and the adaptee so that they are compatible.


The Client calls the specificRequest method **via** the request method. 

We call the method request() on the adapter but eventually that means that we'll call the method specificRequest() on the Adaptee.

# Intention

the intention of the Adapter pattern is to not change the underlying behaviour. The intention is not to add behaviour or remove behaviour or alter behavior. The intention is to somewhat blindly pass on the request. You have two different types of signatures/interfaces and they do not connect. The intention is to make them interoperable.

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
Here in the second line, the Adapter wraps the Adaptee.


# A practical example

Let's imagine in one of your projects, you are using a third-party library. In your code your are calling a specific method of that library with some specific method parameters. Suddenly the library gets updated and in the new version of the library, the order of the method parameters is changed. What you could do now is to write an adapter. You don't have to use the new method parameters order in your code. But you can use an adapter that calls the method with the new parameter order. In this way the client code does not have to change. (Btw. this reminds me of the open-closed principle).

Think of the word indirection. The adapter pattern is introducing a level of indirection. Instead of calling the thing that we want to call directly, we call a thing that calls the thing.

# Ein Dolmetscher
Man kann das Adapter Pattern auch mit einem Dolmetscher oder Vermittler vergleichen. Nehmen wir an, es gibt zwei personen, die unterschiedliche Sprachen sprechen. Dadurch können sie sich gegenseitig nicht verstehen. Wenn man aber einen Dolmetscher dazwischen schaltet, dann kann er die Sprachen übersetzen, sodass die beiden Parteien miteinander kommunizieren können. In diesem Szenario wäre der Übersetzer der Adapter, und die beiden Personen die unterschiedliche Sprachen sprechen sind der Client und der Adaptee.

# Reference 

https://www.youtube.com/watch?v=2PKQtcJjYvc

https://refactoring.guru/design-patterns/adapter