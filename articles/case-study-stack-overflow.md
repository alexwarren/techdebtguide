---
layout: article
title: "Case Study: Technical Debt at Stack Overflow"
permalink: case-study-stack-overflow
---

For several years I worked as a web developer at [Stack Overflow](https://stackoverflow.com/). As with every company, Stack Overflow has some technical debt in its systems (remember, [technical debt is inevitable](/inevitability-of-technical-debt)), and this is a story about a particular piece of technical debt which I was involved with. This is an interesting example because it demonstrates how the changing requirements of a changing business lead to the creation of technical debt. It's also a story with a happy ending, because I was able to implement a plan for fixing it.

### CareersAuth

In 2016, I was working on a redesign of the login experience for [Stack Overflow Talent](https://stackoverflow.com/talent/en), which is the portal that employers use to post job listings, create company pages and search for candidates on Stack Overflow.

I was changing this crufty old login page, with its prominent OpenID options:

<img src="/photos/old-login.png" class="screenshot" alt="Old login page">

To something more like the one which had recently been implemented on the main Stack Overflow Q&A site:

<img src="/photos/new-login.png" class="screenshot" alt="New login page">

This looked like it should be simple - just a bit of HTML and CSS tweaking, pretty much copying and pasting from the Q&A codebase, right? Well, it turned out to be a little bit more complicated than that. There wasn't just the look of the form to consider, but things like the behaviour that occurs when the user types in an incorrect password.

The code for handling user logins and registration was split across two different codebases - the UI lived in the main Stack Overflow Talent codebase, but there was a separate site called CareersAuth that received the login and registration form posts, and handled validating and storing usernames and passwords.

This slowed me down a bit while implementing the updated redesign. Firstly, CareersAuth hadn't been touched for several years, so I had to spend some time updating various dependencies before I got it to work locally on my developer machine. Secondly, being on a separate site to the login and registration forms slowed me down with some of the UI changes I had to make.

I could see other drawbacks of the design too. There was no mechanism for users to change their passwords while they were logged in, which seemed like pretty basic functionality - but would have been hard to add with the current design. Email addresses were stored in two places, as Talent had its own Users table, and CareersAuth had a separate database. That meant that when users changed their email address on Stack Overflow Talent, this was not reflected on the separate CareersAuth database, which meant they couldn't get a password reset sent to their new email address.

Those kinds of issues could have been worked around, but they would have added unncessary complexity. The system was harder to understand than it needed to be, and not well documented. Why had intelligent people implemented this in such a strange way? It turns out, as with a lot of technical debt, that the decisions made perfect sense at the time. Let's look at a little history.

### A failed bet on OpenID

(Nice timeline from Punyon on this: https://meta.stackoverflow.com/questions/312452/careers-unificintegration-jobs-on-stack-overflow)
2006: Joel on Software job board: https://www.joelonsoftware.com/2006/09/05/introducing-jobsjoelonsoftwarecom/

When Stack Overflow launched in 2008, a technical decision was taken which seemed to make a lot of sense at the time, and co-founder Jeff Atwood wrote about it at the time in a blog post called [OpenID: Does The World Really Need Yet Another Username and Password?](https://blog.codinghorror.com/openid-does-the-world-really-need-yet-another-username-and-password/)

> As we continue to work on the code that will eventually become stackoverflow, we belatedly realized that we'd be contributing to the glut of username and passwords on the web. I have fifty online logins, and I can't remember any of them! Adding that fifty-first set of stackoverflow.com credentials is unlikely to help matters. With some urging from my friend Jon Galloway, I decided to take a look at OpenID.

In those days, we still had this idea that the internet was comprised of lots of interoperable services, rather than things being centralised in the control of just a few enormous tech companies. OpenID pre-dated users being able to log in to websites with their Facebook or Google accounts. While Facebook or Google could have implemented OpenID themselves to give their users the ability to do that, _anybody_ could be their own OpenID provider if they had their own website. Then they wouldn't need to rely on a big company to give them one-click login to other websites all over the web.

So, Stack Overflow took a bet on OpenID being the future. It would save them the hassle of having to deal with passwords - no need to worry about storing them securely, and no support calls from people who have forgotten their password, because all of that stuff is effectively outsourced to whoever that user's OpenID provider is. In the future, maybe every website would accept OpenID, and so users would be used to logging in that way.

Unfortunately, that didn't quite come to pass. While it is common these days to log into a website using your Facebook or Google account, we don't use OpenID. Instead, websites use a similar technology called OAuth. This isn't interoperable in the same way that OpenID was - if a website only supports logins from, say, Google, you can't use your Facebook account to log in. Users are limited to logging in using one of the big tech companies - but on the plus side, it is a simpler system for users to understand. With OpenID, you'd have to find your provider from a big list of options, or enter your unique ID URL, but with OAuth, you just hit the "log in with Facebook" or "log in with Google" button and go from there.

Adopting the fairly niche (and not particularly user-friendly) OpenID in 2008 was a technical decision that made sense at the time. The users of Stack Overflow were programmers, so expecting them to understand the technicalities of OpenID wasn't too much of a big ask. It gave Stack Overflow benefits in that they didn't have to build lots of functionality around passwords.

But as time passes, the world changes, and that means that these kinds of early decisions don't make sense forever.



and we would be free from the problems of passwords forever. It was a technical bet on a future that never came to pass, and the consequences were felt for a long time. This was a time before password managers were mainstream. It was a kind of technical debt in its own right, the shortcut was that SO wouldn't need to build anything to handle passwords - wouldn't have to worry about storing them securely, no need to build functionality like password reset and rate limiting. In theory it kept the architecture clean - they didn't need to support both passwords and third-party logins, because they took the decision to not support passwords at all. (Design principle #1 - no passwords)

June 2009 SO job board *and* SF job board https://www.joelonsoftware.com/2009/06/03/get-a-job/
Super basic and not integrated at all with the sites. Rebadging of the Joel on Software job board.

Then came Careers, the job board for Stack Overflow (later renamed to Stack Overflow Talent). At that time the Stack Overflow network looked like: Stack Overflow, Super User, Server Fault. The idea was a network of sites. So it made sense to build Stack Overflow Careers as a separate network site. (Design principle #2 - separate sites for separate things)

Nov 2009 Careers 1.0 https://www.joelonsoftware.com/2009/11/05/upgrade-your-career/
Developers create a CV and pay a fee to sign up and be searchable by employers. Employers couldn't sign up yet, that was December (https://www.joelonsoftware.com/2009/12/02/programmer-search-engine/).
"It’s not for the 5.2 million people who visit Stack Overflow; it’s for the top 25,000 developers who participate actively. It’s not for every employer; it’s for the few that treat developers well and offer a place to work that’s genuinely fulfilling."

SO needed employers as well as developers to log into SO Careers though. And employers are less technical than developers, and would need to log in using a username and password. So the way to build that, to fit in with the architectural principles, was to build a simple OpenID provider that employers could use.

Feb 2011 Careers 2.0 https://stackoverflow.blog/2011/02/23/careers-2-0-launches/ - free but invite-only

https://stackoverflow.blog/2010/04/13/openid-one-year-later/
Starts to talk about discrepancies in behaviour between providers, and other drawbacks

HandleSuccess https://meta.stackexchange.com/questions/207388/why-is-the-handlesuccess-method-such-a-terrible-one

May 2011 - SE finally supports usernames and passwords, by becoming an OpenID provider:
https://stackoverflow.blog/2011/05/27/stack-exchange-is-an-openid-provider/

Later StackAuth was built for SO. Now we were maintaining two OpenID providers.

Revisit principle #1 - later Stack allowed usernames and passwords. OpenID died so the entire principle was rolled back.
Revisit principle #2 - wanted jobs things integrated with Stack Overflow.

The hack to put Jobs in SO was implemented in Dec 2015 https://stackoverflow.blog/2015/12/21/bringing-jobs-to-stack-overflow/
<!-- TODO: I have some slides on this -->

### A plan to fix it

So that's how the technical debt came about. How did I fix it?

Slowly, carefully, and with planning.

Although this technical debt was slowing me down with the work I was doing to update the login pages, I didn't try to fix it immediately. That would have been a lot of scope creep for that task. I finished the work with the technical debt still in place - I wasn't about to try to "sneak in" a rewrite of the login back-end alongside this front-end change. But, I allowed myself enough autonomy over my work to spend a day or two doing _research_ about the change I wanted to make, and write it up as an RFC.

An RFC is a "Request For Comments" document. Stack Overflow had a process for these - anybody could propose a change, write it up in an RFC document, and circulate it around the company to get feedback. There was a dedicated mailing list for this, where RFCs would be sent to other developers, support staff, management and marketing. If your company doesn't have an official RFC process, you can still do the same thing - just send around a document by email to as many relevant people as possible, and ask for feedback and comments on it.

Here's a lightly edited version of the RFC I sent around:

<div class="quotedoc" markdown="1">

### RFC: Operation Nuke CareersAuth

#### Problem

When users log in to Careers using an email address and password, that is handled by a separate app and database called CareersAuth. This is technical debt - it is overly complex for the simple functionality it provides. It also means that:

- Users cannot change their passwords while logged in, they have to use the “forgot password” functionality to get a password reset emailed to them.
- When users change their email address on Careers, this is not reflected in the separate CareersAuth database, which means they can’t get a password reset sent to their new email address.

CareersAuth is implemented as an OpenID provider. It pre-dates StackAuth, which is also an OpenID provider. It would be desirable for us not to maintain two OpenID providers, and it would also be desirable for users to be able to log in to Careers using the same email address and password they use to log in to Stack Overflow.

#### Background

At the time CareersAuth was implemented, StackAuth did not exist. The only way to log in to Careers was via a separate OpenID provider, but this adds complexity for users, as they need to use or set up an account on a third-party website to log in to ours.
CareersAuth was created as an in-house OpenID provider so users could log in to Careers using a username and password. This is implemented using the standard ASP.NET SQL Membership Provider and DotNetOpenAuth to implement OpenID. This way, Careers can talk to CareersAuth just the same as if it were an external OpenID provider.

DotNetOpenAuth is apparently no longer maintained (though I can’t find a source for this) and the ASP.NET Membership provider is ​not great​. Careers only needs to identify a user logging in with an email address and password, and it already has access to the CareersAuth database, so OpenID seems overkill - a user will never use their CareersAuth credentials to log in to any other website.
 
#### Proposed Solution

##### Stage 1: Nuke the CareersAuth app

The Careers app can handle all CareersAuth functionality by talking directly to the CareersAuth database.

There is no great magic to how the ASP.NET SQL Membership provider works, so this functionality is relatively easy to implement ourselves using Dapper.

Here is a link to a prototype: [link to a GitHub branch]

The feature flag `BypassCareersAuth` toggles whether Careers uses the CareersAuth app via OpenID, or whether it uses the new code in AuthService.

When we turn this feature flag on in production, the CareersAuth app can be switched off.

##### Stage 2: Nuke the CareersAuth database

Migrate the following CareersAuth tables to the main Careers database:

- `aspnet_Membership`
- `aspnet_Users`
- `openid_aspnet_Users`
- `PasswordResetRequest`

The remaining tables in CareersAuth are not required by the `BypassCareersAuth` implementation.

There is also an `aspnet_Membership_CreateUser` stored procedure. We could either migrate this or just call the same SQL directly.

##### Future Stages

These need a bit more thought, so are not in scope for right now (but if you do have thoughts, please share them!)

- Fixing the data model. Currently we have email addresses in three places - `CareersAuth.aspnet_Membership.Email`, `CareersAuth.aspnet_Users.Username` and `Careers.Users.Email`. When a user’s email address is changed in Careers, we do not update the CareersAuth data. This means we have to check additional usernames (CareersAuth usernames are just email addresses) in the login code, as the login username is still the old email address. It also means currently you can’t send a password reset to your new email address. Users can log in using their old or new email address. There’s also nothing to stop you changing your email address on Careers to one that has an existing CareersAuth account. Needs more thought about how to tidy this up.
- Migrating to StackAuth. This seems desirable so people can use the same username+password on Careers as they do on SO. Needs more thought about how to handle this, especially what to do about accounts that already exist in StackAuth.

</div>

Feedback:

- "Is migration to StackAuth actually worth doing? StackAuth is kind of a gross hack, and it would put a lot of burden on Careers (and Core) to share it’s functionality."
- "Great! One less thing to worry about during failovers and no more 'What the heck is CareersAuth again?' So I anticipate you are making SREs happy."
- "Looks good, very pragmatic approach, particularly using Dapper rather than that bloody horrible ASP.NET membership cruft... I’m not sure there’s really any advantage to moving the data into Careers if we’re considering a move to StackAuth? Seems like it’ll add additional complexity for little benefit (Careers can already read/write to CareersAuth). Also, wondering if there’s any security advantages to keeping it as a separate DB?"
- "Part of deprecating CareersAuth also involves tearing down the website/app pools, TeamCity configs etc. I’m guessing we need to get SRE involved here; both to get rid of the dev/prod config and to make sure they’re aware of it in their failover plans."

So this showed some benefits I hadn't thought about - simpler for SRE to maintain. It also showed where I could simplify things - we didn't actually need to do a big database migration at all, we could just keep CareersAuth as a separate database. And it showed something which I missed out - the actual decommissioning process for the old app, which would require coordinating with SRE to update various configurations and take the old site down.

A plan was coming together, and we were agreed on a way forward - all without a single meeting. My manager gave the OK for the work to go ahead, and a couple of weeks later, the old CareersAuth app was no more.