---
layout: article
title: The different types of technical debt
permalink: types-of-technical-debt
lava_layers:
    "2010 - all JQuery": "1,1,1,1,1"
    "2012 - add Ember": "1,1,1,2,1,2,2"
    "2014 - add AngularJS": "1,3,1,2,1,3,2,3,3,3"
    "2016 - add React": "1,3,1,4,1,4,2,4,3,3,4,4,4,4"
---

    TODO: Examples, impact, diagrams

### Unnecessary complexity

As the French mathematician Blaise Pascal once wrote, "I would have written a shorter letter, but I did not have the time". The same thing happens in programming - it's harder, and takes longer, to create the simplest system possible that fulfils the requirements. That means our systems are often more complex than they really need to be.

Another factor in this is that as we add features to a system, we improve our understanding of what we're trying to build. But it's easier to keep bolting on new parts than rework the foundations. You end up with a much better understanding of the system you "should" have built in the first place, but it's only through creating the current, messier, more complicated system that you reached that understanding.

The unnecessary complexity of the system you have built means that it becomes harder to maintain and add new features.

In an ideal world, developers would spend time rearchitecting the system to make it as simple as possible. In the real world, such a project could end up being an enormous rewrite, taking a lot of time to deliver a system that works substantially the same as the existing one.

### Duplication

It's very easy to copy and paste, and there's an art to knowing when it's appropriate to do so. Get the balance wrong and you can end up with lots of code dotted around your system that does more-or-less the same thing. That can mean you fix a bug in one place, but it crops up somewhere else as well.

In an ideal world, developers would refactor the code to remove the duplication. In the real world, this can end up not happening, because it doesn't change how the system works, and there's often the risk of breaking something, if the duplicated code is subtly different.

### Lava layer

Over time, you can end up accumulating different ways of doing the same thing. For example, a project that started in 2011 may have chosen jQuery as its front-end framework, so built everything for the first version on top of that. A year later, Angular 1 has been released, and so developers start building new features using that. They don't go back and rewrite all of the older code to use Angular though. As the years go by, new frameworks are used for newer features, but the older features remain using the older frameworks, and so you end up with archaeological layers in your codebase.

<table class="table">
    <thead>
        <tr>
            <th scope="col">Year</th>
            <th scope="col">Code</th>
        </tr>
    </thead>
    <tbody>
    {% for layer in page.lava_layers %}
        <tr>
            <th scope="row">{{ layer[0] }}</th>
            <td>
                {% assign elements = layer[1] | split: "," %}
                {% for element in elements %}
                <span class="lava_layer_{{ element }}">&block;</span>
                {% endfor %}
            </td>
        </tr>
    {% endfor %}
  </tbody>
</table>


This is a form of unnecessary complexity - if you had lots of time, you would rewrite everything using the latest and greatest framework. But that doesn't happen, and so you end up with different frameworks having to get along with each other. It can make debugging harder, and it can make adding new features much harder. It makes things harder to reason about - when you're planning a change to an existing feature, you might be touching multiple frameworks. A developer might have to learn how to implement something in an older framework, because there will be no time to rewrite the whole feature in a newer framework. 

http://mikehadlow.blogspot.com/2014/12/the-lava-layer-anti-pattern.html

In an ideal world, developers would rewrite the older features so that everything is consistently using one framework. In the real world, this doesn't happen, because it can be a big rewrite of existing code that functions perfectly well, and there's always the risk of breaking something along the way.

### Lack of testing

A lack of automated tests, including unit tests and end-to-end tests, means it's harder for developers to confidently make changes to a system. That slows developers down, and results in less reliable software.

In an ideal world, developers would want to spend time adding the missing tests. In the real world, this often doesn't happen, because there are always new features that could be built instead.

### Lack of documentation

Without adequate documentation, it takes longer for developers to understand how a system works. Writing documentation takes time, and writing good documentation takes even longer. A lack of documentation might also mean a lack of comments. Either way, it makes the system harder to understand and so it's slower to get developers up to speed and build new things.

In an ideal world, developers would take the time to create and update documentation with every change to the system. In the real world, this often doesn't happen, because it's left as a task until a feature has shipped - and then it's time to move onto something else.

### Lack of automation

Technical debt sometimes manifests itself not directly in the code, but in the systems and processes around it. For example, a deployment process that has multiple manual steps - this can be error-prone, and slow things down if a human gets something wrong.

In an ideal world, developers would take time to automate manual processes. In the real world, this often doesn't happen if the current process is seen as being "good enough", and automating it might initially introduce some risk.

### Outdated dependencies

Unlike other kinds of technical debt, this is one that can increase over time without anybody actually doing anything. You could have a hypothetically perfect system - then come back a year later, and find that the libraries it depends on have moved on. The newer versions of these libraries may have fixed some security holes, so you really ought to update them, but you may well find that there are now incompatibilities. If you want to add a new feature to the system, do you carry on using an old library or upgrade it? Maybe the documentation for the new one has superseded the old version, so it's going to be harder _not_ to upgrade. On the other hand, upgrading the library may break something else.

In an ideal world, developers would update all the dependencies so everything is on the latest version. In the real world, this can risk breaking things, and can take time away from building the features themselves.