![Scala](//cacilhas.info/img/scala.png)

For a developer to be able to work with Functional Programming, there are a few concepts he or she must master.

In addition to the [functional constraints](/2019/09/lies-they-told-you.html), there are two very important main ideas: [algebraic data types](/2020/10/type-isomorphism.html) (ADT) and algebraic structures. In this post, we’re looking at some algebraic structures.

### Functor

Functor is the simpliest algebraic structure. It’s a wrapper around a datum or data that can map operations over them and their successful results.

A basic functor example in [Scala](https://www.scala-lang.org/) is [`scala.util.Try`](https://www.scala-lang.org/api/current/scala/util/Try.html):

    Try(1 / x)
      .map(e => e*e)
      .map(1 - _)
      .map(_.toString) match {
        case Success(value) => s"the result is $value"
        case Failure(exc)   => s"it raised an exception: $exc"
      }

Most of the Scala basic wrappers, like `Seq`/`List`, `Option`, `Either`, etc, are functors too.

### Monad

[Haskell](https://www.haskell.org/)’s monads have been getting people in dread, but they’re nothing than a variation of functor approach.

A monad is a wrapper around a datum or data that can operate over them flatly mapping the results and possibly describing side-effects.

Our first monad example is the [`Seq`](https://www.scala-lang.org/api/current/scala/collection/immutable/Seq.html):

    Seq(1, 2, 3)
      .flapMap(e => Seq(e * 2))
      .flatMap(e => if (e == 2) Nil else Seq(e))
      .flatMap(e => if (e == 4) Seq(4, 5) else Seq(e))

The result is `Seq(4, 5, 6)`; the `flatMap` method is the main resource of a monad.

Since `Seq` is a functor too, you may replace the first `flatMap` by a `map`, and it has a `filter` method, more expressive than `flatMap`:

    Seq(1, 2, 3)
      .map(_ * 2)
      .filter(_ != 2)
      .flatMap(e => if (e == 4) Seq(4, 5) else Seq(e))

The result is the same. `List`, `Option`, `Try`, `Either`, etc, are monads too.

The knownest monad is the Haskell’s [`IO`](https://wiki.haskell.org/Introduction_to_IO). The `IO` monad describes how an I/O side-effect happens.

For example:

    greetings :: IO ()
    greetings = printStrLn "Hello, World!"

The `greetings` function idempotently returns an `IO` monad that can trigger a writing to the standard output.

### Monoid

A monoid is a structure that supplies an operation and an unit instance for that operation and type.

A simple monoid definition is:

    trait Monoid[A] {
      def add(a: A, b: A): A
      def unit[A]: A
    }

Let’s, for instance, define two monoids for dealing with integer and string sequences:

    implicit object IntMonoid extends Monoid[Int] {
      def add(a: Int, b: Int): Int = a + b
      def unit: Int = 0
    }
    
    implicit object StringMonoid extends Monoid[String] {
      def add(a: String, b: String): String = a concat b
      def unit: String = ""
    }

Defined these two monoids, we can code a function that can reduce an integer or string sequence:

    def reduce[A](xs: Seq[A])(implicit ev: Monoid[A]): A =
      if (xs.isEmpty) ev.unit
      else ev.add(xs.head, reduce(xs.tail))

It works like this:

    reduce(Seq(1, 2, 3)) // returns 6
    reduce(Seq("Hello", ", ", "World", "!")) // returns "Hello, World!"

In order for the `reduce` function to work properly with other types, it’s enough to supply another type’s implicit monoid, for instance a `DoubleMonoid`, no need for changing the `reduce` function.

> `[update]`
> 
> I forgot to mention, an example of Scala’s built-in monoid is [`Numeric`](https://www.scala-lang.org/api/current/scala/math/Numeric.html): the operation method is `ev.plus`, and the unit is `ev.zero`.
> 
> `[/update]`

### Apply, applicative, semiring, lattice, etc

Other algebraic structures are mostly just variants of these three we’ve just been through. You can find some definitions in [this glossary](https://www.linkedin.com/pulse/glossary-functional-programming-john-de-goes/).

* * *

Also in [DEV Community 👩‍💻👨‍💻](https://dev.to/cacilhas/algebraic-structures-2g9o).