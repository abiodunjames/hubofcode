---
title: What Is Serverless Architecture and When Should You Use It?
date: 2019-12-20 10:26:35 Z
categories:
- Serverless
tags:
- Serverless Architecture
- Use cases of serverless
- AWS Lambda
- Google Cloud Functions
- PaaS vs FaaS
layout: post
author: Samuel
comments: true
---

Cloud computing is constantly evolving, from bare-metal to container technologies. The latest trend in this process is the serverless (Function as a Service, or FaaS) computing model. According to [Techbeacon](https://techbeacon.com/enterprise-it/state-serverless-6-trends-watch), serverless has an annual growth rate of 75%, making it the fastest growing cloud service model. So, serverless architecture isn't a mere buzzword. More companies than ever are adopting it.


If you're not sure what serverless architecture is, you've come to the right place. Read on to learn what serverless architecture is, when it makes sense, and when it doesn't.

> Editorial note: I originally wrote this post on Scalyr's blog. You can check out the [original here](https://www.scalyr.com/blog/serverless-architecture/), at their site.

## What Is Serverless Architecture?

The term *serverless architecture* can be confusing. It doesn't mean application designs that allow for running applications in some magical space where servers are nonexistent.

[Serverless architectures](https://martinfowler.com/articles/serverless.html)are application designs that make use of third-party services (Back end as a Service, or BaaS). They may use custom code run in managed, ephemeral containers on a FaaS platform.

Servers still run your application. But a third-party company takes care of the grunt work of provisioning, managing, and scaling servers. In serverless architecture, you manage and provision nothing.

Serverless architecture often incorporates two components: [Function as a Service](https://www.scalyr.com/blog/function-as-a-service-faas/)and Backend as a Service.

- FaaS is a computing service that allows you to run self-contained code snippets called functions in the cloud. Your functions remain dormant until events trigger them. Functions are self-contained, small, short-lived, and single-purpose. They die after execution.
- BaaS is a cloud computing service that completely abstracts backend logic, which takes place on faraway servers. It allows developers to focus on front-end code and integrate with back-end logic that someone else has implemented. BaaS could be authentication, storage services, geolocation services, user management, and so on.

![Serverless Architecture is composed of BaaS and FaaS](https://res.cloudinary.com/samueljames/image/upload/v1570512883/Key-services-3-1024x591.png)

In serverless architecture, you focus on writing code only. You deploy when you’re ready, without caring about what runs it or how it runs.

Is serverless the same as Platform as a Service?

## Serverless and Platform as a Service (PaaS): What's the Difference?

Many people mistake Platform as a Service (PaaS) for serverless architecture. Although they are similar in many ways, they aren't the same.

PaaS providers offer software and hardware infrastructure as a platform to users—what many people call a solution stack. That means you can run custom applications using the provider's platform. Serverless, on the other hand, provides an environment where you can write and run custom code (functions) without managing, provisioning, or scaling infrastructure.

What do Paas and serverless architecture have in common? You don't manage infrastructure in either one. The platform provider takes care of that. However, differences exist in how you compose your application and how you scale it.

### **Composition**

In the PaaS model, you write your application using a framework or language that your platform provider supports, and you deploy your business logic as a single unit. In the serverless model, your business logic is broken down into self-contained units that each perform a business function.

### **Scaling**

Scaling isn't automatic for PaaS applications. Instead, you have to configure your app and add resources to handle more requests. In serverless architecture, on the other hand, your app scales automatically as the workload increases.

### **Lifetime Availability**

Like traditional applications, PaaS applications have to be available at all times to continue to serve requests. In serverless, by contrast, functions are short-lived—they die after execution.

Now you understand that PaaS isn't serverless, though the two systems are similar in some ways. But why would you want to go serverless?

## Why Use Serverless Architecture?

As you ponder this question, consider these three key attributes of serverless architecture.

- **It's scalable and highly available.** Scaling traditional applications requires you to understand your traffic pattern. You estimate how much of each resource you'd need, and then you'd provision accordingly. Users troop in from all geographical regions to use modern applications. A traditional application could be overwhelmed by a spike—probably on a black Friday. In serverless, your application is highly available, and it scales automatically as your users grow and usage increases.
- **It costs less.** One of the reasons serverless architecture is gaining popularity among startups is because of its pricing model. The cost of running servers 24/7 and paying for idle time is no longer an issue in serverless. You pay for usage only. Functions have allocated time in which they run and die afterward. The provider charges based on the number of executions and the size of memory your workload uses. This helps you optimize costs.
- **The time to market is faster.** Operational tasks such as server provisioning, maintenance, and monitoring infrastructure are off your shoulders. You can focus solely on your business logic (code), experiment with ideas, and hit production on time.

With all the promises of serverless, is it perfect for all use cases? No. In some cases, serverless architecture makes sense, but in others, it might not. We'll discuss this point in detail later on in this post.

Let's explore some common use cases of serverless architecture.

## Serverless Architecture Use Cases

In a traditional application, your application runs actively on servers. Since your servers are on 24/7, you also pay for idle time. In the serverless world, though, there are no servers to pay for. You only pay per trigger. In other words, you pay for what you use.
This advantage makes serverless architecture a good option for business cases that don't have to be always on. In this way, you save money from not paying for idle time.

Examples of serverless architecture use cases are:

### High-Traffic Websites

If you're still serving your static websites from an [EC2 instance,](https://aws.amazon.com/ec2)you may be missing out on a lot. With serverless, you can host your static website on S3 bucket and serve your assets with a global, fast cloud delivery network. Not only is it cheaper and fast, but it is also highly available and scalable.

### Multimedia Processing Applications

If your business deals with images and videos, then serverless architecture might work well for you. You can use a scalable storage service such as [AWS S3](https://aws.amazon.com/s3/)to store your data. An upload event can trigger a lambda function after each successful upload that processes your file asynchronously. Your users can continue to enjoy your app while a highly available and scalable back-end service processes the upload in a non-blocking way.

### Mobile Backends

An API gateway gives you an entry point to your business functions. These functions can be exposed as rest API that your mobile app consumes. Serverless services such as [AWS AppSync](https://aws.amazon.com/appsync/)allow you to securely access, manipulate, and combine data from multiple sources in real time.

### Internet of Things (IoT)

IoT devices generate a lot of data from their environments through sensors. Organizations often struggle to process this overwhelming data coming from these connected devices in a scalable way. Using a serverless back end like [AWS IoT Core](https://aws.amazon.com/iot-core/), you can scale to billions of devices and trillions of messages.

### Big Data Applications

Before cloud computing, the insights big data provided were available only to big enterprises because organizations needed the infrastructures' overheads to make sense of that data. Setting up and maintaining infrastructures for big data isn't easy. With serverless computing, your app can now take advantage of several services, including [Amazon S3](https://aws.amazon.com/s3/), [Amazon Athena](https://aws.amazon.com/athena/), [Amazon Kinesis](https://aws.amazon.com/kinesis/), [AWS Glue](https://aws.amazon.com/glue), and [AWS Lambda](https://aws.amazon.com/lambda)to build scalable data pipelines.

Earlier, I mentioned that serverless architecture isn't a silver bullet. There are cases where serverless might not be a good fit.

## When Going Serverless Might Not Make Sense

With all the promises of serverless architecture, it has its drawbacks and limitations too. It's important to keep these in mind when considering serverless architecture and plan accordingly.

1.  There's a limit to how long a function can run. This makes serverless architecture unsuitable for tasks that run for hours.
2. What if you require a fast response with a consistent latency of less than 50 milliseconds? Serverless architecture suffers from the problem of [a cold start](https://mikhail.io/serverless/coldstarts/define/). In a case like this, you might need to reconsider your options.
3. You may be afraid of vendor lock-in. Serverless architecture could tie you to a single vendor. Migrating serverless applications from one vendor to another requires a lot of manual effort and major changes in your code.
4.  Serverless vendors limit how much memory you can assign to functions. If you require a complex compute with high memory requirements, then going serverless might not be a good use case.
5.  Serverless vendors enforce hard limits on deployment size. This could vary from one vendor to another. For example, AWS allows a code deployment size up to [250MB](https://docs.aws.amazon.com/lambda/latest/dg/limits.html). For large code deployment, serverless architecture might not be suitable.
6. Serverless is still relatively young. For this reason, [observability](https://www.scalyr.com/blog/observability-production-systems-why-how/)is still a problem. Having 360-degree insights into your functions can be difficult.

## Conclusion

Before moving to serverless architecture, you must first evaluate your use cases. It's critical to understand what your business requirements are and whether serverless architecture would make sense for your project and some of the limitations you'd hit down the road. When you have this information, you can better prepare.

In this post, we've seen the advantages of serverless architecture. We also discussed some of the drawbacks of serverless architecture and some typical serverless use cases. To build on this knowledge, I encourage you to read these sources:

- [Ten things serverless architects should know](https://aws.amazon.com/blogs/architecture/ten-things-serverless-architects-should-know/)
- [AWS Lambda use cases: 7 reasons Devs should use Lambda](https://www.scalyr.com/blog/aws-lambda-use-cases/)
- [How serverless will change DevOps](https://www.scalyr.com/blog/how-serverless-will-change-devops/)

 