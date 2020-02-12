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

There are various different types of technical debt. Let's take a look at each type, and look into the impact it has. We'll also look at how to fix each type - and also why each type is often left unfixed.

Jump to:

- [Unnecessary complexity](#unnecessary-complexity)
- [Duplication](#duplication)
- [Lava layer](#lava-layer)
- [Lack of testing](#lack-of-testing)
- [Lack of documentation](#lack-of-documentation)
- [Lack of automation](#lack-of-automation)
- [Outdated dependencies](#outdated-dependencies)

---

### Unnecessary complexity

<img src="/photos/immo-wegmann-qnrJoo2_4EQ-unsplash.jpg">

<div class="photo-credit">Photo by Immo Wegmann on Unsplash</div>

As the French mathematician Blaise Pascal once wrote, "I would have written a shorter letter, but I did not have the time". The same thing happens in programming - it's harder, and takes longer, to create the simplest system possible that fulfils the requirements. That means our systems are often more complex than they really need to be.

Another factor is this: as we add features to a system, we improve our understanding of what we're trying to build. But it's easier to keep bolting on new parts than rework the foundations. You end up with a much better understanding of the system you "should" have built in the first place, but it's only through creating the current, messier, more complicated system that you reached that understanding.

**What is the impact?** The extra complexity makes it harder to maintain the system, or add new features.

**How do we fix this type of technical debt?** Rearchitect the system to make it as simple as possible.

**Why does this type of technical debt often go unfixed?** Rearchitecting the system can end up being an enormous task of rewriting software that already "works". Worse, rewriting code may introduce new bugs, which means extensive testing is required to ensure that the new version works just as well as the old.

---

### Duplication

<img src="/photos/judith-prins-AJa7S1fjy-I-unsplash.jpg">

<div class="photo-credit">Photo by Judith Prins on Unsplash</div>

It's very easy to copy and paste, and there's an art to knowing when it's appropriate to do so. Get the balance wrong and you can end up with lots of code dotted around your system that does more-or-less the same thing. That can mean you fix a bug in one place, but it crops up somewhere else as well.

**What is the impact?** It's harder to make changes and fix bugs, because the same "thing" occurs in more than one place. You might implement a change in behaviour, but find that there are edge cases which are handled by copies of the same code - if you forget to update these as well, the change won't work in all situations.

**How do we fix this type of technical debt?** Consolidate and simplify the code, refactoring it so that duplicated code is replaced by calls to the same function or module.

**Why does this type of technical debt often go unfixed?** As with unnecessary complexity, refactoring to remove duplicated code can look like wasted effort, because the code already "works". Sometimes, duplicated code is not an exact match, meaning there can be subtle differences in behaviour. Refactoring this code may therefore introduce bugs.

---

### Lava layer

<img src="/photos/ben-klea-zXODFyN1eII-unsplash.jpg">

<div class="photo-credit">Photo by Ben Klea on Unsplash</div>

Over time, you can end up accumulating different ways of doing the same thing. For example, a project that started in 2010 may have chosen jQuery as its front-end framework, so built everything for the first version on top of that. A couple of years later, a new front-end framework called Ember has been released, and so developers start building new features using that. However, they don't go back and rewrite all of the older code to use Ember - maybe just a few pieces here and there, to make integrating the new features easier.

As the years go by, new frameworks are used for newer features, but the older features remain using the older frameworks. Sometimes, some of the code using these older frameworks is replaced, but rarely are all the older features brought up to date to use the new framework.

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

**What is the impact?** Debugging harder, and adding new features can be much harder. Things are more difficult to reason about - a new feature, or a change to an existing feature, might have to touch multiple frameworks. Developers have to learn multiple frameworks.

**How do we fix this type of technical debt?** Rewrite older features to use the newer framework.

**Why does this type of technical debt often go unfixed?** This is a type of unncessary complexity, and the fix is the same - a rewrite. So, again, this often doesn't happen, because it's a lot of work to rewrite something that already works, and there's the risk of breaking something along the way.

---

### Lack of testing

<img src="/photos/cdc-98PI-JTfQP0-unsplash.jpg">

<div class="photo-credit">Photo by CDC on Unsplash</div>

Time pressure to deliver new features often means that once the code to implement a feature exists, and it has passed some manual testing, then it is shipped to production. Often, developers leave creating automated tests until the end of a project - which means sometimes, they are never written, and the developer moves on to the next thing.

**What is the impact?** A lack of automated tests, including unit tests and end-to-end tests, means it's harder for developers to confidently make changes to a system. That slows developers down, and results in less reliable software.

**How do we fix this type of technical debt?** Add the missing tests.

**Why does this type of technical debt often go unfixed?** There are always new features that could be built instead of adding tests.

---

### Lack of documentation

<img src="/photos/miguel-pinto-4cWNnW14NsU-unsplash.jpg">

<div class="photo-credit">Photo by Miguel Pinto on Unsplash</div>

As with a tests, writing documentation is something that's often left to the end of a project, and then dropped when priorities move on. Documentation can take many forms - wikis, collaborative documents, or even simply code comments. Writing documentation takes time - and writing _good_ documentation takes even longer.

**What is the impact?** Without adequate documentation, it takes longer for developers to understand how a system works. The system is harder to understand and so it's slower to get developers up to speed and build new things. Bugs can be introduced where developers don't have a full understanding of the design principles of a system, or the kinds of edge cases it has been built to handle.

**How do we fix this type of technical debt?** Write the missing documentation.

**Why does this type of technical debt often go unfixed?** Again, there are always new features that could be built instead of devoting time to writing things up.

---

### Lack of automation

<img src="/photos/rock-n-roll-monkey-LEPhZkQbUrk-unsplash.jpg">

<div class="photo-credit">Photo by Rock'n Roll Monkey on Unsplash</div>

Technical debt sometimes manifests itself not directly in the code, but in the systems and processes around it. For example, a deployment process that has multiple manual steps - this can be error-prone, and slow things down if a human gets something wrong.

**What is the impact?** Risk of outages due to errors in performing manual steps. 

**How do we fix this type of technical debt?** Automate manual processes.

**Why does this type of technical debt often go unfixed?** If the current process is seen as being "good enough", then it can be hard to justify the effort of automating it. If people make mistakes when performing manual steps, it may be easier to blame the individual - "be more careful", rather than invest in automation. Automation takes time to get right, so initially it may have bugs of its own.

---

### Outdated dependencies

<img src="/photos/tara-evans-_DAY5kuDvsU-unsplash.jpg">

<div class="photo-credit">Photo by Tara Evans on Unsplash</div>

Unlike other kinds of technical debt, this is one that can increase over time without anybody actually doing anything. You could have a hypothetically perfect system - then come back a year later, and find that the libraries it depends on have moved on. The newer versions of these libraries may have fixed some security holes, so you really ought to update them, but you may well find that there are now incompatibilities. If you want to add a new feature to the system, do you carry on using an old library or upgrade it? Maybe the documentation for the new one has superseded the old version, so it's going to be harder _not_ to upgrade. On the other hand, upgrading the library may break something else.

**What is the impact?** Risk of security issues when using outdated dependencies. Can be harder to find documentation for older versions of libraries. Overall, it's easier to update dependencies regularly than to "jump up" multiple versions at once.

**How do we fix this type of technical debt?** Update dependencies so everything is on the latest version.

**Why does this type of technical debt often go unfixed?** Updating dependencies can risk breaking things, and can take time away from building features.