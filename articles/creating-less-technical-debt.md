---
layout: article
title: Creating less technical debt
permalink: creating-less-technical-debt
photo: akshar-dave-9tnw9e0bluk-unsplash.jpg
credit: Photo by Akshar Dave on Unsplash
---

Is the answer to technical debt simply to stop creating it in the first place? Here are a few ideas for avoiding adding new technical debt to your system.

A good **code review** process will help to ensure that you're not unintentionally shipping overly complex code. It can catch unnecessary code duplication, missing tests, and missing documentation. 

Ensuring that developers have **enough time** to do a good job is also important. It's very easy for developers to over-commit - we have a tendency to be optimistic about how quickly we can get things done, and there is often a temptation when making estimates to try to please people by saying a particular project shouldn't take too long. That usually ends up backfiring - we don't leave enough time to handle the unexpected, and then in rushing to complete work "on time", we can neglect to write enough tests, or create the right amount of documentation.

Always **question urgency**. Everybody likes to imagine that their own project is super important. Whenever somebody asks you to take on extra work, dig into the reasons why it's so important that this happens _now_. Instead of trying to please people by taking on too much work, try to **reduce scope** where you can. If it's really critical to deliver this new thing as soon as possible, see if there is a smaller, simpler version of it that you could try to ship first. The less rushed you are, the less technical debt you'll be creating, and the easier it will be for you and other developers to iterate on the code in the future.

**Resist the shiny**. There's always a new front-end framework to try out - but you need to resist the urge to build new features in the shiny new framework while you have old features in the old framework. You really need to justify adding additional complexity to a system like that. That means that you need to have a review process before adopting something new - the whole team needs to be on board, and you should only allow new frameworks if you have a plan for removing the old ones.

A "**bug duty**" rotation is useful. For each sprint, have a developer assigned to non-feature work. That means they have the time to fix bugs that are raised, and they can take on small technical debt projects, work on automation, and update dependencies if they have some spare time.

However, although all of the above can be used to great effect to create less technical debt, it will always exist - as we'll find out in the next section.