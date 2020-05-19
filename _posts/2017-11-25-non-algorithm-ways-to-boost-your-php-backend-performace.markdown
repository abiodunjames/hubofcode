---
title: 3 ways to boost your PHP backend performance
date: 2017-11-25 09:12:59 Z
categories:
- PHP
tags:
- backend
- performance
- php
layout: post
author: Samuel
comments: true
---

Performance is something I have been intrested in for a while now. I have not only been concerned about writing code that works but also code that performs considerably at the face of large input or traffic.

Since performance is a function of all integral parts of a system, in this post, I emphasize on some few existing ways to improve your PHP backend performance.Whether you are refactoring a legacy application or starting a new project entirely, either way, you will find this post helpful.

### Make use of [Generators ](http://php.net/manual/en/language.generators.overview.php)

Generators are simple iteraror introduced in PHP 5.5.0  that are more memory efficient. The standard PHP iterator iterates on data set already loaded in memory, this is a very expensive operation for large inputs as they have to be loaded in memory.

Generators are more memory efficient for such operations due to its ability to compute and yield values on demand.

Most applications accept and process uploaded Excel/CSV from users, performance cost for a very small CSV file with few lines might be inconsequential but becomes very expensive for a large file as shown in the example below.  
 To show this, we will grab a 25MB csv file from this [GitHub repository](https://github.com/datasets/world-cities/blob/master/data/world-cities.csv) . It contains names of cities around the world.

```php
function processCSVUsingArray($csvfile)
{
    $array = [];
    $fp = fopen($csvfile, 'rb');
    while (feof($fp) === false) {
        $array[] = fgetcsv($fp);
    }
    fclose($fp);
    return $array;
}


//using generator

function processCSVUsingGenerator($csvfile)
{
 $fp = fopen($csvfile, 'rb');
 while (feof($fp) === false) {
 yield fgetcsv($fp);
 }
 fclose($fp);
}
```
Now, we compare perfomance of the two functions using this code:
```php
$time = microtime(TRUE);
$mem = memory_get_usage();

$file ="cities.csv";
foreach (processCSVUsingArray($file) as $row) {
    //do something with row;
}

print_r(array(
    'memory' => (memory_get_usage() - $mem) / (1024 * 1024),
    'seconds' => microtime(TRUE) - $time
));
```

Function ```processCSVUsingArray``` exited with a fatal error: ```Allowed memory size of 134217728 bytes exhausted (tried to allocate 4096 bytes)``` while ```processCSVUsingGenerator``` used **0.00014495849609375MB** of memory and   took **10.339377164841** seconds to complete

> With a generator, you can use ```foreach``` to iterate over a set of data without needing to build an array in memory, which may cause you to exceed a memory limit, or require a considerable amount of processing time to generate.

### Always Cache your data

Caching is an important aspect of optimization and you rarely talk about performance without it on the list. Is it really necessary to make a request to the database or external API each time a certain value is requested? Sometimes, the answer is capital NO and this is where caching should come to play.

For instance, a list of countries, states or province is likely not going to change over a certain period of time, such data should be in cache for faster retrieval.

Personally, I found out that, coming up with  a list, such as the one shown below that  shows how frequent  values are computed or updated in my application allows me to fully harness the power of cache.

![](http://res.cloudinary.com/samueljames/image/upload/v1524270401/cachine_kyiykv.png)

Depending on your application use case, your list should be different from the one in this example. For instance, if you are building an application where users can add, edit or delete countries, caching such data for a month is obviously  a bad idea.

### **Enable [OpCache](http://php.net/manual/en/book.opcache.php)**

[OpCache ](http://php.net/manual/en/book.opcache.php)has been around from PHP5.5. PHP is an interpreted language, scripts are parsed and compiled at runtime. Both compute time and resources are needed for this process, this results to some performance overheads as every request follows this loop.

OpCache was built to optimize this process by caching precompiled bytecode such that the interpreter does not have to read, parse or compile PHP scripts on every request.

By default, OpCache is disabled in apache.

To enable OpCache in apache, update`php.ini`  with this configuration.
```
opcache.enable=1
opcache.memory_consumption=128
opcache.max_accelerated_files=4000
opcache_revalidate_freq = 240 
```