---
layout: article
title: "Case Study: Technical Debt at Stack Overflow"
permalink: case-study-stack-overflow
---

I used to work at Stack Overflow, and this is a story about some technical debt I encountered there. It's interesting to look at how this particular piece of technical debt came to exist in the first place, because it shows how a changing business leads to changing requirements, which is one of the reasons why technical debt is inevitable in any system. Then we'll look at how I went about fixing it.

I came across this technical debt in 2016 when I was working on a redesign of the login experience for Stack Overflow Talent, which is the portal that employers use to post job listings, create company pages and search for candidates on Stack Overflow.

I found that the code for handling user logins and registration was split across two different codebases - the UI lived in the main Stack Overflow Talent codebase, but there was a separate site internally called CareersAuth that received the login and registration form posts, and handled validating and storing usernames and passwords.

This had a few impacts:
- showing validation errors in the UI was a pain
- CareersAuth had outdated dependencies, was hard to get up and running on a developer system
- emails were handled in two places - Careers had a Users table, and CareersAuth had a separate table

Harder to understand, not documented... Simple functionality like a "forgot password" form couldn't be implemented.

So it was overcomplicated, led to bugs where emails got mismatched. Why had intelligent people implemented this in such a strange way? It turns out, as with a lot of technical debt, that the decisions made perfect sense at the time. Let's look at a little history.

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

So that's how the technical debt came about. How did I fix it?

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