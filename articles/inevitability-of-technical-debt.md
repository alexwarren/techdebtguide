---
layout: article
title: The inevitability of technical debt
permalink: inevitability-of-technical-debt
---

Although you can try to reduce the amount of technical debt you create (as we discussed in the [last section](creating-less-technical-debt)), there is an unfortunate truth - it will _always exist_.

There is a simple reason for that - every time we build anything, we do so under constraints. As developers, we build software that has to live in a real world where disk space, memory, CPU and network speeds are all finite. That means we have to make trade-offs when engineering. Similarly, there is a finite supply of money and therefore time available for us to build our software - we never have the luxury of building things with an infinite amount of time available to build the perfect software, and so we have to make trade-offs there too.

Secondly, requirements will always change. We start off building systems under one set of assumptions, and no matter how rigorously you plan or document things up-front, people will change their minds, and you will get a better understanding of the system you are building, as you build it. That means the design of the system will change as we build it, which will result in additional complexity.

Thirdly, the world is always changing - there are always new ways of building things, and programming languages, frameworks and dependencies always get updated or replaced over time. That leads to the "lava layer" building up.

Therefore, no matter how hard you try, technical debt is inevitable. It will always exist in the software you build.

You might wonder then, if technical debt always exists, why don't programmers routinely get given time to address it? We'll look at that next.