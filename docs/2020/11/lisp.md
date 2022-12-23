![Made with secret alien technology](//cacilhas.info/img/lisp.png)

LISP is a specification by [John McCarthy](http://www.genealogy.ams.org/id.php?id=22145), that has started a brand new programming language family since 1958. It’s based on [λ-calculus](https://en.wikipedia.org/wiki/Lambda_calculus), formal system from [Alonzo Church](http://www.genealogy.ams.org/id.php?id=8011)’s work in 1930s, designed to deal with symbolic data instead of numeric, most imperative languages’ standard.

LISP means “list processing,” and list is the specification’s main structure. Every data – as the code itself – are presented as lists.

For instance, the sum of 1 and 2:

    (+ 1 2)

Which is a list of the elements `+`, `1`, and `2`. This list is processed by the `car` (head) and `cdr` (tail) functions:

    (+ . (1 2))

`car` and `cdr` are [IBM 704](https://en.wikipedia.org/wiki/IBM_704) instructions, the system where LISP was formost developed. CAR means “Contents of the Address part of Register number” and CDR means “Contents of the Decrement part of Register number.”

The head represents a lambda function, and the tail its parameters. In this case, the function is `'+`, which returns the sum of the parameters.

The LISP family’s most notable languages are [Common Lisp](https://common-lisp.net/), [Emacs LISP](https://www.gnu.org/software/emacs/manual/html_node/eintr/), [Scheme](http://www.schemers.org/), and [Clojure](https://clojure.org/).

Let’s take a look at the factorial implementation in three LISP languages. First in Common Lisp:

    (defun factorial (n)
      (if (= n 0)
        1
        (* n (factorial (- n 1)))))

In R⁵RS (Scheme):

    (define factorial
      (lambda (n)
        (if (zero? n)
          1
          (* n (factorial (- n 1))))))

In Clojure:

    (defn factorial [n]
      (reduce * (range 1 (inc n))))

### Scheme

Scheme has became a family itself, with a whole lotta different variations, not only different implementations, but different languages too.

Its most important variations are [Guile](https://www.gnu.org/software/guile/), [MIT Scheme](https://www.gnu.org/software/mit-scheme/), and [Racket](https://racket-lang.org/) (formerly PLT Scheme).

Racket is a full [RAD](https://en.wikipedia.org/wiki/Rapid_application_development) platform, containing an IDE called [DrRacket](https://docs.racket-lang.org/drracket/), WYSIWYG interface builder called [MrEd Designer](https://pkgs.racket-lang.org/package/mred-designer), and the PLT interpreter.

The interpreter supports [R⁵RS](http://www.schemers.org/Documents/Standards/R5RS/), [R⁶RS](http://www.r6rs.org/), [PLT](https://docs.racket-lang.org/), [Lazy Racket](https://docs.racket-lang.org/lazy/), and even [C](https://planet.racket-lang.org/display.ss?package=c.plt&owner=jaymccarthy).

For example, the factorial in Racket:

    !#racket
    
    (define factorial
      (λ (n)
        [if (zero? n)
          1
          (* n (factorial (- n 1)))]))

The hash-bang line tells which language Racket must deal with:

*   R⁵RS: `#!r5rs`
*   R⁶RS: `#!r6rs`
*   PLT: `#!racket`
*   Lazy Racket: `#!lazy`
*   C: `#!planet jaymccarthy/c`

### Accumulators

Accumulator is a functional programming design pattern for dealing with stack overflow by taking advantage of [tail-call optimisation](http://wiki.c2.com/?TailCallOptimization) (TCO).

Take the factorial back: the step is defined as the current value times its predecessor’s factorial; the stop is zero’s factorial, which equals one.

    n! = n × (n-1)!
    0! = 1

Looking at the PLT implementation above, you can see the last call id `'*`; in order to enable TCO, it should be `factorial`.

It must exist an accumulator-driven function to make it possible, so the `factorial` becomes:

    (define factorial
      (λ (n)
        *factorial* n 1))

The accumulator-driven version (`*factorial*`) must receive the original parameter and the accumulator, returning the accumulator when it stops:

    (define *factorial*
      (λ (n acc)
        [if (zero? n)
          acc
          (*factorial* (- n 1) (* acc n))]))

Note that now the last call is `*factorial*`, enabling the TCO.

### Lazy evaluation

The most efficient approach to deal with huge calculated data volumes is [lazy evaluation](https://en.wikipedia.org/wiki/Lazy_evaluation), or call-by-need. [Haskell](https://www.haskell.org/), for instance, just evaluates its calls by demand, which enables to build infinite lists:

    fib :: [Integer]
    fib = 1 : 1 : zipWith (+) fib (tail fib)

Than on can take only how many elements one needs:

    take 10 fib

### Promises

[Lazy Racket](https://docs.racket-lang.org/lazy/) uses promises to delay the evaluation, in a similar taste as Haskell.

The evaluation of a lazy function is made by the function `'!!`.

So the lazy factorial is:

    #!lazy
    
    (provide factorial)
    
    (define *factorial*
      (λ (n acc)
        [if (zero? n)
          acc
          (*factorial* (- n 1) (* acc n))]))
    
    (define factorial
      (λ (n)
        (*factorial* n 1)))

And one can evaluate it by calling:

    (!! [map (λ (n) `(,n ,(factorial n))) '(0 1 2 3 4 5)])

The result is:

    '((0 1) (1 1) (2 2) (3 6) (4 24) (5 120))

* * *

Translated from [here](/2017/11/lisp.html) and [here](/2017/11/lazy-racket.html).

Also in [DEV.to](https://dev.to/cacilhas/lisp-473j).