---
title: Why we hate…
date: 2022-05-21
tags: career education-and-culture
image: //cacilhas.info/img/garbage-dump.jpg
permalink: /2022/05/why-we-hate.html
---
[image]: {{{image}}}
[WORKS ON MY MACHINE]: {{{cacilhas.url}}}/img/works-on-my-machine.png
[BEAM]: https://www.erlang.org/blog/a-brief-beam-primer/
[C++]: https://www.cplusplus.com/
[C♯]: https://docs.microsoft.com/en-us/dotnet/csharp/
[Dart]: https://dart.dev/
[DEV.to]: https://dev.to/cacilhas/why-we-hate-3m8k
[Dotty]: https://dotty.epfl.ch/
[Elixir]: https://elixir-lang.org/
[Erlang]: https://www.erlang.org/
[F★]: https://www.fstar-lang.org/
[Java]: https://docs.oracle.com/java/
[Javascript]: https://www.javascript.com/
[Julia]: https://julialang.org/
[Lua]: https://www.lua.org/
[Moonscript]: https://moonscript.org/
[Nim]: https://nim-lang.org/
[OCaml]: https://ocaml.org/
[Odin]: https://odin-lang.org/
[Python]: https://www.python.org/
[Ruby]: https://www.ruby-lang.org/
[Scala]: https://scala-lang.org/
[Scala Native]: https://scala-native.readthedocs.io/
[Standard ML]: http://www.mlton.org/
[Typescript]: https://www.typescriptlang.org/
[Wren]: https://wren.io/

:right ![NPM repository][image]

:first Sometimes I spend some time trying to acquire new programming skills, to
finally figure out I was wasting time with something useless.

But even well-known programming tools and languages have their own issues, and
this is why we hate them.


### Why we hate Python

[Python][] is one of my favourite programming languages, but I haven’t work with
it for a while, and it isn’t for nothing.

Python is strongly typed, but also dynamically typed – which wouldn’t be a
problem if it respected the assigned types, but you can assign an integer to a
variable and then assign a named tuple right after that.

[Erlang][] has a very useful line: “**fail fast and noisily**.” When Python
allows to change variable types, it potentially allows to carry errors away from
where they were caused too, which is the **worse** situation in a
debug.


### Why we hate Ruby

[Ruby][] is one of the most beloved toy programming languages, mainly because it
makes bad programmers to look cool. Everything wrong you can imagine in
programming, you can find in Ruby.

Ruby falls in the same Python’s typing issues. It also promotes monkey-patch,
code injection, assets’ implicit exportation, and tool monopoly. It’s nearly
impossible to track down where a bad behaviour comes from.

You might call Ruby a debugging hell.

However, it’s great for PoC.


### Why we hate Java

[Java][] is the big static companies’ darling.

Unlike Python and Ruby, Java is statically typed, and you need to carry errors
explicitly if you don’t want them to explode right where they arise.

Still, Java is extremely bureaucratic though, the simplest procedure is hard to
be done and requires many code statements.

Besides, Java is very greedy, consuming every hardware resource it can take.


### Why we hate Scala

The worst flaw of [Scala][] is to be not [Dotty][]. Every nice and useful
feature you expect from an impure functional programming language, you get from
Scala.

> `[update]`Actually Scala **is Dotty** currently. I haven’t worked with Scala
> for so long, that I missed that.`[/update]`

However, Scala compiles to JVM bytecodes 😖 – and this is enough reason not to
use it.

One can say you could do [Scala Native][], but that’s a hell you don’t wanna get
in – believe me.


### Why we hate OCaml

[OCaml][] is the perfect impure functional programming language, except for
requiring widely long boilerplates to work.

The simplest project is more bureaucratic than Java, and you can get into a hell
of cyclical references much too soon.


### Why we hate Standard ML

An alternative to OCaml is its grandpa, [Standard ML][], which doesn’t have the
OCaml’s same issues.

But Standard ML has another one, very annoying: lack of tools.

You can find libraries for almost everything you need in OCaml, and what you
can’t find, you can bind from C. Standard ML is way poorer on libraries, and you
can get in trouble missing something very easily.


### Why we hate C/C++

C and [C++][] are great programming languages, so great that lots of other
programming languages are built on top of them. Everything you need, you can
find or build in C/C++.

But C is too verbose, everything is tough to get done, C++ is a pitfall of
infinite tokens, and both have unexpected behaviour. Don’t get me wrong! C/C++
are very stable, but they simply don’t behave like you may expect.

The better description for those languages could be: C/C++ do exactly what you
ask, not what you want.


### Why we hate Julia

Regarding [Julia][], I think we don’t need to go further than this:

What to expect from a programming language that, when you assign a value to a
constant, raises a warning instead of an error? And the program keeps going!

> `[update 2022-05-29]`I must be honest, and I owe Julia a retractation: Julia
> doesn’t allow anymore a program to go on if a constant is redefined. I’m
> intending to write another post about Julia exclusively.`[/update]`


### Why we hate Nim

[Nim][] is Python made right. Or it would be…

The Nim’s big no-no is that you keep bumping into things that don’t behave as
they told – or don’t work at all.

The Nim creators’ line is:

:centre ![WORKS ON MY MACHINE][]


### Why we hate Elixir

[Elixir][] is a toy programming language created because kids can’t code in
Erlang – but Erlang is cool, and they wanna look cool.

[BEAM][] is the Erlang’s virtual machine, and it has a lotta restrictions that
ensure your app isn’t gonna crash unexpectedly.

At some point in the past, Erlang had become cooler than Ruby, so all cool kids
wanted to code in it, but they aren’t able to. Bad programmers can’t deal with
the BEAM’s restrictions. 🤷

In order to allow then to work with Erlang, some ~~imbecile~~ developer decided
to work around the BEAM restrictions, obliterating all Erlang advantages – but
it’s still cool. This is Elixir: a workaround to obliterate BEAM advantages.


### Why we hate X

I could keep going on and on, talking about [Javascript][], [TypeScript][],
[Wren][], [Odin][], [Moonscript][], [Lua][], [Dart][], [C♯][], [F★][], and
every other noteworthy programming language, but I think we’re good here.


-----

:small Also in [DEV.to][].
