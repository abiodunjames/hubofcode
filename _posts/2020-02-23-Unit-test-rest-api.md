---
title: Unit Test a REST API? Everything You Need to Know
date: 2020-02-23 10:26:35 Z
categories:
- JavaScript
tags:
- Test pyramid
- Unit tests
- Integration tests
layout: post
author: Samuel
canonical_url: https://www.testim.io/blog/unit-test-rest-api/
comments: true
description: Do you have questions about how to unit test your REST API? Read this
  to learn 3 types of tests you should have in your test suite and how to write them.
---

My little self stood in front of my computer some years ago, churning out code that today would make me scream like a baby deprived of food. The motivated me had made some changes to a 1,000-line app the previous night, and now the code just wouldn't run. If there was anything I remember, it was asking myself, "How do people build apps that are easy to change?" This was a question I solved by myself a few months later. And the answer is nothing other than rigorous and automated tests done the right way.

> Editorial note: I originally wrote this post on Testim's blog. You can check out the [original here](https://www.testim.io/blog/unit-test-rest-api/), at their site.

When we buy a new pair of shoes, we test them out—we try them on to see if they fit. Because it's through testing that we infer the quality of products. A quality REST app is an indication of rigorous testing done right. In this post, I'll show you three ways you should be testing your REST application.

## Why Writing Tests Is Important

There are several good reasons to write tests:

