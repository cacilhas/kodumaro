![MoonScript](//cacilhas.info/img.moonscript.png)

[2017](/legacy.html), just before the Brazil’s democracy death, was one of my more fruitful years (the more at all was [2008](https://kodumaro.blogspot.com/2008/)), so I decided to transcript some of that year’s posts back.

I’m starting with a compilation of two posts [¹](/2017/02/reiteradores-em-moonscript.html) [²](/2017/02/mais-reiteradores-em-moonscript.html) about interators in [MoonScript](https://moonscript.org/).

### Lua standard iterator

The Lua’s standard approach is a function that returns multiple values:

1.  The deal function, that returns the next value or `nil` if it’s ended.
2.  The overall state (usually the stop condition).
3.  The initial value.

For instance, the [Collatz Conjecture](https://planetmath.org/CollatzProblem) can be implemented as follows:

    collatz = => _collatz, 1, @*2

The initial value is dobled to allow the first step to return it; the loop ends when the state reaches one (`1`).

The deal function (`_collatz`) must accept the stop condition (`1`) and the last value. Using the MoonScript’s `self`, we can omit the first argument:

    _collatz = (last) =>
        return if last == @
        switch last % 2
            when 0
                last / 2
            when 1
                last * 3 + 1

Then it already works in `mooni`:

    moon> print x for x in collatz 10
    10
    5
    16
    8
    4
    2
    1
    moon>

### Closures

We can solve this problem by using closures too.

For that, we need to write a factory that returns only the deal function, but with no arguments.

    collatz = (value) ->
        value *= 2
        ->
            return if value == 1
            switch value % 2
                when 0
                    value =/ 2
                when 1
                    value = value * 3 + 1
            value

This new function holding a closure (the `value` variable) behaves exactly like the `collatz` implemented using the standard iterator, but in one single block.

### Coroutines

The more powerful resource in Lua is the [coroutine](https://www.lua.org/pil/9.1.html).

Coroutines allow yielding results while it keeps running the concurrent routine.

Collatz Conjecture using coroutine is:

    _collatz = (value using coroutine) ->
        import wrap, yield from coroutine
        wrap ->
            while value != 1
                yield value
                switch value % 2
                    when 0
                        value =/ 2
                    when 1
                        value = value * 3 + 1
            1

And also:

    moon> print x for x in collatz 10
    10
    5
    16
    8
    4
    2
    1
    moon>

### Fibonacci

Let’s implement two approaches of [Fibonacci numbers](https://encyclopediaofmath.org/index.php?title=Fibonacci_numbers).

Using the standard iterator, we simply don’t need to know the last result, we can manage it inside a mutable state – it’s possible to implement it in only one function using `unpack`:

    fib = (n) -> unpack {
        -- Deal function
        =>
            return if @n == 0 -- stop
            @a, @b, @n = @b, @a+@b, @n-1
            @a                -- step
    
        -- Initial state
        {a: 0, b: 1, :n}
    }

And this works fine:

    moon> print i for i in fib 10
    1
    1
    2
    3
    5
    8
    13
    21
    34
    55

Now Fibonacci numbers using coroutine:

    fib = (n using coroutine) ->
        import wrap, yield from coroutine
        wrap ->
            a, b = 0, 1
            for _ = 1, n
                a, b = b, a+b
                yield a

The both approaches are performative, ’cause they use double accumulator to run a linear procedure.

* * *

Also in [DEV Community 👩‍💻👨‍💻](https://dev.to/cacilhas/lua-moonscript-iterators-1fd0).