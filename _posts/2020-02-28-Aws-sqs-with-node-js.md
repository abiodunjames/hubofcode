---
title: Using AWS Simple Queue Service From Your Node.Js Application
date: 2020-02-28 10:26:35 Z
categories:
- General
tags:
- Message Queues
- AWS SQS
- Serverless
layout: post
author: Samuel
comments: true
description: Learn how to integrate and optimize costs with AWS Simple Queue Service
  (SQS)
---

In the [previous post](https://hubofco.de/A-gentle-quide-to-message-queues/), we talked in detail about queue systems and why they're essential in scaling applications.

Today, we'll take it a bit further. We'll look at  [AWS Simple Queue Service (SQS)](https://aws.amazon.com/sqs/), a managed queue system provided by AWS, and how you can integrate with it  from your node.js application. During this tutorial,  If you ever feel a need to look at the source code, you'll find it [here](https://github.com/abiodunjames/AWS-SQS-Node-Js).



According to [AWS documentation](https://aws.amazon.com/sqs/),   AWS SQS is a fully managed message queueing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS eliminates the complexity and overhead associated with managing and operating message-oriented middleware and empowers developers to focus on differentiating work. 

### Standard Queue and FIFO Queue

Before we go further, it's essential to know that AWS SQS has two types: Standard Queues and FIFO queues.  Depending on your use cases, you might find yourself having to choose one type over another. 

Standard queues and FIFO queues have many similarities, as well as many differences.   

One of the striking differences is ordering and delivery.

Standard queues provide best-effort ordering, which ensures that messages are delivered in the same order in which they are sent. Occasionally, more than one copy of a message might be delivered out of order.

FIFO queues offer First-In-First-Out delivery, and only one copy of your message is delivered.



| Tables         | Standard Queue                                               | FIFO                                                         |
| :------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Throughput     | Nearly unlimited number of transactions per API action       | Up to 300 messages per second (300 send, receive, or delete operations per second). |
| Delivery       | Messages may be delivered once and occasionally more than 1 copy of a message is delivered | A message is delivered once and remains available until  it's processed and deleted |
| Delivery Order | Occassionally, messages might be delivered out of order      | Messages are delivered based on the order they were sent     |

### What Queue Type Should I Use

You should use standard queues as long your application can process messages that arrive out of order and once. For example, you want to resize images after upload.

On the other hand, you should use FIFO queues if your application can not tolerate duplicates and out-of-order delivery. 

For example, you want to prevent customers from being debited twice after an order is placed or preventing students from enrolling in a course before the registration task is processed.

### Creating a Queue

To create a queue on AWS, you'll need an AWS account. If you already have an AWS account, follow the steps below to create a standard queue.

- Sign in to AWS using your account credentials
- Navigate to the SQS console.
- Click Get Started with Amazon SQS for free
- Click "Get Started Now" and select the standard queue option
- Type in "MyExample"  as the queue name
- Click on "Quick-Create Queue" and note down the queue URL, you'll need it later.

### Configuring AWS SDK

Interacting with AWS services like SQS often requires you to install an [AWS SDK](https://aws.amazon.com/sdk-for-node-js/).   You can install the AWS SDK from the npm repository.

```
npm i aws-sdk
```

For demo purposes, we'll create a file `sqs.js` with the code below.

```js
//sqs.js
const AWS = require("aws-sdk");

const credentials = new AWS.SharedIniFileCredentials({profile: "default"});
AWS.config.credentials = credentials;

const sqs = new AWS.SQS({ apiVersion: "2012-11-05",region:'eu-central-1'});

module.exports = sqs;

```

The code above should be failry simple, we specified an AWS profile the SDK  should use to connect to AWS resources on our behalf.  

 Note: This is one way of granting your app access to AWS resources.  AWS has pretty [good documentation](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-credentials.html) on how you can configure your app to access AWS resources.  I encourage you to check it out.

### Message Producer

A message producer sends messages to a message queue.  For example, if we want to add a message payload that contains an `order_id` and the `date` the order was placed, we could write it like this:

```js
//producer.js

const sqs = require("./sqs");
const QueueUrl = process.env.QUEUE_URL;

// Create a message payload
const message = JSON.stringify({
  order_id: 1234,
  date: new Date().toISOString()
});

// message body
const params = {
  MessageBody: message,
  QueueUrl: QueueUrl
};

//send message to the queue
sqs.sendMessage(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.MessageId);
  }
});

```

### Message Consumer

It's one thing for a message to be added to a queue; it's another  for the message to be consumed. 

You can see a message consumer as a component that reads and processes your messages in the queue.

Creating a message consumer in node.js is relatively easy with the help of  [SQS-Consumer](https://www.npmjs.com/package/sqs-consumer).

`sqs-consumer` abstracts the complexity of consuming messages in AWS SQS and It comes with many configuration options to make your life easier. 

You can install `sqs-consumer` by running `npm i sqs-consumer` from your terminal.

Let's create a file `consumer.js` with the code below:

```js
//consumer.js

const { Consumer } = require('sqs-consumer');
const sqs = require('./sqs')
const queueUrl = process.env.QUEUE_URL

// Create our consumer
const app = Consumer.create({
    queueUrl: queueUrl,
    
    // Message handler function
    handleMessageBatch: async (message) => {
        console.log('Received a message')
        console.log(message)
        sqs.receiveMessage(message);
    },
    // Batch size of messages to process
    batchSize:10,
    
    sqs: sqs
});

app.on('error', (err) => {
    console.error(err.message);
});

app.on('processing_error', (err) => {
    console.error(err.message);
});

console.log('Consumer service is running');
app.start()
```

Please note that:

* `sqs-consumer` polls your queue continously using long polling.
* Your message will be deleted in the queue once the handler function has completed successfully.
* By default, messages are processed at a time.  You can process messages in parallel by using the batch option.



###  SQS Pricing

Before we look at how to optimize costs in SQS, let’s talk about how AWS charges for  Simple Queue Service (SQS). 

Your first 1 million requests per month are on AWS. Yeah, you get that for free. 

What is a request?

A request is an API call made to AWS SQS. 

This could be add, read, or delete a message from the queue. 

Processing a message in a queue usually goes like this:

- An API call to add a message to the queue
- An API call to read the message  from the queue
- An API call to delete the message from the queue after it has been processed.

This means that to handle a message successfully, you’ll make 3 API calls.

At the time of writing this post, a standard queue on AWS SQS costs $0.0000004 per request  which is equivalent to $0.40 per 1 million requests a month  and 0.0000005 per request (that is, $0.50 per 1 million requests a month) for the FIFO queue.



### Optimizing Costs 

**Batch your actions** 

Batching allows you to optimize each API call you make to AWS SQS.  It allows you to do more in each API call. You could batch multiple messages and add them to the queue in one request. 

For each API action, you could have up to 256kb worth of payload size.  For example, if you want to add three messages of size 64KB each to the queue, you can add them all in one API request as long as the overall size does not exceed 256kb. 

**Enable Long polling**

There are two ways to read a message from an SQS: long polling and short polling.

If you use short polling option, your application receives a response immediately, even if the response is empty.

Since you’re also charged when you read messages from the queue, short polling could be expensive with time.  It does not matter if the response is empty or not, you'll still be charged.

On the other hand, long polling allows you to consume a message from the queue when they become available within the maximum wait time of 20 seconds.
To lower your cost, you should enable long polling



### Conclusion

We've seen how to write a simple code that adds messages to SQS and also reads messages from the SQS. We also explored how AWS charges for SQS and talked about ways you could lower your costs.

We've covered a lot of grounds in this post; to build on this knowledge, I encourage you to check [AWS's official documentation](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) on the usage of AWS SQS. 