---
title: A Guide to Creating API Products
date: 2021-03-13 17:53:00 Z
categories:
- SoftwareEngineering
- Product
tags:
- ApI
- API products
author: samuel
comments: true
---

> Originally published by me on [www.mindtheproduct.com](https://www.mindtheproduct.com/a-guide-to-creating-api-products/), \[03/02/2021\].

Building a good Application Programming Interface (API) is more than returning responses. Being a developer and having integrated with tons of APIs, I have noticed a pattern between successful API products and those that are not. It’s about solving problems with great affordance. One might ask the key to building good API products. In this post, I’ll provide some tips that I have found essential when doing so.

After landing my first software engineer role seven years ago, my first big project was building an online book marketplace. In the project, I integrated with multiple Application Programming Interfaces (API). From pre-filling book information using an International Standard Book Number (ISBN) to charging customer credit cards, integration with APIs was key to this project.

## Working with APIs

Reflecting on my career, I have integrated, built, and exposed multiple APIs. With time, I began to notice two distinct patterns about APIs. Some have certain characteristics that make developers fall in love with them. There is something about them that draws you in, and you feel like drawing others in too. You just want to tell or recommend it to others at no cost.

Likewise, the universe is not also free of APIs that make you feel otherwise. They appear not to live up to their promises. It’s either the documentation provides a generic list of actions you can perform with the API, or it’s hard to understand how the API can solve your problem.  Sometimes, If you manage to scale through the documentation hurdle without issues, the implementation is lurking around the corner to bite you. If you overcome the implementation difficulties, security or support are just around the corner waiting to pounce on you.

## Guidelines and Categories

I realized there are two categories of APIs: well-designed APIs and poorly designed ones. Until a particular time in my career, I assumed poorly designed APIs were a function of developers who built them. While this may be true in some cases, I have also learned that the fate of an API is not decided during the development phase only, it starts from the time you conceive an idea that needs to expose some interfaces and beyond. I like to see APIs as not just some mere interface or endpoint, at least in this post. I prefer to think of them as products that expose some interfaces and solve a problem irrespective of the technology or protocols. Therefore I will refer to APIs as API products interchangeably.

While there are so many API guidelines and best practices for implementing them, most are geared towards code implementation details. Little has been said on the business side of things. It’s important to note that well-designed API products ride on a rock-solid business strategy.

The API growth rate is increasing at an explosive rate. According to a recent [Postman API report,](https://blog.postman.com/api-growth-rate/) between January 2019 and January 2020, there was an increase of more than 100% in API growth.  This growth has been mainly due to the significant shift in the tech industry and consumer behavior **—** the rise in cloud adoption, the increase in microservice architecture adoption, and consumers’ love for multi-device experience.

### To put these in a better way:

* Consumers are moving from a single to multi-device experience, which drives a need for systems to be integrated.

* Businesses are moving away from the monolithic way of building applications to a more decentralized architecture.

* Organizations are shifting from on-premise data-center to the cloud.

The tech industry’s significant shifts and [consumer behavior](https://www.mindtheproduct.com/understanding-users-learnings-from-the-mtpcon-session-speakers/) further buttress that the API economy has come to stay, and businesses benefiting the most tend to create API products that developers love to use.

Every successful product either solves a problem or makes solving a problem easy. Such is true with API products. Good API products are sufficiently capable of solving the problem they’re designed to solve without compromising availability and security. They are easy to use, easy to evolve, and hard to misuse.

## Understand the “Why” and “How”

It’s not too uncommon to see APIs that no one understands the problems they are trying to solve. Undeniably, API products are no different from other great products. Great products have a rock-solid strategy — the “why” and “how” are clearly defined and understood. Before you set out to build an API, you first need to ask yourself: why do I want to build an API? What problem will the API solve or make it easy to solve? How will the API help me achieve my goals?  If you’re unable to understand the business needs for an API,  you should engage the right people and figure them out before designing it.  Your strategy will transverse down from the business to implementation details. When your strategy is clear, It will challenge developers to think creatively in achieving the end goals.

## Design Matters

Mehdi Medjaoui, in his book [Continuous API Management](https://www.amazon.com/Continuous-API-Management-Decisions-Landscape/dp/1492043559), describes the design as something you do when you make decisions about how something you’re creating will look, feel, and be used.  Each time you add or update an API, you’re actively making design decisions for better or worse. This is why you have to make a deliberate effort at the design stage — you can’t rush it.

When designing your API, you should pay attention to the following:

* Vocabularies: Are your words and terms easy to understand for your users?

* Styles: What protocols are you supporting, Rest, or GraphQL?

* Naturalness: Do your users have to change their usual ways of solving their problems significantly? Did you follow established standards and conventions?

* Consistency: What level of familiarity will you provide? Are your APIs similar to what your users may have used in the past?

## Get Good at Documentation

Documentation is an important aspect of building API products that people love to use. The year 2020[ state of the API report](https://www.postman.com/state-of-api/executing-on-apis/#executing-on-apis) showed that documentation is the highest obstacle to consuming APIs. If you’re a developer, you understand how frustrating a lack of sufficient documentation can be when integrating with APIs.  Be aware that people will read your API documentation for three primary reasons: evaluation, integration, and debugging.

Evaluation is what people do when they are trying to see if your API product solves their problem. Once your API is evaluated, users will need your documentation for integration. Once your API is embedded in their application, they will visit it when something goes wrong.

If you’re providing only one type of documentation, you’re undeserving your users. Teams must understand that documentation isn’t just a “dump” of API parameters. There are marketing and customer support aspects that should be included. Besides your endpoints, errors, requests, and response structures, your documentation should also tell the story, how you see the problems, and your solutions to solve those problems.

## Get Development Right

This is one of the most important phases in API development. This is where your ideas, strategies are turned into implementations. The development phase contains many vital decisions that are hard to reverse if not made right. You will need to make some internal decisions like what technologies to use, protocols, runtimes, and where to run the [software](https://www.mindtheproduct.com/a-product-managers-approach-to-building-integrations-for-saas-software/) that powers your API. End users don’t care whether you use NoSQL or MySQL as your datastore. What’s important when you make technology decisions is that you choose a technology your team can support.

The quality of tech decisions you make transcends what is needed to launch your product’s first version.  Launching the first version of your API is one part of the job. You have to make decisions about your API’s quality, reliability, changeability, and maintainability over a lifetime. [API performance and uptime ](https://www.postman.com/state-of-api/executing-on-apis/#executing-on-apis)are considered to be some of the major determinants of whether an API product meets consumers’ expectations. This sheds light on how consumers will assess your API.

## Tracking effectiveness

You also have to decide what to test and how you will test it to build confidence in your API. In summary,  your tests should be able to cover the following:

* Can the API Product deliver on the strategic goals?

* Is the API Product quality enough to support the strategic goals?

APIs are meant to be simple, they hide complexities and do not expose them. I like to compare APIs to an electric plug — simple to use without any complicated manuals —  an electric plug does not care about the source of electricity, neither does it care about the change over switch installed or the electricity provider. It’s a conduit for electricity–plug, and it just works. Admittedly, modern systems are complex, but the complexity should not leak to the interfaces.

## In Conclusion

Building a good API is more than accepting requests and returning responses. That’s far from the goal. It’s about solving problems with great affordance.  You have to align usability with purpose continuously.  It’s important to note that people will use your API differently. There will be different problems people use your API to solve. You can’t change that. What you can not do is be everything to everyone.

Finally, you have to go out there and let developers understand your products’ benefits by connecting with them at various events. You have to let them see how your products make their lives easier. The tech community is competitive. It’s no longer a matter of *if you build it, they will come*.  But instead, *build, take it to them and beyond*.