---
title: How managers can lead with visibility – Part 1
date: 2022-12-10 10:09:00 Z
categories:
- SoftwareEngineering
- EngineeringManagement
tags:
- leadership
- engineeringmanager
- management
- visibility
---

Managers’ roles and responsibilities come in different forms and shapes, but one thing is clear: if you’re a manager, you’re there to get better outcomes from the people you work with. To be able to get better outcomes from the people you lead, you need to be able to understand what is happening in that team. To be better put, you need to have visibility into that team.


> You were not in control. You had no visibility: maybe there was a car in front of you, maybe not.
>  –Alain Prost
  
As tech leaders, external factors, culture, and engineers' personal life can affect delivery flow or your team's ability to deliver on business outcomes and goals. You will need to equip yourself with data that allows you to get faster feedback to stay on top of these factors and maintain predictable and consistent delivery of value to users.

 
Whether you're managing from 1 or 2 layers away, there are four main areas you need to get both quantitative and qualitative data from to have the visibility you need to lead your team effectively. I categorize these four areas you need to get data from as process, operation, people and product.

  

You need to get both quantitative and qualitative data that will help you:

-   Understand how your team is building what they’re building on time and within the budget you have – **Process**
    
-   Understand if what has been built is running at the desired level of performance and reliability and that it will continue to do so within the foreseeable future – **Operation**.
    
-   Understand if the folks building that stuff are engaged and happy to continue building it – **People**.
    
-   Understand if the folks (users) you’re building for are getting value and deriving satisfaction from what you’re building – **Product**
    
![Framework for leading with visibility](https://hubofco.de/uploads/Pillars.jpg)
  

# Process – Understanding how your team is building what they’re building

Process is the way you habitually do things. It is how your team gets work done on time and within budget. If you’re a Pizza company, process is how you turn dough into edible Pizzas delivered to consumers. As software company, your process is ideally your approach to building what you’re building.  
  
If your organization is like most organizations, your time and budget is finite. I’m yet to see an organization that has enough of both. We always have to get value delivered within a certain budget and within a specific time – this calls for a balancing act.

One question most managers have is how their teams are building what they’re building within that time and the budget they have. 
To answer this question, you need visibility into where engineers spend their time and things that affect execution, like how collaboration works across different functions, the tooling you have that enables your team, the codebase, systems and architectures your engineers work in.

  

-   Does architecture/codebase allow teams to move as fast as possible?
    
-   Does your team have the right tooling?
    
-   Do they have autonomy to start new services, build, scale and deploy them independently?
    
-   Are your teams empowered to deliver values independently, own their backlog, ideate and figure out the most important thing to build?
    
-   How do different functions within the team work together? Do they have a shared understanding of what’s being built? Does everyone know where they fit in and what roles they need to play in what is being built.
    

  

Answers to questions like these will point you in the right direction in figuring this out.

## 3 Ways to have the visibility required

Here are ways engineering leaders can have the visibility required.

### Developers Experience/Productivity Survey

One of the ways to understand how your team is building what they're building is through regular engineering surveys to understand engineers' experience. What engineers experience while developing can reveal a ton about how they're building what they're building and where bottlenecks of pain points exist.

  
If your organization has a way of measuring employee engagement, you should still run surveys centered on developer experience. Organizational-wide engagement surveys are sometimes too general and may not effectively capture engineering experience.

A simple survey with questions like this could reveal a ton about engineering processes:

 
-   How easy is it to find documentation and access documentation?
    
-   What manual work can be automated that the team does?
    

-   What process do you find valuable?
    
-   What process would you improve?
    
-   How satisfied are you with the tooling used daily e.g linters, IDE?
    
-   What would you add or improve in our tooling?
    


When we ran some of these surveys at [TIER](https://www.tier.app/en/), we realized that documentation and tooling were issues that needed to be addressed. We started investing in making documentation accessible and consolidating them. Along the way, we had to solve a challenge of making documentation close to the code base where engineers work.


If you’re looking for where to start, [developer experience survey questions](https://docs.google.com/spreadsheets/d/1gGKtZ78sKbTzxQTydcZGEB5HiLeXsHmWNqpaTL6ikQU/edit#gid=0) by Laura Tacho is a good one.

### 1-on-1s

  

When 1-on-1 is used effectively, it can be a platform for information that allows you to know what’s going on in the team and with each engineer individually.


When I first started having 1-on-1s, one thing I wanted to avoid was making it a meeting to get project updates and progress reports. In my first meeting with a report, I would set expectations of what the meeting was and was not about. One thing I always made clear was to refrain from talking about projects or getting project updates in the meeting.

  

With time, I realized that talking about projects was unavoidable to support my team effectively. As I listened to concerns from my teams, they always pointed back to the project they were working on.

  

If a report has an issue with another engineer on a team, it’s due to a project or task they’re working on. If a report is not getting what she wants from another team, it’s due to a task or project she’s working on.

  

So I realized it’s okay to talk about projects in 1-on-1s if there is an opportunity to unblock or coach a report.

  

You should use 1-on-1s to pick signals of things going well or not going well in your team. This is why 1-on-1s can be effective in helping you understand how your team is building what they’re building.

  

Here is an example of a conversation with a report.

***Manager**: What do you think about how project XYZ is going?

**Report**: Well, it's going fine except that we underestimated the project. There was ABC we didn't think about or account for when we committed to the project. To do XYZ, we need to do ABC which will take 3 more weeks. In fact we don’t even know if it can be done yet. We're currently undertaking tech discovery on how to do ABC.*

 
From the above conversation, this team is probably not investing in proper product and technical discovery before taking on projects if it becomes a pattern.


### DORA Metrics

Measuring productivity metrics is a hard topic in software engineering. Historically, companies have attempted to measure developers’ productivity but ended up measuring the wrong thing. One thing is certain: developers’ productivity can not be reduced to a single metric or dimension. There are multiple dimensions to it.

One approach that has worked for me in the past is by measuring DORA metrics.

DORA metrics came to be after six years worth of surveys conducted by the DORA team and identified four metrics that elite-performing software teams use to measure their performance.
 

-   **Deployment Frequency**—How often do you release code to production? How often do you deliver values to users?
    

  

-   **Lead Time for Changes**— The amount of time it takes a commit to get into production. How long does it take to deliver value to users?  
     

-   **Change Failure Rate**— What is the percentage of this value that is defective?
    

  

-   **Time to Restore Service**—How fast can we get back up when we fail? How long does it take an organization to recover from a failure in production?
    

  

I have found DORA metrics effective because they can show you trends that allow you to ask crucial questions. When a deployment frequency trends downward, it puts a lot of questions you can ask your team.

  

My teams at [TIER](https://www.tier.app/en/) adopted DORA metrics. We were tracking how well we were doing WoW and MoM. Once every two weeks, I would sit down with Engineering Leads to discuss the metrics.

When we saw deployment frequency trending downwards or upwards, I would ask questions to understand what was going on. When we got better at putting code to production frequently, I also wanted to know what changes we made.

In one of the review meetings with engineering leads, deployment frequency was trending downward. As I sat down with the them to understand the root cause and how I could better support them, we saw that there were issues with our deployment infrastructure. The infrastructure team made changes to the base infrastructure that this team was yet to update to.

Most of the engineers in this team were new and had missed out on comms that had gone out earlier on why the team should upgrade. On finding the root cause, we were able to take a more actionable step, work together with the infrastructure team and get the support needed.
