---
layout: article
title: Making the case for fixing technical debt
permalink: making-the-case-for-fixing-technical-debt
---

In the previous section, we looked at [why developers are not routinely given time to fix technical debt](why-developers-dont-get-time-to-fix-technical-debt). It turns out that if you're a developer, and you have a technical debt problem, it's up to _you_ to make the case for fixing it. How can you do that effectively?

First, you need to remember what your organisation really cares about. Most organisations that hire developers will care about these things:

- How long it takes to ship features
- How reliable their systems are
- Risk
- Above all... money, which is what all the above ultimately boil down to

You all ultimately have the same goal - yes, an organisation wants you to be able to ship code quickly, but it also wants you to ship code that can be confidently adapted to future requirements.

Remember: you are not "just a coder". You're not at school any more - your job is _not_ just to sit there, doing what you're told. That's not how this works. You are a technical expert, and you have input into the product you are building. It's _your job_ to raise technical issues, and if you're not doing that, then you're not doing your job properly. You have knowledge that other people don't - even your direct manager is not aware of all the problems you face day to day. Your job is to identify issues and proactively communicate them.

Communication is a big part of the job of a software developer, and often under-valued. Even coding itself is mostly about communication - your job is not just to write code that compiles, but code that communicates its meaning to the next developer who comes to work on it. Successful software developers remember their ABC - Always Be Communicating.

So, what's the best way to communicate the technical debt problems you're facing? First, you need to identify them.

### Identify the problems

Writing problems down is a good start. Keeping a tech debt log somewhere will help you to prioritise. You could keep that log on a wiki, in Jira or Trello, or on a collaborative Google Doc. When you come up against some technical debt that is causing you pain, add it to the log.

You need to be careful, though, not to make it an infinite wishlist. As developers, we often have a tendency to be nit-picky, and we can always find problems somewhere. It's not about logging all the "wouldn't it be nice if..." ideas - it's about logging things that are actively slowing you down right now. This means you need to advocate for the stuff that really hurts - it should only go on the log if at least one developer is prepared to actively make the case for fixing it. If nobody wants to advocate for fixing something, it's probably not actually that big a problem, so you should focus on something else.

Once you've identified and prioritised the technical debt you want to fix, the next step is to make a plan.

### Make a plan

You should start small so that you can build trust. You want to be able to show that you can fix something when entrusted to do so, and that it will have a benefit. Then you'll be more likely to be able to move on and fix the bigger things.

You need to be able to describe in words the technical debt problem you're having, and the impact of it - that is, how much it is slowing you down, and what the potential risks are if it is not fixed. Then you need to be able to describe the solution - including an honest assessment of how much time it will take to develop that solution, and the benefits it will have. You need to build a solid case for fixing the technical debt - so remember, it needs to be something worthwhile that you can really advocate for, as this is not just about tinkering.

I recommend putting this in the backlog, in the same place as other feature work - that means you can prioritise it against that. With a proper plan written down, it will be much easier for you to make the case for fixing the technical debt, instead of picking up yet another feature.

It should be planned in just the same way as feature work - including testing and so on. If you think that sounds like a lot of admin, remember that the processes you have in place should be there to help you ship reliable code to production. It may be tempting to "sneak in" some technical debt fixes here and there, hidden alongside feature work - and while this may work for very small bits of refactoring that are in the same area as a feature you're working on, any technical debt that is really hurting you is probably worth fixing as a dedicated project itself. Sneaking in fixes is a way of risking more issues - much better to be upfront about what you're doing, and benefit from the testing and deployment processes you have in place. Remember, you want to build trust.

Get support from the rest of your team and your line manager in creating your plan. And be prepared to compromise - you need to understand where the business is coming from, and you won't get your way all of the time. Perhaps you can identify ways of reducing the scope of your technical debt projects, and getting some of the benefit by doing a smaller piece of work. Sometimes, you might conclude that a given piece of technical debt is simply not worth fixing - after going through the whole process, you may find that the expected benefits don't justify the cost.

Ultimately, things will never be perfect. Technical debt can be a useful tool - if we're careful about taking it on, and have an understanding of the cost. As a developer, it's your responsibility to help your organisation understand the cost, and understand when it's really worth fixing.