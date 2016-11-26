---
layout: post
title: "Chain of partial functions in Scala"
date:   2016-11-26
tags: scala partial-function
categories: scala
---

From the Scala doc:

```{scala}
PartialFunction[-A, +B] extends (A) ⇒ B
```

> A partial function of type PartialFunction[A, B] is a unary function where the domain does not necessarily include all values of type A. The function isDefinedAt allows to test dynamically if a value is in the domain of the function.

Let's say we have a number of partial functions:

```{scala}
val func1: PartialFunction[Int, Option[String]] = {
  case x if x >= 0 && x < 10 => Some("func one")
}

val func2: PartialFunction[Int, Option[String]] = {
  case x if x >= 10 && x < 20 => Some("func two")
}

val func3: PartialFunction[Int, Option[String]] = {
  case x if x >= 20 => Some("func three")
}

val otherwise: PartialFunction[Int, Option[String]] = {
  case x if x < 0 => None
}
```

We can pipe them to have a sort of 'multi-conditional' filter using `orElse` method API in this way:

```{scala}
val funcAll = func1 orElse func2 orElse func3 orElse otherwise
Seq(-1, 5, 15, 25).foreach { x: Int => println(funcAll (x)) }
```

or

```{scala}
val funcAll2 = Seq(func1, func2, func3, otherwise).reduce(_ orElse _)
Seq(-1, 5, 15, 25).foreach { x: Int => println(funcAll2 (x)) }
```

Where in both cases they return:

```{scala}
None
Some(func one)
Some(func two)
Some(func three)
```

Note that you can mix with `orElse` partial functions with different types, like:

```{scala}
val func1: PartialFunction[Int, Long] = {
  case x if x >= 0 && x < 10 => 3L
}

val func2: PartialFunction[Int, String] = {
  case x if x >= 10 && x < 20 => "func two"
}

val func3: PartialFunction[Int, Option[String]] = {
  case x if x >= 20 => Some("func three")
}

val otherwise: PartialFunction[Int, Option[String]] = {
  case x if x < 0 => None
}

val funcAll = func1 orElse func2 orElse func3 orElse otherwise
Seq(-1, 5, 15, 25).foreach { x: Int => println(funcAll (x)) }
```

that returns:

```{scala}
None
3
func two
Some(func three)
```
