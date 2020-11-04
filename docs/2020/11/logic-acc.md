![Logic](//cacilhas.info/img/glider.png)

This post is a translation of an [older one](/2017/11/acumuladores.html).

[Declarative](https://en.wikipedia.org/wiki/Declarative_programming) [Logic](https://en.wikipedia.org/wiki/Logic_programming) Programming is a quite alien paradigm for other paradigm pogrammers.

The most popular paradigm is the Imperative Programming (it has changed), which consists of ordering statements that the computer system must perform in sequence, respecting flow changes. It’s as close as possible to the microprogram.

However, standing in a creation and management sight, Imperative Programming’s far from efficient, leads to hard-mantaining codes, much denser of bugs than any other paradigm.

A rival paradigm that’s gaining ground is the [Function Programming](https://en.wikipedia.org/wiki/Functional_programming), based on [λ-calculus](https://en.wikipedia.org/wiki/Lambda_calculus), that takes advantage of some constraints to solve the integrity issues.

Beyond those paradigms, there are a lot more, notable two:

*   The Declarative Programming: doesn’t express the algorithms by the control flow, but instead by declaring a facts domain.
*   The Logic Programming: largely based on formal logic.

The Logic Programming barely exists apart from Declarative Programming, giving existence to a large intersection named Declarative Logic Programming.

While in Imperative Programming one orders statements to the machine, in the Declarative Logic Programming one defines a facts domain and the logical relations between clauses.

No “order” is give to the system, instead queries are asked. The aftermath of the system trying to figure out the answers is the expected program behaviour.

The behaviour varies from I/O side-effects to domain changes itself.

Let’s take [Prolog](https://www.swi-prolog.org/) as reference: Prolog has four types of clauses: [facts, rules, goals, and queries](http://www.ablmcc.edu.hk/~scy/prolog/pro02.htm).

**Facts** are self-sufficient truths, they don’t depend on other facts or rules. For instance, “Anna is Bob’s friend,” that can be represented as following:

    friend(anna, bob).

**Rules** depend on facts or other rules to validate whether it’s true. For instance:

    sunny(Today) :- \+ raining(Today).

Today is sunny if it’s not (`\+`) raining.

**Goal** is the target query, with no variables. **Query** is any question asked to the system, with variables or not.

A query may be true, false, or give multiple answers depending on the variable values. The goal must be true, returning a failure status otherwise.

### Fibonacci

Let’s define the Fibonacci numbers now.

It’s required the two first values (indexes 0 and 1) to equal 1. One represents it in Prolog like this:

    fibonacci(0, 1).
    fibonacci(1, 1).

The step is defined as the sum of its two predecessors. It’s valid to call the current value `N` and its predecessors `N1` and `N2`:

    fibonacci(N, R) :- succ(N1, N),
                       succ(N2, N1),
                       fibonacci(N1, R1),
                       fibonacci(N2, R2),
                       plus(R1, R2, R).

This is a valid implementation, but with a serious stack overflow issue. [TCO](http://wiki.c2.com/?TailCallOptimization) is needed for solving it.

Let’s define two rules: the main one calling an auxiliary one. The auxiliary rule uses accumulators to transport the result:

    fibonacci(N, R) :- N >= 0,
                       fibonacci(N, 0, 1, R).
    
    fibonacci(0, _, R, R) :- !.
    fibonacci(N, A, B, R) :- succ(N1, N),
                             plus(A, B, AB),
                             fibonacci(N1, B, AB, R).

The `fibonacci/2` is the main rule; `fibonacci/4` is the auxiliary one, it takes the index (`N`), two accumulators (`A` and `B`), and the result (`R`).

Since the last call is `fibonacci/4`, it takes advantage of TCO.

### Definite clause grammar

[Definite clause grammar](https://www.swi-prolog.org/pldoc/man?section=DCG) (DCG) is a way for expressing natural and formal grammar in Prolog. It uses the `-->/2` clause as a syntax sugar for grammar clearance.

Shallowly it omits the last two parameters, creating a chaining of clauses.

The `fibonacci/4` becomes something like:

    fibonacci(0, _) --> '=', !.
    fibonacci(N, A) --> { succ(N1, N) },
                        call(\B^B^B^(true), B),
                        plus(A),
                        fibonacci(N1, B).

The curly brackets are used to define the `N1` as `N`’s predecessor without change the chain. The `call/4` uses a lambda to define `B` as the first implicit parameter.

After that, the `A` value is added to the first implicit parameter.

Finally, the recursion is called passing the predecessor, the original first parameter, followed by the current implicit parameters.

You can try the following query:

    forall(between(0, 10, X), (fibonacci(X, F), writeln({X, F}))).

For some DCG magics, look at the [Anne Ogborn tutorial](http://www.pathwayslms.com/swipltuts/dcg/).

* * *

For curiosity, the DCG above expands to:

    fibonacci(0, _, `_1`, `_R`) :- `_1` = `_R`, !.
    fibonacci(N, A, `_1`, `_R`) :- succ(N1, N),
                                   call(\B^B^B^(true), B, `_1`, `_2`),
                                   plus(A, `_2`, `_3`),
                                   fibonacci(N1, B, `_3`, `_R`).

* * *

Also in [DEV Community 👩‍💻👨‍💻](https://dev.to/cacilhas/accumulators-in-declarative-logic-programming-5cl7).