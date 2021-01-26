---
title: What People Get Wrong about DevOps
date: 2021-01-26 19:35:00 Z
comment: true
author: Samuel
canonical_url: https://medium.com/swlh/what-people-get-wrong-about-devops-535bfe48664e
---

I was scrolling carelessly on Reddit a few days ago when I saw a thread on buzzwords that made me pause for a few seconds. In truth, buzzwords are powerful. They tend to express ideas and concepts in one word that are usually more popular than the core ideas they represent. For me, I like to see buzzwords as a way of packaging ideas with fancy words.

But this post is not about buzzwords but rather a word that many people misunderstand: DevOps.

I get emails from recruiters and companies looking for DevOps engineers all the time. This makes me wonder how different are people’s ideas of DevOps. Needless to say, the countless number of posts that had been written on technology to master to become a DevOps guru. If you’re looking to transition from a backend engineer to a DevOps engineer and vice versa, the internet got the answer for you. Today, no fewer than 7 million results are available on Google when you search the word “DevOps Engineer position.” Companies are waiting for that candidate to be their DevOps god.

At a time in my career, I thought DevOps was another tech role. Five years ago, I remember sitting at an interview and the engineer at the other side of the table popped the question. “What do you understand by DevOps”? I answered it all wrong. I thought it was a role. Retrospectively, DevOps is still a word not many people comprehend.

You might ask, what is DevOps? To understand what DevOps is, we first need to understand what DevOps is not.

#### It’s not a role or a title.

DevOps is not a role you assume after graduating with a CS degree from a top university. Neither a role you’re entitled to after building test automation infrastructure for a company’s product. DevOps is much more than a title conferred on one person.

#### It’s not about tools.

While many tools can augment your DevOps initiatives, DevOps is not about tools. You don’t practice DevOps by installing or learning tools. DevOps is broader than the tools you use. If your organization does not fully grasp DevOps’ main concept, you will have a hard time benefiting from all the concepts DevOps has to offer.

#### It’s not all about bringing Devs and Ops together in one team.

DevOps is much more than combining Devs and Ops in the same team. While having Devs and Ops in the same team with the purpose of improving collaboration is one of the DevOps practices, it’s easy to fall into the trap that you’re practicing DevOps by merely bringing Devs and Ops together in one team.

#### It’s not a team

It’s common to see businesses wanting to adopt a DevOps mindset assume they practice DevOps by creating a team tagged “DevOps team” mainly composed of Ops engineers in charge of operations related tasks. Having a DevOps team does not mean you have adopted DevOps.

> DevOps is a culture, a way of doing things. It transcends a single team.

It must be embraced by the entire organization. You may have heroes who understand DevOps and have shown the willingness and strong leadership skills able to help drive DevOps transformation across the board in your organization. However, a dedicated team of Ops engineers is not the same thing as practicing DevOps.

### What does DevOps mean?

The word DevOps came to be in 2009 after Patrick Debois coined it. It’s a word formed by combining “development” and “operations.” To understand the reasoning behind the word and how it came to be, you first need to understand the problem it tries to solve.

Historically, there is friction between development teams and operation teams, which creates a myriad of problems such as longer time to market, frequent outages, reduced quality, and continuous technical debt mounting. This friction is a result of two opposing goals that both teams must pursue simultaneously.

The development teams are tasked with shipping more features, making frequent changes, and responding to shifting and changing market demands. On the other hand, IT Operations teams are tasked with making service reliable, stable, and secure.

When you make frequent changes, the chances of disrupting service stability or breaking something are high. Since the IT Ops team has a goal of service reliability and stability, it often responds by putting measures that make it difficult to disrupt production services. It hinders the development team from responding quickly to changing markets. These measures often include bureaucratic approval policy, outright rejection, or long waiting time from my personal experience.

In the real world, apps that generate the most revenue are the ones that need frequent changes, releases, and need to be the most reliable and stable. These opposing goals make developer-operations conflict unavoidable, and hence something has to be done. Hence, the word DevOps was born.

### What does it mean to practice DevOps?

DevOps is about people as well culture with one goal of continuously delivering business value.

> *It’s a set of practices and patterns that turn human capital into a high-performance organization capital.*

It’s a way of working. It’s about building practices around Culture, Automation, Lean, Measurement, and Sharing. It’s hinged on a collaborative working relationship with IT Ops and Developers.

![img](https://cdn-images-1.medium.com/max/1600/0*uMTk7yzL1vvVR-XL.png)Image from https://itnext.io/do-not-put-devops-in-a-cage-3604a83821e1

Rather than having developments and IT Ops in separate teams, they are brought together in the same team for a fast feedback loop. The cross-functional team implements feature validation and ensures quality. It’s like my all-time favorite quote from the CTO of Amazon: you build it; you run it.

> The cross-functional team builds it; they are responsible for running it.

The team that practices DevOps works in a production-like environment where features are continuously rolled out to production. Instead of a one big bang deployment that happens once a month or year, they deploy several times a day.

Rather than working on a huge feature that takes months to deliver, features are broken into smaller deliverable chunks that provide business value. Short-lived PRs are opened and merged quickly once the feature is done, creating a fast feedback loop. With the fast feedback loop, they can see how their changes and actions negatively or positively impact the business outcome. This team has efficient and fast automated tests that run whenever changes are committed into version control, ensuring stability and security.

Rather than a culture that frowns at mistakes, mistakes are embraced and become a valuable learning tool. The team members are encouraged to take risks.

> Rather than waiting for someone to tell them what to do, they take initiative and innovate.

Instead of finger-pointing when a problem occurs, they openly discuss it and learn from it. The organization builds a culture where people are rewarded for taking risks. With the fear of making mistakes gone, they frequently experiment, thus fostering more innovation.

Some features are in production for weeks yet remain invisible to the end-users. They are turned on for the internal or some selected users allowing teams to test and verify that they meet business outcomes before turning on all users.

> They conduct several A/B tests to learn user behavior and understand how each feature directly affects the business outcome.

Because the organization hired smart minds, micro-management is non-existent. Each team is autonomous and makes decisions in the business’s interest. Instead of assigning tasks to teams, problems and goals are given. The business trusts the smart minds it hired to develop the best solution to solve the problem.

Because the business cares about achieving business goals, they create long-term teams around each goal responsible for meeting them. Developers are no longer reshuffled or re-assigned to new projects. Each team takes full responsibility and works independently to achieve its assigned goal.

Rather than thinking in terms of output, they think in terms of outcome. They don’t celebrate based on the number of tasks completed but rather on outcomes or business outcomes. Because the team cares about quality, resilience, and reliability, they deliberately inject failures into their production environment to uncover weaknesses and learn how their systems fail and remediate them.

### Wrapping up

We could go on and on what DevOps is; the bottom line is DevOps is not a title, a role, or a job function, and you can’t hire it. It’s more of a culture, and your entire organization needs to embrace DevOps for it to work. Beautifully sums up by Irma Harlann in his [post](https://neonrocket.medium.com/devops-is-a-culture-not-a-role-be1bed149b0#:~:text=Mike Dilworth%2C Agile and DevOps,DevOps for it to work.&text=DevOps is about continual learning and improvement rather than an end state.), *“The whole company needs to be doing DevOps for it to work”