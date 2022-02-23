---
title: The “Either” Issue
date: 2022-02-23
tags: career education-and-culture
image: //cacilhas.info/img/lamp.png
permalink: /2022/02/either-issue.html
---

[dev.to]: https://dev.to/cacilhas/the-either-issue-3j53
[Either]: https://www.scala-lang.org/api/current/scala/util/Either.html
[scala]: https://scala-lang.org/

{:class="pull-right"} <img src="{{{ image }}}" alt="Lamp"/>

{:class="mg-first"} Once I was running an operation, and my team had to build
the application from scratch.

I had met the core developers, we had discussing a lot about which technology
would better address our demand and, after many considerations, we had chosen
[Scala][scala].

**But** (and there’s always a *but*) Scala programmers are expensive, and the
company wanted to save every cent (as always).

So the company hired Java programmers (cheaper than Scala) and ordered us to
train ’em.

So far, so good – but (another *but*) try to build from scratch a big and
complex system, containing lots of interrelated nodes with countless
interactions, and unpredictable corner cases – with a team that has
**no experience** on the technology!

Well… the first problem we have gotten was the [`Either` construct][Either].

I usually use `Either` to transport an error using its left side, leaving the
right side for the success result. It’s a common approach.

However it must be done with caution, one cannot simply put `Either` in every
function return; and there’s no rule to set it, one needs some *feeling*,
intuition, which is earned from the experience.

As expected, the base programmers took my code as *written-on-rock* rules, once
they had no other reference to follow, and they saw me using `Either` in some
functions, so they started to use the same approach… **everywhere**.

It put the project in real trouble: many errors were lost silently, other ones
were raised a far cry from where it should’ve been caught – thanks to Scala, the
stacktrace was carried along, but even so it used to be hard to debug.

The core team and I weren’t enough to review every
<u title="Pull Request">PR</u> with the required attention, worse ’cause it’s a
very subtle and hard-to-catch issue.

After a while, I had to spend a long time cleaning the code: removing the
`Either` from returns, and catching errors. It was necessary, but awful for the
team morale.

The team wasn’t to blame, they were doing what was expected; but the company
should’ve invested more in hiring qualified professionals and in qualifying the
newbies.

Unfortunately the Capitalism is about profit, not quality, getting the employees
to that kinda highly uncomfortable situation.

-----

{:class="small"} Also in [DEV Community 👩‍💻👨‍💻][dev.to].