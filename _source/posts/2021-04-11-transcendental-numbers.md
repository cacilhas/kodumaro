---
title: How to Compute Arbitrary Precision Transcendental Numbers
date: 2021-04-11
tags: concept education-and-culture math python
image: //img.montegasppa.cc/lamp.png
permalink: /2021/04/transcendental-numbers.html
---
[image]: {{{image}}}
[become my patron]: https://www.patreon.com/join/cacilhas?
[DEV.to]: https://dev.to/cacilhas/how-to-compute-arbitrary-precision-transcendental-numbers-59lc
[employer’s project]: https://contabilone.com/
[Euler]: https://www.wolframalpha.com/input/?i=Leonhard+Euler
[Euler’s constant]: https://mathworld.wolfram.com/e.html
[Euler’s identity]: https://mathworld.wolfram.com/EulerFormula.html
[Functional Works]: https://www.works-hub.com/learn/how-to-compute-arbitrary-precision-transcendental-numbers-3c1bc?utm_campaign=Automation%20-%20Candidate%20Emails&utm_medium=email&_hsmi=106671327&_hsenc=p2ANqtz-8_Q4Pbh3qH2i_8NyfURQW4LB3Zw54WMX8p4CsnfaSnzEvto6P2K-OaMuOPdhx42y46dRm5lSrLFBtwKWSRIf8d91mzGA&utm_content=106671327&utm_source=hs_email
[Kodumaro]: {{{site}}}
[Leibniz]: https://www.wolframalpha.com/input/?i=Gottfried+Wilhelm+Leibniz
[mail me]: mailto:kodumaro@montegasppa.cc
[musical project]: https://www.patreon.com/cacilhas
[my last post]: {{{kodumaro.url}}}/2020/12/implicit-conversions.html
[my personal blog]: {{{montegasppa.url}}}
[Python]: https://www.python.org/
[subscribe]: https://www.patreon.com/join/kodumaro?
[Take a look at what I’ve been on.]: https://www.youtube.com/channel/UCVJR3ltOPy2fQ7zxzXjUvJA
[Transcendental numbers]: https://mathworld.wolfram.com/TranscendentalNumber.html
[Wolfram Alpha]: https://www.wolframalpha.com/input/?i=pi
[Wolfram MathWorld]: https://mathworld.wolfram.com/Pi.html#related

### Announcement

It has been four months since my [my last post][], one could think this blog has
come to an end, but that’s not the case.

I’ve been very busy working on my [employer’s project][], what drained my time,
stopping me working on my personal projects.

Even so, [Kodumaro][], [my personal blog][], and my [musical project][] are
**not** over. I just have not enough payback for those project, so I cannot
apply much time on those. 😢

If you’d like to patronise my musical project, [become my patron][]!!

[Take a look at what I’ve been on.][]

Now, if you enjoy {{{ kodumaro.title }}}, and would like to fund this project,
please [mail me][], and let’s talk about it. If I’m getting enough funding, I’m
considering making it my main job.

Bringing it in mind,
<strong>I’m releasing a <a href="{{{ patreon.url }}}">Kodumaro’s Patreon page</a></strong>.
Please [subscribe][].

-----

### Getting into the point

:right ![Idea][image]

:first [Transcendental numbers][] are probably the most useful tools in
Mathematics. They are irrational numbers that cannot be expressed using a finite
formula.

For example, the [Euler’s identity][] is considered the most beautiful
Mathematical formula (perhaps I’m gonna explore it in the future):

:centre ^^math e^{i\pi} + 1 = 0

It exposes a relation between the two most important transcendental numbers, the
complex numbers, the unit and the null / zero. It’s used mostly in rotation
over real 𝑣𝑠 imaginary axes, on complex exponential, roots, and other useful
operations.

Yet, it’s useless if one doesn’t know how to get 𝑒 and π in the required
precision.

### Euler’s constant

The [Euler’s constant][] or Euler’s number, 𝑒 for short, is the ratio describing
any constant growth. It’s defined as:

:centre ^^math e = \lim_{n \to +\infty}\left(1 + \frac{1}{n}\right)^n

It’s about 2.71828…. 𝑒 is kinda magical number, poping up in a lot of
Mathematical problems, offering good and easy solutions, since complex number
operations to logarithm, exponential, and other kinds of growth behaviour.

But how to get to the desired precision?

One can take this formula and compute greater and greater 𝑛 values, until it
reaches the required precision. Nevertheless, the exponent can be quite
intimidating, leading to an undesired weak performance – similar or worst than
the π computation below. Yet, it’s hard to say how long it must go to getta the
desired precision.

Fortunately another [Euler][]’s formula gives us a better solution:

:centre ^^math e = \sum_{n=0}^{+\infty}\frac{1}{n!}

This formula is very convenient, ’cause it increases the precision every step
in an easly predictable way:

