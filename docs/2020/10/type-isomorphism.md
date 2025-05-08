![Scala](//cacilhas.cc/img/scala.png)

In [type algebra](https://kseo.github.io/posts/2016-12-25-type-isomorphism.html), type isomorphism is the case when two types are interchangeable with no information lost.

That means, two types `A` and `B` are isomorphic to each other if any `a: A` can be casted or converted to `B` and back to the same `a`, and any `b: B` can be casted or converted to `A` and back to the same `b`.

For instance, take the following example:

    sealed trait YesNo
    object Yes extends YesNo
    object No extends YesNo

The `YesNo` type is isomorphic to `Boolean`:

    implicit def yesno2bool(value: YesNo): Boolean = value == Yes
    implicit def bool2yesno(value: Boolean): YesNo = if (value) Yes else No

There’s no information lost in converting `YesNo` to `Boolean` and back.

### Arity

The way to find whether two types are isomorphic is by their arities: if the arities are the same, the types are isomorphic.

Briefly, a type’s arity is the amount of its possible values. Consider a type as a set: the arity is the number of elements.

A few main examples:

*   `Nothing`: arity is 0 (no possible instance)
*   `Unit`: arity is 1 (only the `()` value)
*   `Boolean`: arity is 2 (`true` and `false`)
*   `Int`: arity is 4 294 967 296 (2³², 4 bytes)

So if `A` and `B` have the same arity, you can map each value in `A` to only one in `B`, and vice versa.

### Algebraic Data Types

[ADT](https://wiki.haskell.org/Algebraic_data_type) (don’t confuse with [ADT – abstract data type](https://www.geeksforgeeks.org/abstract-data-types/)) are compounded types types under a compounding-operation viewpoint, and their arity follows simple arithmetic rules.

A disjunctive type – when the compounding values are exclusively optional, that means, one **or** another – has arity equal to the sum of its compounding types.

For instance, the `YesNo` type is the disjunction of `Yes.type` and `No.type`, it means a `YesNo` instance can be a `Yes` (instance of `Yes.type`) or a `No` (instance of `No.type`):

*   `Yes.type` has arity 1
*   `No.type` as arity 1
*   `YesNo ≡ Yes.type + No.type`
*   ⊢ The `YesNo`’s arity is 1 + 1 = 2

Note: [Dotty (Scala 3)](https://dotty.epfl.ch/) has a nice [syntax](https://dotty.epfl.ch/docs/reference/new-types/union-types.html) for type disjunction, similar to [Haskell](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/sum-types)’s.

A conjunctive type – when the compounding values are companions, that means, one **and** another – has arity equal to the product of its compounding types.

For instance, take the tuple `(Boolean, Int)`:

*   `Boolean` has arity 2
*   `Int` has arity 4 294 967 296
*   `(Boolean, Int) ≡ Boolean * Int`
*   ⊢ The `(Boolean, Int)`’s arity is 2 × 4 294 967 296 = 8 589 934 592

A lambda type has arity equal to the return type’s arity raised to the power of the argument type’s – multiple arguments are equivalent to a conjunctive type.

*   `Boolean => Int` has arity 4 294 967 296²
*   `Int => Boolean` has arity 2⁴²⁹⁴⁹⁶⁷²⁹⁶

You can understand why reading [this](https://codewords.recurse.com/issues/three/algebra-and-calculus-of-algebraic-data-types#fn:answer).

### Swapping

Note that swappable types are isomorphic, cause `a+b=b+a` and `ab=ba`.

So `A|B` is isomorphic to `B|A`, and `(A,B)` is isomorphic to `(B,A)`.

The proof is nothing trickier than the `swap` function itself: if you can swap the values without losing data, the types are isomorphic.

    def swap[A, B](v: (A, B)): (B, A) = (v._2, v._1)

### Curiosity: isomorphism in C

Since C has low-level access to the memory (through pointers) and is weakly typed, one can cast between the two most different types if they have the same memory length.

For instance, in C `int` and `float` are isomorphic.

And amusing code is the fast inverse square root form Quake Ⅲ Arena (`Q_rsqrt`), it does some type black magic with pointer casting:

    float Q_rsqrt(float number) {
        long i;
        float x2, y;
        const float threehalfs = 1.5F;
    
        x2 = number * 0.5F;
        y  = number;
        i  = *(long *) &y;
        i  = 0x5f3759df - (i >> 1);
        y  = *(float *) &i;
        y  = y * (threehalfs - (x2 * y * y));
    
        return y;
    }

In this example, `long` and `float` aren’t isomorphic, ’cause they have different arities, but it shows how data are easily interchangeable, promoting the isomorphism.

* * *

Also in [DEV.to](https://dev.to/cacilhas/type-isomorphism-3bp9).