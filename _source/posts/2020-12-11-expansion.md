---
title: Implicit Conversions in Scala
date: 2020-12-11
tags: functional scala
image: //cacilhas.info/img/scala.png
permalink: /2020/12/implicit-conversions.html
---
[dev.to]: https://dev.to/cacilhas/implicit-conversions-in-scala-4dgb
[implicits]: https://www.scala-lang.org/files/archive/spec/2.13/07-implicits.html
[scala]: https://www.scala-lang.org/
[singleton]: https://docs.scala-lang.org/sips/42.type.html
[value-classes]: https://docs.scala-lang.org/overviews/core/value-classes.html

{:class="pull-right"} <img src="{{{ image }}}" alt="Scala"/>

{:class="mg-first"} [Scala][scala] (in version 2.13 while I write) has a
powerful conversion system based on its [implicits system][implicits].

It works by expanding implicit methods and classes into a more complex
structure, which would be way harder to code if it needs to be done explicitly.

We’re gonna talk about three ways to implement implicit conversions and their
expansions.

### Implicit methods

Take the double value sequence:

```scala
Seq(0.4, 1.8, 2.2)
```

In this case it’s a short list, but it could be hundreds larger. One needs only
their ceiling integer values, so one must a way to convert the values.

One can use an implicit method:

```scala
implicit def double2int(value: Double): Int = value.ceil.toInt

val values: Seq[Int] = Seq(0.4, 1.8, 2.2)
```

The value of `values` is:

```scala
Seq(1, 2, 3)
```

It’s expanded to:

```scala
def double2int(value: Double): Int = value.ceil.toInt

val values: Seq[Int] = Seq(double2int(0.4), double2int(1.8), double2int(2.2))
```

### Implicit classes

Implicit classes are the way to inject methods into existent types.

For instance, let’s create a string method to generate the XML node from it:

```scala
implicit class XMLString(value: String) {
  def toXML: Option[NodeSeq] = Try(XML loadString value).toOption
}

val source = getDataFromOutsideSource() // : String

val node = source.toXML
```

It expands to:

```scala
class XMLString(value: String) {
  def toXML: Option[NodeSeq] = Try(XML loadString value).toOption
}

object XMLString extends (String => XMLString) {
  def apply(value: String): XMLString = new XMLString(value)
}

val source: String = getDataFromOutsideSource()

val node: Option[NodeSeq] = XMLString(source).toXML
```

Note that everytime `.toXML` is called, a new `XMLString` is created.

### Implicit value classes

[Value classes][value-classes] are lightweight Scala resources, which **don’t**
create new instance each call. Instead, every value class uses a companion
object to envelope the methods, and calls them from it.

Consider the following example, a method to determine whether a double is
integral:

```scala
implicit class IntegralDouble(val value: Double): extends AnyVal {
  def isIntegral: Boolean = value % 1 == 0
}

val value = getSomeFloatPointValue() // : Double

if (value.isIntegral)
  doSomeMathWith(value)
```

Which expands to:

```scala
class IntegralDouble(val value: Double): extends AnyVal {
  def isIntegral: Boolean = IntegralDouble isIntegral$expansion value
}

object IntegralDouble {
  def isIntegral$expansion(value: Double): Boolean = value % 1.0 == 0.0
}

val value: Double = getSomeFloatPointValue()

if (IntegralDouble.isIntegral$expansion(value))
  doSomeMathWith(value)
```

No instance is created on `.isIntegral` call.

Note: value classes are tagged by inheriting `AnyVal`, and need a value of type
`* <: AnyVal`, i.e., `Boolean`, `Byte`, `Char`, `Double`, `Float`, `Int`,
`Long`, `Short`, `Unit`, and their [literal-based singleton types][singleton].

-----

{:class="small"} Also in [DEV Community 👩‍💻👨‍💻][dev.to].
