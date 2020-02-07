---
layout: article
title: The inevitability of technical debt
permalink: inevitability-of-technical-debt
---

Although you can try to reduce the amount of technical debt you create (as we discussed in the [last section](creating-less-technical-debt)), there is an unfortunate truth - it will _always exist_.

There is a simple reason for that - every time we build anything, we do so under **constraints**. As developers, we build software that has to live in a real world where disk space, memory, CPU and network speeds are all finite. That means we have to make **trade-offs** when engineering. Similarly, we are constrained by the very finite supply of _money_ and therefore _time_ available for us to build our software. We'll never have the luxury of the infinite time it would take to make something perfect. That means we have to make trade-offs here too, building something imperfect to get it done within the time available.

Secondly, **requirements change**. When we build things for our users, those users often change their minds about what they want over time. Sometimes it's only through seeing what we've built that they get a better idea of what they _really_ wanted us to build in the first place. No matter how rigorously you plan or document things up-front, this can still happen. In a similar way, as a developer you too will get a better understanding of the system you are building, as you build it. Both of these things mean that the design of the system will change as it is built, which will result in [additional "unnecessary" complexity](types-of-technical-debt#unnecessary-complexity).

Thirdly, **the world changes**. There are always new ways of building things, and programming languages, frameworks and dependencies always get updated or replaced over time. That leads to the "[lava layer](types-of-technical-debt#lava-layer)" building up.

Therefore, no matter how hard you try, **technical debt is inevitable**. It will always exist in the software you build.

You might wonder then, if technical debt always exists, why don't programmers routinely get given time to address it? We'll look at that next.