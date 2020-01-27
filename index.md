---
layout: page
title: Technical Debt - a guide for developers and product managers
---

Under construction...

How good is your code? If I had to ask you, on a scale of 1 to 10 - where 1 is barely functioning, and 10 is the kind of ideal, clean code that developers dream about - what rating would you give the code you work with on a daily basis?

When I give talks and ask this question, I get a wide range of answers. 6 or 7 seems to be fairly common. Generally, the code we're working with is not _dreadful_, but there's usually a lot of room for improvement. Why is that? Why are we, as professional developers, not creating code that is as good as we know it could be?

That gap, between the code we have now, and the high-quality code that it could be (and some would say _should_ be), is often referred to as **technical debt**. Sometimes, it's worth fixing - and sometimes, it's not. Even when it is worth fixing, this is the kind of work that often falls to the back of the queue, in favour of pushing out yet more features.

In this guide, we'll look at the different kinds of technical debt, and why it exists. We'll look at how to empower developers to improve code, while still delivering value to the business, and how the business can understand the benefits of fixing technical debt.

<ul>
{% assign articles = site.data.articles %}
{% for article in articles %}
    <li><a href="{{ article.url }}">{{ article.title }}</a></li>
{% endfor %}
</ul>