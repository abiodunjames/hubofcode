---
title: Upload objects to s3 using pre-signed URL with customer-provided keys (SSE-C)
date: 2018-09-18 21:41:00 Z
categories:
- PHP
tags:
- aws
- s3
layout: post
author: Samuel
comments: true
---

With S3 pre-signed URLs, you can create short-lived URLs to upload objects to AWS S3.  More often than not business requirements demand that objects are encrypted and sometimes with keys provided by the customer. 

SSE-C option allows you to specify your own encryption keys, AWS automatically handles encryption and decryption of objects with these keys. 

Before I proceed any further, it's important you keep the following in mind when using SSE-C for uploads on AWS S3.

1. The request must be made through https
2. Encryption keys must be specified in the request header
3. Encryption keys must be stored securely. If you lose the key, you will not be able to retrieve the object.

**Install aws-php-sdk using composer**
 
 ```php
 composer require aws-php-sdk
 ```
 
**Generate pre-singned upload url**
 ```php
 
$key = bin2hex(random_bytes(32));  // Note, if you lose this key, you lose access to all objects encrypted by it

//Create customer key
$customerKey = hash('sha256',$key,true);

// Create customer MD5 Key
$customerMd5Key =md5($customerKey, true);

// Instantiate an Amazon S3 client.
$client = new S3Client([
            'version' => 'latest',
            'region' => 'your region',
            'credentials'=> [
            'key' =>'***********',
            'secret' =>'**********'
            ]
]);

$command =$client->getCommand('PutObject',[
'Bucket' =>'bucketname',
'Key'=>'testfile.json',
'SSECustomerAlgorithm' =>'AES256',
'SSECustomerKey' =>$customerKey,
'SSECustomerKeyMD5' =>$customerMd5Key
]);

// Create a pre-signed request that expires in 10 minutes;

$signedRequest = $client->createPresignedRequest($command,'+10 minutes');

$signedUploadUrl = $signedRequest->getUri();

$encodedCustomerKey = base64_encode($customerKey);

$encodedCustomerMd5Key = base64_encode($customerMd5Key);

var_dump($signedUploadUrl);  

var_dump($encodedCustomerKey); /// DIxGD8n7eLwfAusYvqpLK+Ysql+xYL4jdbR13Z3e08=

var_dump($encodedCustomerMd5Key); // Hz1c31UCZ/f5CwxxKnMeAQ==
 
 ```
 
**Upload an object using the url**

You can upload your object by making a `PUT`  request to the pre-signed URL including `x-amz-server-side-encryption-customer-key`and `x-amz-server-side-encryption-customer-key-MD5` in your header.

![Postman](http://res.cloudinary.com/samueljames/image/upload/v1537008795/SSE-C.png)
```
curl -v -H "x-amz-server-side-encryption-customer-key”:"DIxGD8n7eLwfAusYvqpLK+Ysql+xYL4jdbR13Z3e08=", - H "x-amz-server-side-encryption-customer-key-MD5": "Hz1c31UCZ/f5CwxxKnMeAQ==", -F "file=./test.json"  https://xxxxxxxxxx.s3.eu-central-1.amazonaws.com/testfile.json?x-amz-server-side-encryption-customer-algorithm=AES256&x-amz-server-side-encryption-customer-xxxxxxxxxxxx
```