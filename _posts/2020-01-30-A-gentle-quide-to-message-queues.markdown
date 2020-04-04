---
layout: post
title: "A gentle guide to message queues"
date: '2020-01-30 10:26:35'
categories: General
author: 'Samuel'
comments: true
description: "A message queue is temporary message storage that facilitates the exchange of data between a producer and a consumer in an asynchronous manner"
tags:
- Message Queues
- AWS SQS
- Serverless
---

The word "queue" is used everywhere. It has all kinds of meanings in the real world and even in the world of software development.


Last night, a friend pulled me aside and whispered, "Why do you hate queues so much?" I stared at him for a while and blurted, "Why shouldn't I? I'm a [millennial](https://luckyattitude.co.uk/millennial-characteristics/)! Time is money". 

Everything quick! Isn't it? we want to grab our lunch first in a fast restaurant! We want the paycheck to drop early in our accounts. We save time to waste it later, playing video games at night. 



But today, I'm not talking about waiting in line nor about the queues at [Burger Kings](https://www.burgerking.com/).  I'm talking about the invisible message queues that handle your payment notification when you place an order on an e-commerce site.  A vital component of the infrastructure that allows you to check on your crush on social media. You're wondering how I got to know that? I'm a f**cking wizard! 

But before I proceed, on a personal level, I want to ask you a question.

What is a message queue? 

*Well, I'll bail you out.*


> A message queue is temporary message storage that facilitates the exchange of data between a producer and a consumer in an asynchronous manner. 

![](https://res.cloudinary.com/samueljames/image/upload/v1580362156/Untitled_Diagram_4.png)


Now, let me tell you a secret. Those senior demigods (software architects) you see on your screen telling you the next big thing in tech hardly do away without a message queue in their architecture. 

But wait, what is such a big deal with message queues?

### It allows you to process tasks asynchronously 

See, we're all in haste in this world. Some tasks take too long to process that even my 80 years old Grandma wouldn't care to wait. You can't only continue to stare at your small screen while the mysterious backend application handles your tasks.  

What if you hand over your tasks and continue with your daily life while the application runs tasks and hand out results when completed? Yeah! Sound cool?  A message queue allows you to do that. It allows message consumers to process your tasks in the background while you continue to do what you want. 



Think of building an e-commerce application; you may want to send an email to your customers after a successful order. You could have an order service responsible for processing orders and a notification service that sends an email notification to customers after.

![](https://res.cloudinary.com/samueljames/image/upload/v1580363917/Untitled_Diagram_5.png)


When an order is processed, the order service produces a message in the message queue, notification service reads, acts on this message and  in turn sends an email notification.

### It allows you to decouple your architecture

Decoupling and scaling are not synonyms. It's interesting that hardly can one go without another. Have you ever had that particular senior engineer in your team who looks at everything from a telescopic lens of scaling every time? This guy does not like the word "coupling" as such. He tends to see it 100 millions miles away. Do you know why? When you have everything all coupled together, it's challenging to scale — a total nightmare.

Things tend to get better when there is a clear separation of responsibility.  

A message queue allows you to decouple your message producers and consumers. If your message producer needs scaling, you can scale independently.

![](https://res.cloudinary.com/samueljames/image/upload/v1580364038/Untitled_Diagram_6.png)

### It makes your architecture resilience.

If you're reading this at 3 am just after a call that your server is down, please take heart, you're not alone. 

Even [WhatsApp](https://twitter.com/search?q=%23WhatsAppDown&src=typed_query) was down.

We all face downtimes. What separates the snoring people in their comfy bed at the moment from you is the system they've put in place to mitigate downtimes. 

The architecture they have that prevents the entire business from being thrown offline when downtime knocks. If you have your message producers and message consumers all coupled up, and something goes wrong with the producers, it'll affect the consumers as well. 

A message queue allows you to have other components of your application up and running when a service suffers a failure.

There are even many more reasons why a queue system is essential. I recommend you read [more here](https://blog.iron.io/top-10-uses-for-message-queue/) later.

For now, let's talk about what you need to know before using a queue system.

## Using a Queue System

Brian Zambrano, in his book serverless design patterns and best practices, wrote some questions you should have answers to before using a queue system. Knowing these could help you make better design choices when using a message queue.

Before you use a queue system, you should have answers to the question:

* What type of data fetching model is supported by the queue system, pull or push?

* What is the maximum lifetime of messages or the maximum queue depth?

* What happens with messages that consistently fail? Is there a dead-letter queue option?

* Is there a guarantee of exactly-once delivery, or can messages be delivered multiple times?

* Is ordering guaranteed, or can messages arrive out of order relative to the order from which they were sent?

  

If you've read this post to this point, I think you're great and I want to be you when I grow up more. 

But wait, this is not the end to this post.  Part two of this post is coming.   And I must tell you that in part two, we'll choose a message queue and integrate with it.  We'll write the code you've always wanted and talk  on cost optimization tricks.
