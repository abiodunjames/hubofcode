---
title: Three Ways to Gain Visibility into Indirectly Managed Teams
date: 2023-06-10 08:11:00 Z
categories:
- EngineeringManagement
tags:
- visibility
image: "https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff4a5a817-0793-4f87-ab70-9e3a2fb81980_1164x983.png"
---

How do you foster execution, remove roadblocks from frontline teams?

[](https://res.cloudinary.com/samueljames/image/upload/c_fit,w_400/v1686385258/https_3A_2F_2Fsubstack-post-media.s3.amazonaws.com_2Fpublic_2Fimages_2Ff4a5a817-0793-4f87-ab70-9e3a2fb81980_1164x983.png)

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff4a5a817-0793-4f87-ab70-9e3a2fb81980_1164x983.png)

Will Larson provided [three ways](https://lethain.com/mail-bag-resources-for-engineering-directors/) to do this that I have found useful.

-   First, you need to understand what’s happening on indirectly managed teams below you.
    
-   Second, add things necessary for execution.
    
-   Third, remove things getting in the way of execution.
    

The biggest puzzle to solve is understanding what is happening in the indirectly managed teams.

As one goes up in an organisation, the less the details you know about day-to-day jobs of people below and the unique challenges they face. An engineering director will have more knowledge about how each piece of work connects with teams under her purview but less knowledge about the codebase or the nitty-gritty of the environment frontline engineers work in and the impact it has on execution.

To draw an analogy, it’s like zooming in and out on maps. In a zoomed out mode, you see the big picture. When you’re in a zoomed in mode, you get a close look at select details. Frontline engineers have a zoomed in experience. And the higher you go in an organisation, the more zoomed out your view becomes. Folks at the top of an organisation tend to see and experience things in a broader sense than those who are layers away under them who may see things narrower but in-depth.

There are a few areas engineering leaders can look at to have visibility required to foster execution and remove roadblocks from frontline teams. That starts from finding a way to get quantitative and qualitative data that will help you:

-   Understand how your teams are building what they’re building on time and within the budget you have.
    

-   Understand what your teams have built are running at the desired level of performance and reliability and that it will continue to do so within the foreseeable future.
    

-   Understand if the teams building that stuff are engaged and happy to continue building what they’re building.  
    
-   Understand if the users you’re building for are getting value and deriving satisfaction from what you’re building.
    

## Understanding how your teams are building what they’re building.

Understanding how your teams build requires you to understand the process your teams take to build that thing they’re building. Let’s call this “process“. Process is the way your teams habitually do things. It is how your teams get work done on time and within budget. If you’re a Pizza company, process is how you turn dough into edible Pizzas delivered to consumers.

For a software company, your process is ideally your approach to building what you’re building.

If your organisation is like most organisations, your time and budget are finite. You always have to get value delivered to users within a certain budget and within a specific time. Due to this, you have to find an optimum way to optimise value delivery to users within the budget that you have.

To gain an understanding of how your team builds stuff and the possible roadblocks, take a look at where engineers spend their time, things that affect execution, how they collaborate, the tooling you have that enables them, the codebase, systems and architectures they work in.

-   Does architecture/codebase allow teams to move as fast as possible?
    

-   Do your teams have the right tooling?
    

-   Do they have autonomy to spin up new services, build, scale and deploy them without hand-offs? How long does it take them to do this?
    

-   Are your teams empowered to deliver values independently, own their backlog, ideate and figure out the most important thing to build?
    

-   How do different functions within the teams work together? Do they have a shared understanding of what’s being built?
    
-   Does everyone know where they fit in and what roles they need to play in what is being built.
    

-   Do they have necessary tools
    

Answers to questions like these will point you in the right direction in figuring out what may be standing in the way of execution.

## 3 Ways to have the visibility required

You want to gain a better understanding of unique challenges your indirectly managed teams face? Include the following in your data gathering process.

### 1. Run developers experience surveys quarterly

Running surveys is one way to understand the experience your engineers have. Engineers' development experience can reveal a ton about how they're building what they're building as well as what is standing in the way of getting things done.

I have seen engineering surveys centred on tools and technology only. Surveys like that often fall short. Sometimes the biggest source of roadblocks could be unrelated to tech or tools. A process or a social contract that has nothing to do with tech can be equally a source of friction for engineers.

I like the way C J Silverio puts it in this [post](https://blog.ceejbot.com/posts/reduce-friction/).

> _If people are regularly doing any end-run around a process to get work done (say, regularly asking for rubber-stamp PRs so they can be unblocked), you have a process that’s not earning back its energy cost. Fix it._

A simple survey with questions like the ones below could reveal a ton about your engineering processes:

-   How easy is it to find documentation and access documentation?
    
-   What manual work can be automated that the team does?
    
-   What process do you find valuable?
    
-   What process would you improve?
    
-   How satisfied are you with the tooling used daily e.g linters, IDE?
    
-   What would you add or improve in our tooling?
    

If you’re looking for where to start, [developer experience survey questions](https://docs.google.com/spreadsheets/d/1gGKtZ78sKbTzxQTydcZGEB5HiLeXsHmWNqpaTL6ikQU/edit#gid=0) by Laura Tacho is a good one.

### **2. Hold 1-on-1s and skip-level 1-on-1s**

When 1-on-1 is used effectively, it can be a platform for gathering information that allows you to know what’s going on in the team and with each engineer. 1-on-1 is a great time to offer your perspectives on what is going well and not going well. But it is also a good time to gather qualitative data, disproof or proof what you already know that is not going well.

If you’re leading from 2 or many layers away, hold skip-level 1-on-1s with frontline engineers.

-   It will help you to gather information about how your managers are really doing beyond what they tell you.
    
-   It will help you get a pulse on what's happening on the front lines.
    
-   It will help you learn where there is dysfunction, insufficient communication, or confusion within parts of your organisation.
    

### 3. Track DORA metrics

DORA metrics came to be after six years worth of surveys conducted by the DORA team and identified four metrics that elite-performing software teams use to measure their performance.

-   **Deployment Frequency**—How often do you release code to production? How often do you deliver value to users?
    
-   **Lead Time for Changes**— The amount of time it takes a commit to get into production. How long does it take to deliver value to users?
    
-   **Change Failure Rate**— What is the percentage of this value that is defective?
    
-   **Time to Restore Service**—How fast can we get back up when we fail? How long does it take an organisation to recover from a failure in production?
    

Measuring productivity metrics is a hard topic in software engineering because companies have attempted to measure developers’ productivity but ended up measuring the wrong thing. A few companies learned from this and abstained totally from ever measuring anything.  
One thing is certain: developers’ productivity can not be reduced to a single metric or dimension. There are multiple dimensions to it.

Personally, I find DORA metrics effective because they can show you trends that allow you to ask crucial questions. When a deployment frequency trends downward, it puts a lot of questions you can ask your team.  
  
One of my experiences with using DORA started at [TIER](https://www.tier.app/en/). We were tracking how well we were doing WoW and MoM. Once every two weeks, I would sit down with Engineering Leads to discuss the metrics.

When we saw deployment frequency trending downwards or upwards, I would ask questions to understand what was going on. When we got better at putting code to production frequently, I wanted to know what changes we made.

In one of the review meetings with engineering leads, deployment frequency was trending downward. As I sat down with them to understand the root cause and how I could better support them, we saw that there were issues with our deployment infrastructure. The infrastructure team made changes to the base infrastructure that this team was yet to update to.

Most of the engineers in this team were new and had missed out on comms that had gone out earlier on why the team should upgrade. On finding the root cause, we were able to take a more actionable step, work together with the infrastructure team and get supports needed.

#### Additional resources

If you’re looking for more inputs? The following links are great resources.

-   [Reduce friction](https://blog.ceejbot.com/posts/reduce-friction/)
    
-   [Skip Level Meetings: Everything You Need To Know About Skip Level 1 On 1s](https://getlighthouse.com/blog/skip-level-meetings-one-on-ones/)
    
-   [How engineering leaders can lead with visibility](https://speakerdeck.com/abiodunjames/how-engineering-managers-can-lead-with-visibility)