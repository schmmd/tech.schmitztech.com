---
layout: post
title: Case Class Inheritance
author: Michael Schmitz
category: Scala
---

Case classes are great.  For you free you get:

1.  `equals`
2.  `hashCode`
3.  `toString`
4.  `copy`
5.  a companion object constructor

All this for *free*?  What's the drawback?

Well, one problem is that case classes do not allow for inheritance.

```scala
case class Point(x: Int, y: Int)
```

What if a run-of-the-mill `Point` isn't good enough and we need a
`ColoredPoint`.  Extending `Point` gives us a nasty warning.

```
scala> case class ColoredPoint(color: String, x: Int, y: Int) extends Point(x, y)
<console>:9: error: case class ColoredPoint has case ancestor Point, but case-to-case inheritance is prohibited. To overcome this limitation, use extractors to pattern match on non-leaf nodes.
```

What's wrong here?  One principal of case classes is that they define an
equality relation.  However, when extending `Point` with `ColoredPoint`, it's
no longer clear how to define `equals`.  Should `Point(0, 0)` equal
`ColoredPoint("red", 0, 0)`?  Should `ColoredPoint("red", 0, 0)` equal
`Point(0, 0)`?  It's not clear how this should be defined anymore.

Case class inheritance also caused caused problems in earlier version of Scala:

1.  http://www.scala-lang.org/old/node/3289
2.  http://www.scala-lang.org/old/node/1582

How should you share behavior between `Point` and `ColoredPoint`?  Simple: make a common ancestor.

```scala
abstract class Point {
  def x: Int
  def y: Int
}
case class 2DPoint extends Point
case class 2DColoredPoint extends Point
```

Here equality is clearly defined so we have no problem.
