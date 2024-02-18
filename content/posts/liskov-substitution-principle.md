+++
title = "Liskov substitution principle in depth"
date = "2024-01-24"
description = "Article explaining the liskov substitution principle in depth."
+++

This blog post offers a detailed explanation of the liskov substitution principle and gives an overview of the rules you should follow as a software engineer when dealing with inheritance in object-oriented programming to produce clean code. The following is a summary of some [sources](#reference) I have read however also includes my own opinion on that topic. Code examples are written in Java.

## Overview

The Liskov Substitution Principle (LSP) was first introduced by [Barbara Liskov](https://en.wikipedia.org/wiki/Barbara_Liskov) in 1987 in her article ["Data Abstraction and Hierarchy"](https://www.cs.tufts.edu/~nr/cs257/archive/barbara-liskov/data-abstraction-and-hierarchy.pdf) and is one of the five SO**L**ID design principles of object-oriented programming.

{{< figure src="/blog/images/liskov-substitution-principle/barbara_liskov.jpg" caption="Barbara Liskov (Source: [Wikipedia](https://commons.wikimedia.org/wiki/File:Barbara_Liskov_computer_scientist_2010.jpg))." width="200" height="300" >}}

The original definition of this principle is as follows:  
**_If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2, then S is a subtype of T._**

Ok, that sounds pretty complicated. So what does that even mean?

The LSP states that a program using objects of a base class ``T`` should be able to work correctly with objects of its derived class ``S`` without altering the program. In other words, all subclasses must behave in the same way as the base class.

Simply put:
If ``S`` is a Subtype of ``T``, then objects of type ``T`` may be replaced by objects of type ``S``.

Wherever you are using the base type, you should be able to use the subtype instead without causing any unwanted behavior in the program. That means: _"Subtypes must be **substitutable** for their base types."_

Let's take a closer look at that concept with an example:  
Assume that class S inherits from class ``T``.

{{< figure src="/blog/images/liskov-substitution-principle/inheritance_1.svg" caption="Class S inherits from class T." width="200" height="300" >}}

And then, let's imagine that we have written a program where we have used class ``T`` in multiple places in the code and created instances of it ``T t = new T()``. Now, we need to ensure that we can replace instances of class ``T`` with instances of class ``S`` at all these points without changing the program's behavior.

So instead of

``T t = new T()``

you should be able to use

``T t = new S()``

and then the program should behave exactly the same way as before.  
If this is the case, then the LSP holds. ``S`` was substituted for ``T``!

## Animals, Cats, Dogs and Tables
Let's take a step back and address a more fundamental question:  
_How should you actually use inheritance in programming?_

Consider a classic example where the subclass ``Cat`` extends the base class ``Animal``.

{{< figure src="/blog/images/liskov-substitution-principle/inheritance_2.svg" caption="Class Cat inherits from class Animal." width="200" height="300" >}}

If the ``Animal`` can do certain things, then the ``Cat`` must also be able to do all these things. All subclasses of Animal must be able to do these things.
But the subclasses may also be able to do more things than the base class. And by more things, I mean more **specific** things. The ``Cat`` class may contain the ``meow()`` method. But the ``Animal`` class does not contain the ``meow()`` method because it is a specific behavior of a cat. But that's not a problem, because the ``Cat`` class can still be used instead of the Animal class without changing the behavior of the program. Because a cat can do everything that an animal can do in exactly the same way. So the LSP holds.

* This means: a ``Cat`` is a subset of an ``Animal``.
* This means: a ``Cat`` **IS** an ``Animal``.
* This means: an ``Animal`` is not necessarily a ``Cat``. An ``Animal`` can also be a dog, for example.

This is why we speak of an _is-a_ relation in programming when we talk about inheritance.

For example: a table is not an animal and therefore does not inherit from animal and therefore is not a subset of ``Animal``.

{{< figure src="/blog/images/liskov-substitution-principle/circle.svg" caption="Cat is a subset of Animal because it can do everything an animal can do (and more). Table is not a subset of Animal.">}}

## LSP Definition
Let's take a closer look at the original definition from Barbara Liskov (slightly simplified):

_If for each object Os of type S there is an object Ot of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when Os is substituted for Ot, then S is a subtype of T._

The following diagram tries to explain this definition in a visual way:

{{< figure src="/blog/images/liskov-substitution-principle/definition.svg" caption="LSP definition explained as a diagram." >}}

* Os is an instance of class S.  
* Ot is an instance of class T.  
* The program P uses Os.  
* The Program P' uses Ot.  
If you use Os instead of Ot in Program P, and the behaviour of Program P is unchanged, then the LSP holds.  
That means P' must behave exactly like P.

## Practical Example: Violated LSP

When is the LSP actually violated?  
_If you are inheriting from the base class without making the subclass substitutable for the base class, then you are breaking the LSP._  
In the following, a classic example of a program with violated LSP is shown. Consider we have a class Rectangle ad a class Square where Square extends Rectangle.

{{< figure src="/blog/images/liskov-substitution-principle/rectangle.svg" caption="Class Square inherits from class Rectangle." >}}

In this example, square is not a proper subtype of rectangle, because the height and width of the rectangle are independently mutable. In contrast the height and width of the square must change together. Since the user believes it is communicating with a rectangle, it could easily get confused.

```java
Rectangle r = _________
r.setHeight(5);
r.setWidth(2);
assert(r.area() == 10)
```

If the blank line above produced a square, then the assertion would fail.

However let’s take a deeper look into the java code of this example. A rectangle can set its width and height independently. In contrast, whenever the width/height of a square is set, the height/width is changed accordingly. 
the setWidth() and setHeight() functions are inappropriate for a Square, since the width and height of a square are identical. So we notice that something goes wrong here. However let’s ignore this indication that there is a problem and sidestep this problem by overriding the setWidth and setHeight methods.

```java
class Rectangle{

  int width;
  int height;

  setHeight(height){
    this. height = height;
  }

  setWidth(width){
    this.width = width;
  }

  area(){
    return width * height;
  }

}
```

```java
class Square extends Rectangle{

  @Override
  setHeight(height){
    this. height = height;
    this.width = height;
  }

  @Override
  setWidth(width){
    this.width = width;
    this. height = width;
  }

}
```

When using the code now, there is unexpected behaviour when using the square instead of the rectangle. Hence, the LSP breaks! You should never receive anything unexpected from a subclass.

```java
Rectangle r = new Rectangle()
r.setHeight(5);
r.setWidth(2);
System.out.println(r.area()); // 10
```

```java
Rectangle r = new Square()
r.setHeight(5);
r.setWidth(2);
System.out.println(r.area()); // 4
```

A solution to this problem could be introducing a Shape class, where Rectangle and square are inheriting from:

{{< figure src="/blog/images/liskov-substitution-principle/shape.svg" caption="Square and Rectangle both inherit from Shape." >}}

Or an even better solution: **use composition instead of inheritance**.

## Method Overriding

In the previous example, the LSP was violated because the methods setWidth() and setHeight() were overwritten in the subclass. As the two methods in the subclass behaved differently, Square could no longer be used instead of Rectangle without changing the behaviour of the program. You may be asking yourself:

**Is method overriding always a violation of Liskov Substitution Principle?**  
-> Answer: "it depends!"


## Reference
https://de.wikipedia.org/wiki/Design_by_Contract

Design By Contract in Java - Seminarbericht WS06/07 - Matthias Hausherr

Program Development in Java: Abstraction, Specification, and Object-Oriented Design,

Clean Architecture: A Craftsman's Guide to Software Structure and Design: A Craftsman's Guide to Software Structure and Design (Robert C. Martin Series)

https://www.baeldung.com/java-liskov-substitution-principle

https://www.baeldung.com/java-method-overload-override 

University Lecture called “Software Design Methods” by
Ing. Dr. Hans J. Prüller, B.Sc.
Personal website: https://hansprueller.lbs-logics.com/ 

https://www.youtube.com/watch?v=ObHQHszbIcE 

https://www.youtube.com/watch?v=dJQMqNOC4Pc&t=176s 

https://www.youtube.com/watch?v=bVwZquRH1Vk&t=144s 

https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)#Covariant_method_return_type 




# H1

## H2

### H3

#### H4

##### H5

###### H6

## Paragraph

Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? Quiatem. Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.

## Links

This is a [internal link](/posts/emoji-support) to another page. [This one](https://www.gohugo.io) points to a external page nad will be open in a new tag.

## Blockquotes

The blockquote element represents content that is quoted from another source, optionally with a citation which must be within a `footer` or `cite` element, and optionally with in-line changes such as annotations and abbreviations.

#### Blockquote without attribution

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use _Markdown syntax_ within a blockquote.

#### Blockquote with attribution

> Don't communicate by sharing memory, share memory by communicating.<br>
> — <cite>Rob Pike[^1]</cite>

## Tables

Tables aren't part of the core Markdown spec, but Hugo supports them out-of-the-box.

| Name  | Age |
| ----- | --- |
| Bob   | 27  |
| Alice | 23  |

#### Inline Markdown within tables

| Italics   | Bold     | Code   |
| --------- | -------- | ------ |
| _italics_ | **bold** | `code` |

## Code Blocks

#### Code block with backticks

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8" />
        <title>Example HTML5 Document</title>
    </head>
    <body>
        <p>Test</p>
    </body>
</html>
```

#### Code block indented with four spaces

    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Example HTML5 Document</title>
    </head>
    <body>
      <p>Test</p>
    </body>
    </html>

#### Code block with Hugo's internal highlight shortcode

{{< highlight html >}}

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
{{< /highlight >}}

## List Types

#### Ordered List

1. First item
2. Second item
3. Third item

#### Unordered List

-   List item
-   Another item
-   And another item

#### Nested list

-   Fruit
    -   Apple
    -   Orange
    -   Banana
-   Dairy
    -   Milk
    -   Cheese

#### Foot Notes

Check it[^2] at the end[^3] of this text[^4].

## Other Elements — abbr, sub, sup, kbd, mark

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.

[^1]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.
[^2]: A footnote.
[^3]: Another one.
[^4]: Cool, right?