- We don't write code once. Working code changes often. It's through refactoring that we continuously improve a code base and design. Tests are controls that ensure that the intended behavior is preserved while you implement your changes. 
- When you refactor code, you want to make sure you're not accidentally modifying an existing behavior. Without tests, it's difficult to know when you cross the boundary of refactoring.
- Tested code gives confidence. Nothing is as terrible as customers finding the bug you should have detected during tests. The more [automated tests](https://www.testim.io/blog/test-automation-benefits/) you have in your code, the more confident you become.
- Some bugs are very difficult to catch, regardless of how good of a programmer you are. We're all humans, and we make mistakes. Automated tests help you discover these bugs early.

## 3 Types of Tests You Should Be Writing

Let's say you're building a new e-commerce app in Node.js. This application would allow users to browse products and place orders. Usually, an e-commerce app is composed of many modules and components, like catalog service, cart service, payment service, and a data store.
![img](https://res.cloudinary.com/samueljames/image/upload/v1585682998/A-software-component.png)

One thing you could do is build a complete app and later perform a manual functional test by clicking every nook and cranny to confirm that it works as expected. In the end, not only would you end up with a low-quality, buggy application that's brittle, but the building process would be highly frustrating as well.

What if you had some automated ways to verify that every bit of your code works as you code? An [automated test](https://blog.testim.io/test-automation-vs-manual-testing-picking-the-right-balance/) compares an actual outcome with the expected outcomes.

The term "testing" can be ambiguous and often means different things to different people. The fact that [different kinds of testing](https://blog.testim.io/what-is-software-testing-all-the-basics-you-need-to-know/) exist doesn't help the matter. Even if you know a few kinds, chances are there are many you might not even know exist. In this post, we'll focus on the [test pyramid](https://martinfowler.com/bliki/TestPyramid.html)—the three layers of tests you should have in your test suite and how to write them.

Mike Cohn came up with the concept of the [test pyramid](https://martinfowler.com/bliki/TestPyramid.html), a strategy for having a proper balance of automated tests on different layers of an application.

A test pyramid looks like this:
![img](https://res.cloudinary.com/samueljames/image/upload/v1585683030/test-pyramid.png)

The test pyramid has three layers: low-level unit tests, middle-level integration tests, and high-level end-to-end (UI) tests running through a graphical user interface. Ideally, you should have more low-level unit tests than high-level UI tests.

But wait a minute! What do I mean by a unit test, and how does fit into your [test suite](https://www.testim.io/blog/api-testing-tools/)? Read on to find out.

## Unit Test

Developers often have different ideas about unit testing; the individual expectation of what a unit test should be varies. Most of the time, the differences aren't in the framework used in testing. Instead, the differences lie in what's considered to be a unit of code.

In some cases, I've seen the broad line between unit test and integration test become so thin and sometimes disappear. One could ask, how can I test my APIs? If I write tests for a REST API endpoint, am I also doing unit testing?

No, you’re not, and here's why.

A unit test verifies a small portion of your code independently from other modules of your application. If you’re not writing a "Hello World” app, usually your app will contain services and modules that are interconnected.

![img](https://res.cloudinary.com/samueljames/image/upload/v1585683059/module-vs-unit.png)

A unit test doesn't test a module as a whole. It tests the smallest units that make up a more significant module.

## What to Test in Unit Testing

A unit test could assert that a method:

- Returns an expected value
- Throws an exception under the tested condition
- Changes the state of the system
- Calls another function

Isn't it okay just to test the module and leave the unit of work? Again—no, it’s not! It’s the unit of work that makes up a module. The nastiest bug I've seen in production as an engineer happened at the unit level. If your code suffers defects at the unit level, it’ll propagate to your entire application.

For example, you remember the hypothetical e-commerce app we discussed earlier. Let's say we have a price calculator module responsible for calculating the price of a product, including the shipping fee. For simplicity, we could strip this module down to something that looks like this:

![img](https://res.cloudinary.com/samueljames/image/upload/v1585683086/pricing-module-1024x982.png)

A unit test would test that function **shippingFee()** returns 20% of the base price. Let's take a moment to write a simple unit test for this method using [Jest.](https://jestjs.io/) Jest is a JavaScript testing framework created at Facebook.

![img](https://res.cloudinary.com/samueljames/image/upload/v1585683112/unit-testing-shipping-fee-1024x432.png)

If you look at the code above, we’re not testing the **PriceCalculator** behavior with respect to other services in the application. Nor are we testing the entire module; we’re testing a unit of work (a method) independent of other modules.

Because a unit test does test a unit of code, it’s usually fast, and it always should be. If your unit tests take longer to run, chances are you’re doing something wrong.

Now that we've seen what a unit test should be and how to write it, it’s also essential that you test that your code still works after integrating with other components. Could there be a chance of conflict after integration? Nobody knows, and this why you should also write integration tests.

## Integration Test

Working software consists of different modules and components in synergy. From the database layer down to the presentation layer, one component depends on another. The more interconnected the parts, the higher the likelihood of conflicts or something going wrong. An integration test combines individual units of work and tests them as a group. It could be testing that when a user accesses a path with the ID of a product, the product details are returned as JSON.

In an e-commerce app, you may want to return product details when a user clicks on an item. There could be many things going on under the hood. For example:

- A database service may retrieve the product details from the database.
- A pricing service could calculate the product price and shipping fee, taking the user's location into play.
- An analytics service could track user interaction with the product.

Assuming the snippet below does all of these, an integration test will test that these components work well together.

![img](https://res.cloudinary.com/samueljames/image/upload/v1585683140/endpoint-1-1024x379.png)

Let's take a moment to see how we could test this endpoint using [supertest](https://github.com/visionmedia/supertest), a node package that allows you to test HTTP servers.

The test would look like this:

![img](https://res.cloudinary.com/samueljames/image/upload/v1585683185/integration-testing-1024x541.png)

When you perform integration testing, it's important that you verify:

- The HTTP status code
- The response payload
- The response headers
- The API performance/response time

That said, let us explore the UI test.

## UI Tests

In the test pyramid, the UI test stands at the top because it's the type of test you write after all modules and components have been integrated. Unlike the unit test or integration test, a UI test isn't limited to a module or a unit of your application; it tests your application as a whole. It simulates real user actions.

It's a test performed to ascertain that an app runs as expected and meets the system requirements. It's the slowest and the most expensive test because it replicates real user actions in the browser. You can't write good UI tests without a full understanding of your app's requirements and end goals.

A UI test for our e-commerce app could verify that users can access the homepage and, within a certain amount of time, click on a link to view a product. We could do this with [Nightwatch.js](https://nightwatchjs.org/), an end-to-end testing package for Node.js apps, as follows:

![img](https://res.cloudinary.com/samueljames/image/upload/v1585683226/ui-test-1024x783.png)

## Conclusion

In conclusion, the UI tests verify that the overall system meets requirements, but they're slow, expensive, and can inhibit your agility. Hence, you should write more low-level tests and keep your UI tests lean. Integration tests lie in the middle of the pyramid. Although they don't go into details of the app as much as UI tests do, they're good for pinpointing when integration between components leads to undesired behaviors. On the other hand, unit tests are fast, cheap, easy to write, and efficient. They're quick to show where things go wrong but not good at detecting misbehavior on the integration level.

Finally, if you want to take a look at the code used in this post, you can find it [here.](https://github.com/abiodunjames/testim.io-unit-test-api)