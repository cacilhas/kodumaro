![Scala](//cacilhas.info/img/scala.png)

I was around with a cyclic reference issue in [Scala](https://www.scala-lang.org/): [JSON](https://www.json.org/) representation.

I was using [Gson](https://github.com/google/gson) for serialising and deserialising (ain’t going in details), but how to represent the models inside the language ecosystem?

I could follow the Gson documentation and use [pojos](https://pt.wikipedia.org/wiki/Plain_Old_Java_Objects) ([case classes](https://docs.scala-lang.org/tour/case-classes.html) are great for it), but I was in need of something more flexible.

I decided to use language primitives, like integers, strings, big numbers, and so one.

So I created some types for deal with that:

    type unroll[+A] = A
    type JValue = unroll[_ <: Any]
    type JArray = Seq[JValue]
    type JObject = String Map JValue

Not bad, I can hold complex objects in `JValue` variables:

    val data: JValue = Map(
      "x" -> 3,
      "y" -> 4,
      "name" -> "test",
      "list" -> Seq("zero", 1, 2, 3, true),
    )

But odd things started to happen, like:

    val element: JValue = <data>This is an XML node</data>

It should crash! But doesn’t, since `NodeSeq` is `Any`’s subclass.

I needed something stricter.

### Enter Dotty

Now I’ve tried [Scala 3, codename Dotty](https://dotty.epfl.ch/).

My first unsuccessful attempt was:

    type JNull = Unit
    type JValue = Int | BigDecimal | String | Boolean | JNull | Seq[JValue] | String Map JValue

And Dotty put a damper on my plans:

> Illegal cyclic type reference: alias Map\[Int | BigDecimal | String | Boolean | JNull | Seq\[JValue\] | String, JValue\] of type JValue refers back to the type itself.

Then I hit [this strange solution](https://users.scala-lang.org/t/defining-a-type-in-a-recursive-way-in-dotty/6798/8):

    type Rec[F[_], A] = A match {
      case Int => Int | F[Rec[F, Int]]
      case _ => A | F[Rec[F, A]]
    }

[Jasper M](https://users.scala-lang.org/u/jasper-m) is suggesting taking advantage of the Scala [type erasure](https://medium.com/@sinisalouc/overcoming-type-erasure-in-scala-8f2422070d20) to create a runtime `LazyRef`. So I followed the advice:

    type JNull = Unit
    type JPrimitive = Int | BigDecimal | String | Boolean | JNull
    
    type Rec[JArray[_], JObject[_], A] = A match
      case JPrimitive => JPrimitive | JArray[Rec[JArray, JObject, JPrimitive]] | JObject[Rec[JArray, JObject, JPrimitive]]
      case _          => A          | JArray[Rec[JArray, JObject, A]]          | JObject[Rec[JArray, JObject, A]]
    
    type JValue = Rec[Seq, [A] =>> String Map A, JPrimitive]

And _vois la_! It works pretty well!

    // Okay, unit represents null
    var x: JValue = ()
    
    // Also okay
    x = true
    
    // OMG! Still okay!
    x = Map(
      "x" -> 3,
      "y" -> 4,
      "name" -> "test",
      "list" -> Seq("zero", 1, 2, 3, true),
    )
    
    // It throws an exception
    x = <data>nope</data>
    
    // If -Yexplicit-nulls is enabled, it throws an exception too
    x = null

Now the `JValue` is quite stricter, and the `WeakRef` made the recursive type possible.

* * *

Also in [DEV Community 👩‍💻👨‍💻](https://dev.to/cacilhas/recursive-types-32je).