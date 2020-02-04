---
layout: page
title: Technical Debt - a guide for developers and product managers
---

# {{ page.title }}

<div class="alert alert-warning" role="alert">
    Under construction...
</div>

How good is your code? On a scale of 1 to 10 - where 1 is a barely functioning mess, and 10 is the kind of ideal, clean code that developers dream about - what rating would you give the code you work with on a daily basis?

When I give talks and ask this question, I get a wide range of answers. 6 or 7 seems to be fairly common. That means that while our code is usually not totally dreadful, there's a lot of room for improvement. Why is that? Why are we, as software professionals, not creating systems that are as good as we know they could be?

We know what good software looks like, so why aren't we writing it? Are developers simply incompetent? Or are there other factors?

The gap between the code we have now, and the high-quality code that it could be (or _should_ be), is often referred to as **technical debt**.

I've been building software for a long time in a lot of different companies, both large and small, and always with plenty of super smart people. Yet, I don't think I've ever seen a system that is completely perfect. Every company has technical debt.

I've often heard developers complain that they're not being given the time to fix technical debt. They feel they're being pressured to just carry on shipping new features. Perhaps you're one of them. If so, this guide is for you. We're going to take a look at the different kinds of technical debt, and why it exists. We'll look at how to you can successfully make the case for improving code, and how to get your workplace to understand the benefits of fixing technical debt.

<ul>
{% for article in site.data.articles %}
    <li><a href="{{ article.url }}">{{ article.title }}</a></li>
{% endfor %}
</ul>