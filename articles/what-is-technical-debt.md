---
layout: article
title: What is technical debt?
permalink: what-is-technical-debt
---

The concept of "technical debt" was coined by Ward Cunningham. In 1992 he wrote an [experience report](http://c2.com/doc/oopsla92.html) for the OOPSLA conference, in which he talked about the development process of a financial product he worked on:

> We developed the product by incremental growth from a working prototype... Mature sections of the program have been revised or rewritten many times... Although immature code may work fine and be completely acceptable to the customer, excess quantities will make a program unmasterable, leading to... an inflexible product. Shipping first time code is like going into debt. A little debt speeds development so long as it is paid back promptly with a rewrite... The danger occurs when the debt is not repaid. Every minute spent on not-quite-right code counts as interest on that debt. Entire engineering organizations can be brought to a stand-still under the debt load...

Here, Cunningham observes that it takes time to get software right. It's perfectly possible to take a shortcut, and ship non-perfect code that functions fine as far as the customer is concerned, but this will slow down future development. That means that the code should be cleaned up sooner rather than later, or else developers will end up spending so much time working around it that they won't be able to ship new features.

These days, people often use the term "technical debt" to talk about all kinds of non-perfect code - not just code that was intentionally shipped in a non-perfect state as a shortcut. Sometimes, technical debt is unintentional - maybe the original developers didn't realise there was a better way of doing what they did. And sometimes, the world moves on, leaving us code that has become outdated, and needs tidying up.

In the next section, we'll look at the different types of technical debt you'll come across.