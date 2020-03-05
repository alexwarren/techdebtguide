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

Stack Overflow launched in 2008. This was during the optimistic days of OpenID, back when there was this idea of the internet being comprised of interoperable services rather than things being in the control of just a few enormous tech companies.

Article from 2008 https://blog.codinghorror.com/openid-does-the-world-really-need-yet-another-username-and-password/

To save the hassle of SO having to deal with usernames and passwords, instead delegate everything to OpenID. That appealed to geeks, they could run their own OpenID providers. In the future maybe every website would accept OpenID and we would be free from the problems of passwords forever. It was a technical bet on a future that never came to pass, and the consequences were felt for a long time. This was a time before password managers were mainstream. It was a kind of technical debt in its own right, the shortcut was that SO wouldn't need to build anything to handle passwords - wouldn't have to worry about storing them securely, no need to build functionality like password reset and rate limiting. In theory it kept the architecture clean - they didn't need to support both passwords and third-party logins, because they took the decision to not support passwords at all. (Design principle #1 - no passwords)

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

Code review. Shipped it. Plan with SRE to decommission old CareersAuth site. Ended up not going further and merging the databases.

Sep 2016.  I was working on the new Talent onboarding experience, and needed to update the design on the login pages to match the main Stack Overflow (as one aspect of Careers being a fork of old SO was that it had the old SO design, which was really not very user friendly). I noticed CareersAuth was way out of date with .Net frameworks. Then I questioned its existence.

Week of 18 July: Talent Onboarding - Implementing Courtny’s redesign

Week of 1 August: Login
- Resumed implementing the new Stack Overflow login design on Careers
- Upgraded CareersAuth from MVC 2 to 5.2.3, and from .Net 4.0 to 4.5.2
- I don’t think keeping CareersAuth as a separate project makes sense any more, so after implementing the new login screen designs I’ll implement its functionality “locally” on Careers itself

Week of 8 August:
- New login and register pages now implemented, hidden behind a feature flag. I will enable this on dev when I get back so we can test it properly before making it live.
- Operation Nuke CareersAuth going well so far. I have a separate feature flag for this. Currently have login and registration code working without needing to hit the CareersAuth app. It still uses the CareersAuth database for now. Need to consider where we take things next, I think ideally getting everything onto StackAuth - I’ll write up an RFC when I’m back.

Week of 22 August:
- Testing new login design on dev - will switch this on in prod on Tuesday 30th
- Sent out RFC about CareersAuth nuking

<!-- TODO: Go through what was in the RFC, and the kinds of comments people had. -->

- Finish implementing remaining CareersAuth functionality, so we can turn on the BypassCareersAuth flag and switch off the CareersAuth app
Week of 12 September:
- BypassCareersAuth feature flag now turned on on dev. Dev CareersAuth app now throws exceptions when you access it.
- Created a decommissioning plan for switching off the CareersAuth app - waiting on SRE to fill in their bits
- The CareersAuth DB now has proper migrations, just like Careers and Careers.BigStuff
- A few tweaks from code review

Week of 19 September:
- Feature flag turned on in production - Careers is now handling its own logins
- Monitoring old app to check it doesn’t get hit any more
- Created DB migrations to tidy things up once the old app is turned off [removed a lot of the standard ASP.Net Membership cruft that my new implementation didn't need]

Week of 26 September:
- CareersAuth app is dead dead dead - removed from the app pool and TeamCity

Week of 21 November:
- Start tidying CareersAuth tables

Week of 28 November:
- More CareersAuth tidying