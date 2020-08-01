---
title: Production-grade App Checklists
date: 2020-08-01 15:49:00 Z
---

As a self-taught, I struggled with many things at the beginning of my career. I remembered asking why my changes always break my code. "What do other engineers do that prevent them from introducing breaking changes when adapting a software?", I asked. Later I figured I needed to write tests and write them well.

As I grew further, I realized that writing code is not all that matters. Implementing a feature is just one small piece of a bigger puzzle. I realized that my work is just getting started when my code hits production. After all, [I built it, and I should run it.](https://aws.amazon.com/blogs/enterprise-strategy/enterprise-devops-why-you-should-run-what-you-build/) I should be able to obtain feedback from my end-users to understand better how they use my app. 

Change is constant! I should expect that my users will grow, a node will go down. All sorts of things will happen in production – and when they do happen, I better have some feedback mechanisms that will allow me to react and remain awesome to my users.Without dwelling on post-production activities, I want to ask you, What does a production-ready app mean to you?

A production-ready app means different things to different people. For some, it's an app that runs smoothly and turns in cash, and for some, it's no more than an app that satisfies project requirements or running in an environment tagged "production". The scary thing is they are right! What is production-ready to A may not be for B. That's why the following checklists are opinionated.

To assess if an app is ready for production, one must understand production-readiness criteria –– and that's the very reason I created this checklist to guide you. Hopefully, it sparks some new ideas in you.