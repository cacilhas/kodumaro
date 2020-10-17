![λ](//cacilhas.info/img/lambda.png)

What makes a program “functional?” You’ve probably found yourself asking yet. There are some few concepts – or **constraints** – that define the functional paradigm.

This is just a brief and shallow list of the main concepts.

Requirements:

*   **Pure function**: pure functions must have no side effects; not all functions need to be pure, but they should whenever it’s possible.
*   **Immutability**: once set, it’s for good; a value cannot change.
*   **Determinism** or **Idempotency**: given the same parameters, a function must return the very same result.
*   **Tail-call optimisation** (TCO): the last call in a procedure must replace the current memory stack; it prevents stack overflow when going recursively.
*   **First-class function**: functions are first-class citizens, able to be other function’s parameters or result, or even object attributes.
*   **Algebraic structure**: read [this other post](/2020/10/algebra.html).
*   **Algebraic data type**: ADTs are compounded types; read [this post](/2020/10/type-isomorphism.html#algebraic-data-types).

Desirable features and other concepts:

*   **Recursion**: prefer recursion (and TCO) over a loop.
*   **Laziness** or **Non-strictness**: a function may be strictly or lazily evaluated; on lazy evaluation, calls are performed on demand. In type algebra, a lazy value arity is represented by `a¹`, where `a` is the result type, take a look at [this topic](/2020/10/type-isomorphism.html#algebraic-data-types).
*   **Typed Lambda**: functions are typed by their signature; a type is a set of possible values.
*   **Arity**: given a type, arity is the amount of its possible values.
*   **Generics**: broadly, generics are about type parameters, usually related to functors and monads.
*   **Kind**: a kind is a type of types; for example, a `Set` over `Set[A]`.
*   **Higher kind**: higher kinds are a way to build more complex types on the fly, usually bringing holes; read [this](https://dotty.epfl.ch/docs/internals/higher-kinded-v2.html) and [this](https://github.com/typelevel/kind-projector#function-syntax).

Functional programming is about implementing the [λ-calculus](http://www.cse.chalmers.se/research/group/logic/TypesSS05/Extra/geuvers.pdf), created by [Alonzo Church](https://johnmacfarlane.net/church.html) in the ’30s. You‘d really get aware about him and other names like [Haskell Curry](https://iep.utm.edu/curry/) and [William Howard](https://peoplepill.com/people/william-alvin-howard/).