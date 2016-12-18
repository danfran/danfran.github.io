---
layout: post
title: "How to add implicit methods in Scala and implicitly check for them"
date:   2016-11-19
categories: scala
tags: implicit
---
Let's start with this simple example from REPL:

```scala
scala> 30.divideMeBy(6)
<console>:11: error: value divideMeBy is not a member of Int
       30.multiplyMeBy(6)
         ^
```

As expected the `Int` value `30` has not any method `multiplyMeBy`. In Scala it is possible to extend it `dynamically` in a very simple way.
Let's create a class that accepts in the constructor an `Int` value:

```scala
scala> implicit class MyAmazingIntExpander(val i: Int) {
     |   def multiplyMeBy(d: Int): Int = i * d
     | }
defined class MyAmazingIntExpander
```

and repeat again the previous failing operation:

```scala
scala> 30.multiplyMeBy(6)
res9: Int = 180
```

nice :) we have extended the functionality of the primitive `Int` class.
How can we check if there is an `implicit` conversion from `Int` to `MyAmazingIntExpander`?

```scala
scala> implicitly[Int => MyAmazingIntExpander]
res20: Int => MyAmazingIntExpander = <function1>
```
