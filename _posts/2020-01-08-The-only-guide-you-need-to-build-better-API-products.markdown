---
layout: post
title: "The only guide you need to build better API products"
date: '2020-01-08 10:26:35'
categories: General
author: 'Samuel'
comments: true
tags:
- API 
- agileprocess
- API products
- API design
---

![](https://res.cloudinary.com/samueljames/image/upload/v1578483090/template_6.png)

In the 19th century, [Le Corbusier](https://en.wikipedia.org/wiki/Le_Corbusier), a French architect, made public a visualization that changed how we live. 

This plan opened the door to modern urban planning today. Around 1930,  public health became a significant concern; the poor working conditions of municipal workers in congested and polluted cities.

Le Corbusier's goal was to reform the working conditions of urban dwellers, which led to a [diagram](https://www.citylab.com/design/2012/11/evolution-urban-planning-10-diagrams/3851/) (design) that became radical in 1930.

[Benjamin Grant](https://www.spur.org/about/staff/benjamin-grant), a public realm and urban design program manager for San Francisco, described it as taking a complex set of issues and providing us with excellent communication of the solution. 

He added, "An oversimplification of complex problems." 

And to this day, Le Corbusier's plan is the foundation of modern urban planning that inspired how our cities are built.

Why?

**It's simple, and it solves a problem.**

Again, what does this have to do with API?

Well, it does have much to do with building API.  In history,  simplicity and problem solving have always been the driver of adoption whichever way  you look. 

Successful API products have few things in common.

1. They are simple and self-descriptive.
2. They are designed with users in mind.
3. They are fast.

At one point in time, every developer has integrated with at least an API that makes you feel you should buy its developers a bottle of beer. 

And some that make you think whosoever developed it owes you a bottle of beer for even trying to integrate.

If we look at the problem, we'd know that poorly designed APIs don't fall off the sky. Processes birth them. 

* The process does not make API a first-class citizen.
* API is simply a by-product of code.
* API is something that is generated after writing a ton of code. 
* No planning is put into it.
* Lack of collaboration during the design phase.


Well, does it matter as long as the API is running in production? 

Well, it does matter. Poorly implemented APIs have consequences. 



## Why poorly implemented APIs are bad

#### Reduce adoption

I have not seen too many APIs built not to be used. We build APIs for others to use. We can say that one of the goals of any API project is adoption. Interestingly,  poorly implemented APIs drive away adopters. 

#### May not reflect the needs of end-users

API solves business problems. An API project without proper planning may overlook or not capture all the needs of its users. An API is much more than exchanging data between machines. It's about providing business capabilities that solve users needs. 

#### Difficult to extend

>  A doctor can bury his mistakes, but an architect( API engineer) can only advise his clients to plant trees. 

Once API is built and released. It can not be overhauled else you would break backward compatibility. 

Bad API is difficult to extend. An ill-designed API can also come back to bite you.

#### Hard to learn

An unplanned API project is challenging to master and often lacks clear and comprehensive documentation. A source of constant head-ache to its users.


> Poorly designed APIs don't fall off the sky. Processes birth them. 
>
> 

Building an API product goes beyond establishing a set of contracts on how API should behave and then rolling to production.  Exposing a set of contracts for API is one thing; designing and planning  API that is easy to use and solves business problems is another. 

## How to build better API products

#### Start with the "why."

Why do you need an API?  What business capabilities will it provide its user?  Every API product should have a business goal. If you can not explain why your API is valuable to the business, then there is no need to create it in the first place.

 If the "why" of your API is not clearly defined, most likely, developers are going to have a hard time figuring out what to do with the APIs.

#### Design API specifications and with users in mind.

More often than not, we are tempted to skip this step and go directly to writing code. After all, it easy to generate API specification from code these days. Design is as important as development. We (engineers) often prioritize stability, scalability, and efficiency over planning. Don't get me wrong; these are not bad themselves; in fact, they are critical to any great API products. 

Stability and scalability are useless; an API product does not serve its purpose. It's important to take adequate time in designing how your API will be used and what it should look like from the end-user perspective.

There are many tools like [OpenAPI specification](https://swagger.io/docs/specification/about/) to describe the interface of your API before development starts, it does not matter what you use, but it does matter you follow industry standards. Don't reinvent the wheel.

#### Seek feedbacks

API is never about developers alone. There are stakeholders, and you should involve them in the process. 

Before writing your first line of code, present your design to the stakeholders,  incorporate their feedback, and reiterate.

#### Create a mocked version of your API

A mocked version of your API allows you to seek early validation before development commences. Early bugs can be caught. In addition, it fosters communication and collaboration within teams. It gives room to parallelize development within teams.

#### Implement the API

Finally, write the code you've always wanted to write and follow best practices.  

