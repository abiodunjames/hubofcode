---
title: How We Built a Serverless E-Commerce Website on AWS to Combat COVID-19
date: 2020-09-12 06:52:00 Z
categories:
- Serverless
tags:
- aws
- serverless
- product
- snipcart
- ecommerce
canonical_url: canonical_url
Field name: https://medium.com/swlh/what-we-learned-from-building-a-serverless-e-commerce-website-on-aws-to-combat-covid-19-2b66155f9b08
comment: true
author: samuel
Key: 
---


![Image for post](https://miro.medium.com/max/1962/0*WcEW2tF0Rf7k21Kr)

2020 turned out to be radically different from what everyone had expected. COVID-19 has impacted our lives in many ways. As the pandemic spread, one question that baffled us was: what can we do with technology to combat the virus?.

A few days later, we got the answer.

In a phone conversation in March 2020, [Olalekan Elesin](https://medium.com/u/98b05140c1ad?source=post_page-----2b66155f9b08----------------------) informed me of an idea he thought about while doing his regular grocery shopping at [DM Drogrie](https://www.dm.de/). The idea is centered on a bracelet like your typical watch that dispenses Sanitizer. Before this time, Sanitizer comes packaged in bottles and cans of different sizes and kinds. Some are mounted on walls, and some are portable enough to be carried around in handbags. But not portable enough to be worn on the wrist.

Having spent years building scalable applications and data applications at various startups, we understood that every idea must be validated. We have to know it’s an idea that people want and are willing to pay for. As for us, this means doing some market research.

# **Problem Discovery and Customer Validation**

We started out with one goal split into two hypotheses: Is this a big enough problem, and are customers willing to part with cash in exchange for the product. Validating such assumptions with software/digital products is relatively straightforward. One could build a simple landing page, explain the idea, and add a form — the lean startup way. But for physical products, this is quite different. Developing MVP could mean designing and 3D-printing hand bracelets.

After several iterations in coming up with the leanest and cheapest way to test, we arrived at one: [blogpost as MVP](https://medium.com/@elesin.olalekan/maintaining-hand-hygiene-with-new-sanitizer-bracelets-d52bc0e8f647).

In the blogpost, we embedded a pre-order Google form to collect some information about potential customers. The form included the color, price, payment method and quantity fields.

> While doing market research, it’s essential to collect potential customer contact details like email addresses, etc. If your product resonates with people, potential customers will not hesitate to give their contact details. Customers who provide contact details at the pre-launch phase are usually the first to be converted when you go live.

Before our product hit the market, we recorded over **$100,000** in pre-order bookings from matured markets such as the USA, Germany, Italy, France, and the United Kingdom through the medium post with zero ad budget. More than **85%** of customers were willing to pay online and have their [SanitizerWristbands](http://sanitizerwristbands.com/) delivered to them. We validated our problem and customer hypotheses with these and other leading indicators: big enough pain, and customer willingness to pay. This informed our next decision to develop the first physical MVP.

# **Manufacturing: An Uncharted Territory**

Physical goods manufacturing was uncharted territory for us. It was not long before we realized it’s different from everyday app development. In manufacturing, you design and create a product and then replicate it repeatedly. In software, the product design is the product. This is not to say one can not apply certain software development principles to manufacturing. The concept of lean, which is popular in tech, originated from the Toyota production system.

With our first product design ready, we spoke to manufacturing companies to build the prototype. We learned quickly that our design was too cumbersome and would cost a substantial amount to produce.

![Image for post](https://miro.medium.com/max/60/1*dE_ooPtDYnmOdA9y42mYuA.png?q=20)

![Image for post](https://miro.medium.com/max/4768/1*dE_ooPtDYnmOdA9y42mYuA.png)

Figure 1: An Earlier Iteration

We arrived at the simplest possible design that works through customer feedback and trying to cut down the production cost to match the amount customers are willing to pay for.

We were ready and it was time we put our store online.

# **Scalable eCommerce Website With 0$ Commitment**

There are many e-commerce platforms for online retailers that don’t require technical expertise. [Shopify](https://www.shopify.com/) and [Square](https://squareup.com/us/en/ecommerce) are popular choices. We wanted a platform that would allow us faster access to the market with little to zero cost commitment. Shopify and other popular platforms didn’t meet our cost requirements, so we built a custom solution in 2 days with 0$ commitment. It’s a static website hosted on s3 with a serverless cart and inventory capabilities provided by [Snipcart](https://app.snipcart.com/register?utm_source=hubofcode&utm_medium=referral&utm_campaign=aws-post).

The high-level architecture looks like this:

![Image for post](https://miro.medium.com/max/60/0*cT0BYaOGzG7KTqOW?q=20)

![Image for post](https://miro.medium.com/max/1446/0*cT0BYaOGzG7KTqOW)

Figure 2: E-commerce Website Architecture

Right from the start, our goal was to automatically deploy changes made to our website whenever a PR is merged. We leveraged AWS CodeBuild and CodePipeline to achieve this and automatically deploy new changes to an S3 bucket.

![Image for post](https://miro.medium.com/max/60/0*61hii4IAT51buwJw?q=20)

![Image for post](https://miro.medium.com/max/1962/0*61hii4IAT51buwJw)

Figure 3: CI/CD with AWS

We sent a campaign email to customers who had pre-ordered before we launched.

# **Automating Orders Fulfilment**

With orders coming in, there was yet one more problem we had to solve: automating orders fulfilment.

Our warehouse and logistics partners are in China. This implies we must find a way of notifying them every time an order is placed. For a few orders per day, sending an email with an attachment to our logistic partners suffices. As the number of orders per day grows, this becomes really boring and automating the process became a necessity. Could we automate it?

To automate fulfilment of orders, we created a serverless “order fulfilment addon” whose architecture looks like this:

![Image for post](https://miro.medium.com/max/60/0*WcEW2tF0Rf7k21Kr?q=20)

![Image for post](https://miro.medium.com/max/1962/0*WcEW2tF0Rf7k21Kr)

Figure 4: Automating Order Fulfilment (WIP)

# **Making Informed Decisions with User Behaviour**

Our conversion goal was to get potential customers to buy our product. To reach this goal, we needed to learn about the things that interrupt user flow from right from landing on the website to actually placing an order. For us, this means conducting several A/B, Multivariate tests weekly and implementing what gets us closer to our goal.

------

# **Lessons Learned**

While we’re yet to achieve a massive scale like the popular e-commerce platforms, we’ve learned some valuable lessons that could spark new ideas for people looking to start online businesses.

- When it comes to manufacturing, you’ll save a sizeable amount of money by manufacturing from developing countries (like India, etc.) than from developed countries like the US, Germany, etc.
- If you’re in Germany, registering a business can take a long time. If you plan to get to market fast, ensure you factor in the time, it will take you to navigate the bureaucracy.
- Patent your ideas before doing market research. We started with market research. After we saw that the idea was valuable, we wanted to move on with patenting; unfortunately, it was too late. Someone already filed a patent
- China makes manufacturing look like a piece of cake.
- Data is king. “In God we trust, all others bring data” — Edwards Deming. It’s important to start with a data-driven mindset, no matter how small. Think of how to decompose big ideas into testable components and figure out the data required to validate along the way.
- Get your analytics right early! This is not referring to Google Analytics. Consider the metrics that are indicative of your success and instrument them from the get-go. Great resource — [$9 Marketing Stack: A Step-by-Step Guide](https://robsobers.com/9-marketing-stack-stepbystep-guide-archived/).
- Experiment like no one cares. For us, we were always experimenting, we have a minimum of 5 tests running weekly. A/B tests, multivariate tests, Redirect tests, etc. You can do this for free with Google Optimize.


