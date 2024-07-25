+++
title = "The Builder Pattern"
date = "2024-01-24"
description = "Article explaining the Builder Design Pattern by Gang of Four."
+++

This blog post offers an explanation of Builder Design Pattern by Gang of Four. The examples are written in java.

## Intent

_"Separate the construction of a complex object from its representation so that the same construction process can create different representations."_

{{< figure src="/blog/images/builder-pattern/builder.png" caption="Visualization of the Builder Pattern (Source: [Refactoring-Guru](https://refactoring.guru/design-patterns/builder))" >}}

## Overview

The builder pattern is classified as "creational pattern" which means that it deals with the creation of an object. Creational design patterns abstract the instantiation process. They help to make a system independent of how its objects are created, composed and represented.

Builder lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

Simply put: Instead of using a constructor to create a new object, you can use the builder pattern instead. This could solve some problems. For instance if you have big constructors with loads of parameters. Or if you have to overload constructors many times. Or if you have constructors where you don't want to initialize every single member variable of a class by calling the constructor (= optional parameters).

## The Problem that the builder pattern solves

Imagine a complex object that requires laborious, step-by-step initialization of many fields and nested objects. Such initialization code is usually buried inside a monstrous constructor with lots of parameters. Or even worse: scattered all over the client code.

For example, let’s think about how to create a House object. To build a simple house, you need to construct four walls and a floor, install a door, fit a pair of windows, and build a roof. But what if you want a bigger, brighter house, with a backyard and other goodies (like a heating system, plumbing, and electrical wiring)?

You can create a giant constructor right in the base House class with all possible parameters that control the house object. 

In most cases most of the parameters will be unused, making the constructor calls pretty ugly. For instance, only a fraction of houses have swimming pools, so the parameters related to swimming pools will be useless nine times out of ten.

## When to use the builder pattern

- get rid of huge constructors

Use the Builder pattern to get rid of a “telescoping constructor”.

 Say you have a constructor with ten optional parameters. Calling such a beast is very inconvenient; therefore, you overload the constructor and create several shorter versions with fewer parameters. These constructors still refer to the main one, passing some default values into any omitted parameters.

```java
class Pizza {
    Pizza(int size) { ... }
    Pizza(int size, boolean cheese) { ... }
    Pizza(int size, boolean cheese, boolean pepperoni) { ... }
    // ...
```

Creating such a monster is only possible in languages that support method overloading, such as C# or Java.

**The Builder pattern lets you build objects step by step, using only those steps that you really need. After implementing the pattern, you don’t have to cram dozens of parameters into your constructors anymore.**

- Use the Builder pattern when you want your code to be able to create different representations of some product (for example, stone and wooden houses).

 The Builder pattern can be applied when construction of various representations of the product involves similar steps that differ only in the details.

The base builder interface defines all possible construction steps, and concrete builders implement these steps to construct particular representations of the product. Meanwhile, the director class guides the order of construction.


- Use the Builder to construct Composite trees or other complex objects.

 The Builder pattern lets you construct products step-by-step. You could defer execution of some steps without breaking the final product. You can even call steps recursively, which comes in handy when you need to build an object tree.

A builder doesn’t expose the unfinished product while running construction steps. This prevents the client code from fetching an incomplete result.

## Pattern

```java
public class FullName{

 private String firstName;
 private String lastName;

  public FullName(String firstName, String lastName){
    this.firstName = firstName;
    this.lastName = lastName;
  }

  public Builder builder(){
    return new Builder();
  }

  public class Builder{
        String fn;
        String ln;

        public Builder firstName(String fn){
            this.fn = fn;
            return this;
        }

        public Builder lastName(String ln){
            this.ln = ln;
            return this;
        }

        public FullName build(){
            return new FullName(fn, ln);
        }

  }

}
```

```java
FullName fullName = FullName.builder()
                      .firstName("Johannes")
                      .lastName("Gerstbauer")
                      .build();
```


Besseres Beispiel hier mit required attributes:


```java
public class User
{
	//All final attributes
	private final String firstName; // required
	private final String lastName; // required
	private final int age; // optional
	private final String phone; // optional
	private final String address; // optional

	private User(UserBuilder builder) {
		this.firstName = builder.firstName;
		this.lastName = builder.lastName;
		this.age = builder.age;
		this.phone = builder.phone;
		this.address = builder.address;
	}

	//All getter, and NO setter to provde immutability
	public String getFirstName() {
		return firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public int getAge() {
		return age;
	}
	public String getPhone() {
		return phone;
	}
	public String getAddress() {
		return address;
	}

	@Override
	public String toString() {
		return "User: "+this.firstName+", "+this.lastName+", "+this.age+", "+this.phone+", "+this.address;
	}

	public static class UserBuilder
	{
		private final String firstName;
		private final String lastName;
		private int age;
		private String phone;
		private String address;

		public UserBuilder(String firstName, String lastName) {
			this.firstName = firstName;
			this.lastName = lastName;
		}
		public UserBuilder age(int age) {
			this.age = age;
			return this;
		}
		public UserBuilder phone(String phone) {
			this.phone = phone;
			return this;
		}
		public UserBuilder address(String address) {
			this.address = address;
			return this;
		}
		//Return the finally consrcuted User object
		public User build() {
			User user =  new User(this);
			validateUserObject(user);
			return user;
		}
		private void validateUserObject(User user) {
			//Do some basic validations to check
			//if user object does not break any assumption of system
		}
	}
}
```


```java
public static void main(String[] args) 
{
	User user1 = new User.UserBuilder("Lokesh", "Gupta")
	.age(30)
	.phone("1234567")
	.address("Fake address 1234")
	.build();

	System.out.println(user1);

	User user2 = new User.UserBuilder("Jack", "Reacher")
	.age(40)
	.phone("5655")
	//no address
	.build();

	System.out.println(user2);

	User user3 = new User.UserBuilder("Super", "Man")
	//No age
	//No phone
	//no address
	.build();

	System.out.println(user3);
}
```



## Directors

You can go further and extract a series of calls to the builder steps you use to construct a product into a separate class called director. The director class defines the order in which to execute the building steps, while the builder provides the implementation for those steps.

Having a director class in your program isn’t strictly necessary. You can always call the building steps in a specific order directly from the client code. However, the director class might be a good place to put various construction routines so you can reuse them across your program.

In addition, the director class completely hides the details of product construction from the client code. The client only needs to associate a builder with a director, launch the construction with the director, and get the result from the builder.

Unter anderem Beispiel von hier nehmen: https://www.youtube.com/watch?v=MaY_MDdWkQw 


# Using Interfaces
Todo: show example of builders with interfaces (https://refactoring.guru/design-patterns/builder/java/example)

# Project Lombok
builder annotation von Lombok erklären

## Reference

Book: Design Patterns - Elements of Reusable Object-Oriented Software (1995 Addison-Wesley) by 
Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides

https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/

https://refactoring.guru/design-patterns/builder

https://medium.com/javarevisited/all-the-16-lombok-annotations-explained-in-a-4-minute-article-926f71934ec6

https://refactoring.guru/design-patterns/builder/java/example 

https://www.youtube.com/watch?v=MaY_MDdWkQw 




