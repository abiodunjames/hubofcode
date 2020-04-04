---
layout: post
title: Iterating large data in doctrine 2
date: '2019-04-19 10:26:35'
author: 'Samuel'
categories: PHP
comments: true
tags:
- php
- doctrine orm
- mysql
---


A few days ago, I wanted to update millions of records in a  MySQL database using Doctrine 2. I did a couple of searches and landed on  this doctrine [documentation](<https://www.doctrine-project.org/projects/doctrine-orm/en/2.6/reference/batch-processing.html>).  It suggests combining  query iteration with batching strategy to achieve this. 

For instance: 
<br>
```php
$batchSize = 1000;
$numberOfProcessedUpdate = 0;

$query = $entityManager->createQuery('select u from App\Model\User u');

$iterableResult = $query->iterate();

foreach ($iterableResult as $row) {
    $user = $row[0];
    $user->updateSomethingImportant();
    if (($numberOfProcessedUpdate % $batchSize) === 0) {
        $entityManager->flush(); // Executes all updates.
        $entityManager->clear(); // Detaches all objects from Doctrine!
    }
    $numberOfProcessedUpdate++;
}

$entityManager->flush();
```
<br>
This did not work well for my use case as it seems to be ineffective for records above a million.

| Records |     Time (s)      |                   Memory (mb) |
| ------- | :---------------: | ----------------------------: |
| 500k    | 185.1500730514526 |             6.255645751953125 |
| 250k    | 69.39160299301147 |             4.524032592773438 |
| 1 M     |                   | Allowed Memory size exhausted |

![](https://res.cloudinary.com/samueljames/image/upload/v1555663488/Screenshot_2019-04-17_at_09.27.47.jpg)

<br>

I made a little modification to the code snippet. I combined iteration,  batching and pagination strategy. 
<br>

```php
$batchSize = 1000;
$numberOfRecordsPerPage = 5000;

$totalRecords = $queryBuilder->select('count(u.id)')
            ->from('App\Model\User u')
            ->getQuery()
            ->getSingleScalarResult();   //Get total records to update

$totalRecordsProcessed = 0;

        while (true) {
            $query = $entityManager->createQuery('select u from App\Model\User u')
                ->setMaxResults($numberOfRecordsPerPage) //Maximum records to fetch at a time
                ->setFirstResult($totalRecordsProcessed);
          
             $iterableResult = $query->iterate();
            if ($iterableResult->next() === false) {
                break;
            }
                      
            foreach ($iterableResult as $row) {
                $user = $row[0];
                $user->updateSomethingImportant();
              
                 if (($totalProcessed % $batchSize ) === 0) {
                    $entityManager->flush();
                    $entityManager->clear();
                }
                $totalProcessed++;
            }
        }

	$entityManager->flush();
```
<br>

| Number of records to update |     Time (s)      |       Memory (mb) |
| --------------------------- | :---------------: | ----------------: |
| 2M                          | 9.931747436523438 | 1042.758535861969 |
| 500k                        | 7.36187744140625  | 200.1340479850769 |