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

If the Animal can do certain things, then the Cat must also be able to do all these things. All subclasses of Animal must be able to do these things.
But the subclasses may also be able to do more things than the base class. And by more things, I mean more **specific** things. The Cat class may contain the ``meow()`` method. But the Animal class does not contain the ``meow()`` method because it is a specific behavior of a cat. But that's not a problem, because the Cat class can still be used instead of the Animal class without changing the behavior of the program. Because a cat can do everything that an animal can do in exactly the same way. So the LSP holds.


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