|                                     |   |                                      |   |        |
|:-----------------------------------:|---|:------------------------------------:|---|--------|
| <sup>1</sup>/<sub>0!</sub>          | = | <sup>1</sup>/<sub>1</sub>            | = | 1      |
| 1 + <sup>1</sup>/<sub>1!</sub>      | = | 1 + <sup>1</sup>/<sub>1</sub>        | = | 2      |
| 2 + <sup>1</sup>/<sub>2!</sub>      | = | 2 + <sup>1</sup>/<sub>2</sub>        | = | 2.5    |
| 2.5 + <sup>1</sup>/<sub>3!</sub>    | = | 2.5 + <sup>1</sup>/<sub>6</sub>      | ≈ | 2.6667 |
| 2.6667 + <sup>1</sup>/<sub>4!</sub> | ≈ | 2.6666 + <sup>1</sup>/<sub>24</sub>  | ≈ | 2.7083 |
| 2.7083 + <sup>1</sup>/<sub>5!</sub> | ≈ | 2.7083 + <sup>1</sup>/<sub>120</sub> | ≈ | 2.7167 |
| 2.7167 + <sup>1</sup>/<sub>6!</sub> | ≈ | 2.7177 + <sup>1</sup>/<sub>720</sub> | ≈ | 2.7180 |
| ⋱                                   |   |                                      |   | ⋱     |
| prev. + <sup>1</sup>/<sub>∞!</sub>  |   | =                                    |   | 𝑒      |

It’s easly predictable ’cause the precision equals to *log₁₀(n!)*:

|            |              |   |            |   |            |
|------------|:------------:|---|:----------:|---|------------|
| n = 0 →    | log₁₀(0!)    | = | log₁₀(1)   | = | 0          |
| n = 1 →    | log₁₀(1!)    | = | log₁₀(1)   | = | 0          |
| n = 2 →    | log₁₀(2!)    | = | log₁₀(2)   | ≈ | 0.3010     |
| n = 3 →    | log₁₀(3!)    | = | log₁₀(6)   | ≈ | 0.7782     |
| n = 4 →    | log₁₀(4!)    | = | log₁₀(24)  | ≈ | 1.3802     |
| n = 5 →    | log₁₀(5!)    | = | log₁₀(120) | ≈ | 2.0792     |
| ⋱          | ⋱            |   |           |   | ⋱           |
| n = 1000 → | log₁₀(1000!) |   | ≈          |   | 2’567.6046 |

It gets big very quickly.

### More everyday case

Image one needs to calculate 𝑒 to the 80-bit precision. That doesn’t differ from
before. Bits are binary digits, i.e, 2-base numbers. So, instead of *log₁₀*, one
must use *log₂*:

- log₂(n!) ≈ 80
- 2<sup>log₂(n!)</sup> ≈ 2<sup>80</sup>
- n! ≈ 2⁸⁰
- 25! &lt; 2⁸⁰ &lt; 26!
- 25! &lt; n! &lt; 26!
- 25 &lt; n &lt; 26

So, 26 steps are good enough.

Let’s implement it using [Python][]:

```python
from typing import Callable

# Lazy factorial implementation
fact: Callable[[int], int] = lambda n: prod(range(1, n+1))

# An LRU cached version, if you prefer:
#
# from functools import lru_cache
#
# fact: Callable[[int], int] = lru_cache(lambda n:
#     1 if n <= 0 else (n * fact(n - 1))
# )

def steps(prec: int) -> int:
    res = 1
    while fact(res) >> prec == 0:
        res += 1
    return res
```

The right shift (`>>`) returns zero as long as the value’s less bit wide than
the precision.

Now let’s compute 𝑒 itself:

```python
compute_e: Callable[[int], Rational] = lambda prec: sum(
    1./fact(x) for x in range(prec)
)
```

The solutions coming out from it is:

```python
>>> calculate_e(steps(64))  # Python uses 64-bit precision floating point numbers
2.7182818284590455
>>> math.e
2.718281828459045
```

Not bad at all. In fact, we might reach the 64-bit precision with only 18
iterations.

### Performance

We’re not worried about performance here, it’s not this post’s scope. We’re just
showing how it works and how you can implement and use it.

### What about π?

π is the ratio of any circle’s perimeter (circumference) to its diameter. That’s
π very definition.

However, it’s a transcendental number and needs an infinity serie to be
computed. [Leibniz][] gave us a neat solution:

:center ^^math 1-\frac{1}{3}+\frac{1}{5}-\frac{1}{7}+\frac{1}{9}-\frac{1}{11}+\cdots=\frac{\pi}{4}

The process is quite the same used for 𝑒, take the formula:

:centre ^^math \pi=4\sum_{n=0}^{+\infty}\left(\frac{1}{4n+1}-\frac{1}{4n+3}\right)

Then get the precision:

```python
compute_pi: Callable[[int], Rational] = lambda prec: 4. * sum(
    (1./(4*n+1) - 1./(4*n+3)) for n in range(prec)
)
```

Tip: the precision is *log₂(4n)*, so <em>n = 2<sup>prec-2</sup></em>, which one
can get by left shifting. Note that we got exactly the inverse of the previous
calculation: here the necessary steps amount grows very fast with small gain.

So you may get disappointed when running:

```python
>>> compute_pi(1 << (64 - 2))
```

This computation is way more inefficient than the previous one – Python isn’t
that good in dealing with floating-point operation and it’s necessary a
humongous amount of steps to getta the target. I recomend 5M steps:

```python
>>> compute_pi(5000000)
3.141592553588895
>>> math.pi
3.141592653589793
```

To this task, one’s gonna need a tougher tool, as C and multithreading. Again
performance is not this post’s scope. Probably it requires using some C cast
spells in order to optimise the computation.

Or you can go down through the [Wolfram MathWorld][]’s or [Wolfram Alpha][]’s π
page references in search of better formulæ. I did it, and I can ensure they
are, including serie representations.

-----

:small Also in [DEV.to].

:small Also in [Functional Works][].

:language python
