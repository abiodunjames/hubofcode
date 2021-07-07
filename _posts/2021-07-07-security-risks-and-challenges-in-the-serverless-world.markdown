---
title: Security Risks and Challenges in the Serverless World
date: 2021-07-07 06:41:00 Z
published: false
categories:
- Serverless
- Security
tags:
- engineering
- software
---


Adopting an architecture that gives you complete control over your application and infrastructure (servers, identity management, etc.) is good because of the flexibility it offers, but it’s only sustainable for a while. As your organization grows, things will start to get complicated, and scaling and infrastructure management will become a big challenge. Instead of delegating these responsibilities to developers, [why not adopt serverless](https://iamondemand.com/blog/aws-fargate-for-running-serverless-applications-on-kubernetes/)? This will allow you to shift the responsibility of managing your application infrastructures to a cloud provider.

Going serverless offers numerous benefits, such as greater scalability, faster time to market, lower operational overhead, and automated scaling—all at a reduced cost. But serverless also comes with some challenges. Like with any technology, serverless applications are susceptible to malicious attacks that can be difficult to protect against. According to an [audit by PureSec](https://www.networkworld.com/article/3268415/one-in-five-serverless-apps-has-a-critical-security-vulnerability.html), 1 in 5 serverless apps has a critical security flaw that attackers can leverage to perform various malicious actions.

> **Editorial note: This post was initially published on Iamondemand’s blog. You can check out the** [original here](https://iamondemand.com/blog/security-risks-and-challenges-in-the-serverless-world/)**, at their site.**

Having built a number of serverless applications,  in this post, I’ll share some of the best practices I’ve found to be useful for mitigating security risks.

## Serverless Attack Vectors

Serverless applications are almost never built on functions as a service (FaaS) alone. Rather, they also rely on several third-party components and libraries, connected through networks and events. Every third-party component connected to a serverless app is a potential risk, and your application could be easily exploited or damaged if a component is compromised, malicious, or has insecure dependencies.

Instead of securing serverless applications using firewalls, antivirus solutions, intrusion prevention/detection systems, or other similar tools, focus on securing your application functions hosted in the cloud. While the cloud provider provisions and maintains the servers that run your code and manages resource allocation dynamically, you still need to ensure that your app is free of the following vulnerabilities, which are unique to serverless:

* **Data vulnerabilities:** Vulnerabilities that arise due to the movement of data between app functions and third-party services. These vulnerabilities are also introduced when you store app data in non-secure databases.

* **Libraries vulnerabilities:** Security vulnerabilities that are introduced when a function uses vulnerable third-party dependencies or libraries.

* **Access and permission vulnerabilities:** Vulnerabilities that are introduced when you create policies that allow excessive access or permissions to sensitive functions or data.

**Code vulnerabilities:** Vulnerabilities that are introduced when you write bad codes or vulnerable serverless functions.

## Security Risks and Challenges in the Serverless World

As more enterprises adopt and build applications using serverless architectures, it’s really important to keep serverless deployments and services secure. Unfortunately, many enterprises aren’t aware of the security risks in serverless applications, not to mention crafting strategies for mitigating those risks. In this section, I’ll discuss some critical security risks to consider when running serverless applications.

### Inadequate Monitoring and Logging of Serverless Functions

Serverless apps operate amid a complex web of connections and use different services from various [cloud providers across multiple regions.](https://iamondemand.com/blog/building-a-multi-region-serverless-app-with-aws-appsync/) In a serverless application, insufficient function logs lead to missed error reports. Because serverless functions communicate across a network, it’s very easy to lose track of the audit trail or event flow that you need in order to detect and identify what’s happening within the app.

What’s more, without proper monitoring and logging of serverless functions and events, you won’t be able to identify critical errors, malicious attacks, or insecure flows on time. Eventually, the delay will lead to app downtime that could affect your customers or brand reputation.

### Sensitive Data Exposure Due To a Large Attack Surface

Serverless applications have a large attack surface and comprise hundreds, or even thousands, of functions that can be triggered by many events, including API gateway commands, data streams, database changes, emails, IoT telemetry signals, and more. Serverless functions also ingest data from various third-party libraries and data sources, the majority of which are difficult to inspect using standard application-layer protections, such as web application firewalls.

There are many factors that increase entry points to serverless architectures, including the vast range of event sources, large number of small functions associated with serverless apps, and active exchange of data between deployed functions and third-party services. In addition, all of these factors combined increase the potential attack surface and risk of sensitive data exposure, manipulation, or destruction.

### Function Event-Data Injection

At a high level, a function event-data injection attack occurs when a hacker uses hostile, untrusted, and unauthorized data inputs to trick an app into providing authorized access to data or executing unintended commands. A serverless application is vulnerable to fault injection attacks when it allows malicious user data to slip through the cracks without filtering, validating, or sanitizing that data.

These injection attacks can lead to access denial, data corruption, data loss, and even complete host takeover. In extreme cases, the hacker can take total control of an app’s high-level execution and modify its regular flow via a ransomware attack. Some common examples of function event-data injection attacks associated with serverless architectures are:

* SQL and NoSQL injection

* Server-side request forgery (SSRF)

* Object deserialization attacks

* Function runtime code injection (e.g., Golang, C#, Java, JavaScript/Node.js, Python)

* XML External Entity

As you would imagine, [serverless functions](https://iamondemand.com/blog/google-cloud-run-vs-aws-lambda-is-cloud-run-a-serverless-game-changer-part-1/) aren’t immune to the previously mentioned security threats and risks. Your app will still be vulnerable if you have functions or code that use excessive permissions or don’t follow security best practices.

## Best Practices for Securing Serverless Applications

So how do you secure a serverless app? First, know that designing and implementing security into your app should always be a top priority—even with serverless architectures. Since you’re responsible for managing some parts of your serverless app, you need to adopt best practices that allow you to secure it against attacks, insecure coding practices, errors, and misconfigurations. Here are a few tips to get you started.

### Adopt the Principle of Least Privilege

One way to secure serverless applications is to ensure proper authentication and authorization, allowing each function to access only the minimum permissions it needs to operate well or perform an intended logic. With the principle of least privilege, you grant only enough access required for a function to do its job. Setting out rules for what each function can access is essential for maintaining security in serverless architectures.

This also allows you to minimize the level of security exposure for all deployed functions and mitigate the impact of any attack. Least privilege access also ensures that each function does exactly what it was designed to do, helping you maintain compliance and improve your security posture.

### Monitor and Log Functions

Once you start to use serverless architecture, where the provider takes care of tasks like infrastructure maintenance and scaling, you may discover that things start to move quite quickly, as there is less work to do. Also, because serverless functions are stateless and event driven, it’s very easy to miss most suspicious activities if you don’t have a good monitoring strategy. A better approach for preventing, detecting, and effectively managing security breaches is to adequately log and monitor security-related events.

You can collect real-time logs from different cloud services and serverless functions, as well as periodically push the logs to a central security information and event-management system. Most cloud providers have a comprehensive log-aggregation service you can leverage. That way, it’s easier to do an audit trail that you can reference whenever you need to hunt security threats. When monitoring your serverless functions, you should collect reports on resource access, malware activities, network activity, authorization and authentication, critical failures, and errors.

### Define IAM Roles for Each Function

In some cases, a serverless app will contain hundreds, or even thousands, of functions, which makes managing roles and permissions a time-consuming and tedious task. In a bid to make this less demanding, some enterprises fall into a trap: setting a single, wildcard permission level for an entire app that consists of tons of functions. This approach might seem less harmful when experimenting in the sandbox environment, but it can be very dangerous. In fact, it actually increases the security risks faced by serverless applications, as most code in the sandbox environment finds its way to production.

As you adopt a serverless architecture, you need to think about each function individually. You should also manage individual policies and roles for each function. As a rule of thumb, every serverless function within your application should have only the permissions it needs to complete its logic—nothing more. Even if all your functions have or begin with the same policy, you should always decouple the IAM roles to ensure least privilege access control for the future of your functions.

## Summing Up

New opportunities pave the way for new challenges, and serverless computing is no exception. Despite the security challenges and risks, serverless architecture is a very exciting technological evolution in the world of infrastructure and a boon to many enterprises.

To address and mitigate security risks, you need to understand the serverless attack vectors and the unique challenges in serverless environments. Most importantly, you need to “[shift left](https://www.checkpoint.com/cyber-hub/cloud-security/what-is-shift-left-security/)” and integrate security throughout the entire software-development lifecycle.

All serverless applications work under the shared responsibility model, where compliance and security are a shared responsibility between the cloud provider and application owner. The cloud provider is responsible for securing the serverless infrastructure and cloud components (servers, databases, data centers, network elements, the operating system and its configuration, etc.). You are responsible for securing the application layer by enforcing legitimate app behavior, managing access to data and application code, monitoring for security incidents and errors, and so on.

Clearly, you need to invest heavily in securing your app before you can reap the benefits of serverless. In this article, I discussed the security risks you should watch out for and the best practices you should adopt to keep your serverless environments secure and safe against insecure coding practices, errors, and misconfigurations. Good luck!