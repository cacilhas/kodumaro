![Lamp](//cacilhas.info/img/lamp.png)

Once I was running an operation, and my team had to build the application from scratch.

I had met the core developers, we had discussing a lot about which technology would better address our demand and, after many considerations, we had chosen [Scala](https://scala-lang.org/).

**But** (and there’s always a _but_) Scala programmers are expensive, and the company wanted to save every cent (as always).

So the company hired Java programmers (cheaper than Scala) and ordered us to train ’em.

So far, so good – but (another _but_) try to build from scratch a big and complex system, containing lots of interrelated nodes with countless interactions, and unpredictable corner cases – with a team that has **no experience** on the technology!

Well… the first problem we have gotten was the [`Either` construct](https://www.scala-lang.org/api/current/scala/util/Either.html).

I usually use `Either` to transport an error using its left side, leaving the right side for the success result. It’s a common approach.

However it must be done with caution, one cannot simply put `Either` in every function return; and there’s no rule to set it, one needs some _feeling_, intuition, which is earned from the experience.

As expected, the base programmers took my code as _written-on-rock_ rules, once they had no other reference to follow, and they saw me using `Either` in some functions, so they started to use the same approach… **everywhere**.

It put the project in real trouble: many errors were lost silently, other ones were raised a far cry from where it should’ve been caught – thanks to Scala, the stacktrace was carried along, but even so it used to be hard to debug.

The core team and I weren’t enough to review every PR with the required attention, worse ’cause it’s a very subtle and hard-to-catch issue.

After a while, I had to spend a long time cleaning the code: removing the `Either` from returns, and catching errors. It was necessary, but awful for the team morale.

The team wasn’t to blame, they were doing what was expected; but the company should’ve invested more in hiring qualified professionals and in qualifying the newbies.

Unfortunately the Capitalism is about profit, not quality, getting the employees to that kinda highly uncomfortable situation.

* * *

Also in [DEV.to](https://dev.to/cacilhas/the-either-issue-3j53).