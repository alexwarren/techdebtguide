---
layout: article
title: Why fix technical debt?
permalink: why-fix-technical-debt
---

Why fix technical debt? In the previous section, we looked at the different [types of technical debt](/types-of-technical-debt), and what unifies them all is that they don't directly affect the end users of our systems. Customers don't see code - the only people who are affected by technical debt are developers, right? The systems work fine for end users, and so everybody else in the business is happy - Product Managers, the CTO, etc.

Only developers see the true horror lurking behind the scenes, but if the customers are happy, isn't that all that matters? Why would we waste time cleaning up the internal details of our software?

Well, the main reason is _it slows us down_. Technical debt makes it slower for us as developers to build new features. It makes it slower for us to find and fix bugs when they occur. It also means there are more bugs (due to unnecessary complexity and duplicated code).

Technical debt not only makes our software less reliable, it also makes our estimates less reliable. If our systems are harder to understand, then there is more chance of being surprised by something while we're trying to build a new feature, which means it's going to be harder to come up with an accurate estimate for how long a given piece of work should take.

And all of this ultimately leads to developers being less happy in their work.

Now, nobody really wants anybody to be unhappy at work, but why should the businesses we work for really care about all of this? Ultimately, they should care because all of these things translate into the one thing that every business really cares about - money.

If it's slower to build new features, then it will cost more to develop those features. Those features will also ship later, meaning the business cannot profit from them as quickly.

If it's slower to find and fix bugs, then it will cost more to maintain our systems. If there are more bugs, then there will be more outages, which costs money, and less satisfied customers, which costs money.

If our estimates are less reliable, then the business will not be able to come up with reliable plans. It will not be able to forecast when additional revenue from those new features will actually be available.

And if developers are unhappy at work, they'll go and work somewhere else, and the business will have to recruit and train new employees, which costs a lot of money.

### The debt analogy

At this point, it may be useful to think back to the concept of "technical debt" itself. Under the financial definition of debt, there are two parts:

- The _principal_, which is the amount you're borrowing.
- The _interest_, which is what you're paying to maintain the debt.

In the analogy of technical debt, the _principal_ would be the amount of clean-up work you're choosing not to do. For example, if it would take two weeks to clean up a particular piece of technical debt, then by not actually cleaning it up, you're "borrowing" two weeks of work. That means you have two weeks of work you can "spend" on something else, like a new feature.

But you don't get that two weeks of time for free - there's a cost, which is the _interest_ you're paying.  If that piece of technical debt is causing an extra hour of work for developers per day, then that's the cost we're paying to maintain that debt.

In this example, it might be prudent to keep that piece of technical debt around for a little while, but we have to be careful. If we keep adding technical debt, sooner or later those interest payments will really add up.

If you end up spending more and more time maintaining technical debt, you will have less and less time for adding new features. At some point, you'll end up spending all of your time maintaining the debt, and have no time for new features at all - you'll be completely stuck, unable to build anything new. In the analogy, you will have reached bankruptcy.