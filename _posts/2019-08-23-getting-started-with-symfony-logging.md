---
layout: post
title: "Getting Started Quickly With Symfony Logging"
date: '2019-08-23 10:26:35'
categories: PHP
author: 'Samuel'
comments: true
tags:
- Symfony
- Logging
- PHP
- Monolog
- Log rotation
---

> Editorial note: I originally wrote this post on Scalyr's blog. You can check out the [original here](https://www.scalyr.com/blog/getting-started-quickly-symfony-logging/), at their site.


Logging is an important aspect of software development. In fact, in my opinion, it's just as important as testing. Why?

Logs become the only source of truth when things go wrong.

That's why in this post, you'll learn more about logging, why you'd do it, and how to log using the Symfony framework.

We’ll maintain the same basic structure that was used in [previous posts](https://www.google.com/search?rlz=1C1CHBF_enUS774US774&ei=ZpJDXe6VA4TcsAWGsaGYAg&q=site%3Ascalyr.com%2Fblog+getting+started+quickly&oq=site%3Ascalyr.com%2Fblog+getting+started+quickly&gs_l=psy-ab.3...13293.14274..15049...0.0..0.185.523.4j1......0....1..gws-wiz.INZAYqlsXK0&ved=0ahUKEwju4PaEhePjAhUELqwKHYZYCCMQ4dUDCAo&uact=5). That includes the following:

- The simplest way of logging in Symfony
- What to log and where to log it
- How to use the monolog logger in Symfony

## What Is Symfony?

Symfony is an open-source framework that thousands of developers have contributed to. [Fabien Potencier](https://www.linkedin.com/in/fabienpotencier/) originally created it, and [SensioLabs](https://sensiolabs.com/) maintains it. It has a huge community, with more than 600,000 developers from 120 countries.

Symfony is one of the most popular PHP frameworks for building large applications. It's built on many decoupled, reusable PHP libraries called[ components](https://symfony.com/components). Symfony components are widely used in most PHP applications today. It’s the core component behind [Magento](https://magento.com/), [PrestaShop](https://www.prestashop.com/en), [Drupal](https://www.drupal.org/), and others. [Laravel](https://laravel.com/), probably the most popular PHP framework today, uses many Symfony components.

## How Can You Install It?

One of the easiest ways to install the Symfony framework is through the Symfony client. So before you go further, make sure you have [PHP](https://www.php.net/)installed, whether it's on macOS ([here's how](https://tecadmin.net/install-php-macos/)), Ubuntu ([here's how](https://linuxize.com/post/how-to-install-php-on-ubuntu-18-04/)), or Windows ([here's how](https://www.sitepoint.com/how-to-install-php-on-windows/)).

You'll also want to have [Composer](https://getcomposer.org/doc/)installed. Composer is a package manager for PHP. You can install it by following their [official documentation](https://getcomposer.org/doc/00-intro.md).

 

From the command line, run the code below to install Symfony client.

```
curl -sS https://get.symfony.com/cli/installer | bash
```

If you run the command above successfully, your screen will look like this:

![Symfony client installation console output](https://res.cloudinary.com/samueljames/image/upload/v1566639366/installing-symfony-client-1024x440.png)

To complete the installation, you'll need to add Symfony's path to your shell.

To do that, open your terminal and run:

```
  export PATH="$HOME/.symfony/bin:$PATH"
```

Let's verify the installation by executing:

```
$ symfony -v
```

If properly installed, the output of your screen will be similar to the image below.

![Verifying symfony installation](https://res.cloudinary.com/samueljames/image/upload/v1566639388/symfony-setup-1024x330.png)

## A Simple Console App With Symfony

For the demo, we'll build a simple console application using the Symfony framework. Jorge Luis Borges said, "Life itself is a quotation." So, in this application let's display random quotes every five seconds.

You can keep this application simple or extend it further. For example, you can display it on your TV screen. (Your guests will admire you, pulling you aside to say, "This is amazing! Are you some sort of genius?")

Now that you have the Symfony client installed, you can start a brand-new app with the command below:

```
symfony new random-quote
```

The command above creates a project in the directory "random-quote"  with the project dependencies installed. It should less than two minutes, provided you have good internet speed.

[Guzzle](http://docs.guzzlephp.org/en/stable/)is a package in PHP that allows you to make HTTP requests to web services. Our goal is for demo app will get random quotes from an external web service using Guzzle. You can install it using Composer.

Let's create a new HTTP client and define the base URL where you'll obtain quotes.

Go to the root directory of the demo app and run:

```
 composer require guzzlehttp/guzzle
```

Create a new folder named **Command**in the **src** directory.

```
...
├── public
│   └── index.php
├── src
│   ├── Command
│   ├── Controller
│   └── Kernel.php
├── symfony.lock
...
```

Create a new class called **Quote**with this content:

```php
//src/Command/Quote.php
<?php declare(strict_types=1);

namespace App\Command;

use GuzzleHttp\Client;

final class Quote
{
    /**
     * @var Client
     */
    private $client;

    /**
     * Quote constructor.
     */
    public function __construct()
    {
        $this->client = new Client(['base_uri' => 'https://api.quotable.io', 'verify' => false]);
    }

    /**
     * @return string
     */
    public function getQuote(): string
    {
        $response = $this->client->get('random');
        $contents = json_decode($response->getBody()->getContents(), true);

        return $contents['content'];
    }
}
```

You don't want to verify the SSL certificate, so set **verify**to **false**.

To summarize, in the **getQuote** method above, the code performs the following tasks:

- It makes a **GET**request to obtain a random quote.
- Then it decodes the JSON response.
- Finally, it returns the content of the API response.

## Creating a Symfony Command

Symfony provides commands through **bin/console**. Commands are one of the ways you can interact with your application. Let's create one now.

In the **Command** directory, create a new class **QuoteCommand** with this content:

```php
//src/Command/QuoteCommand.php

<?php declare(strict_types=1);

namespace App\Command;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

final class QuoteCommand extends Command
{
    /**
     * @var Quote
     */
    private $qouteService;

    /**
     * @var string
     */
    protected static $defaultName = 'app:get-quote';

    public function __construct(Quote $quoteService)
    {
        $this->quoteService = $quoteService;
        parent::__construct(null);
    }

    /**
     * @return void
     */
    protected function configure()
    {
        $this->setDescription('This command gets a random quote from quotable.io.');
    }

    /**
     * @param InputInterface $input
     * @param OutputInterface $output
     * @return int|null|void
     */
    protected function execute(InputInterface $input, OutputInterface $output)
    {
        $output->writeln([
            'A doze of inspiration to power your day',
            '========================================',
            '',
        ]);
        $quoteSection = $output->section();

        while (true) {
            $quote = $this->quoteService->getQuote();
            $quoteSection->overwrite($quote);
            sleep(5);
        }

    }
}
```

In this code snippet, we injected the **Quote** class in **QuoteCommand** and wrapped it in a while loop so it runs continuously. A command in Symfony must implement two methods: **configure** and **execute.** 

- The function **configure** allows you to describe what the command does and some other parameters like name and help text.
- The function **execute** contains the logic that will be executed when the command is invoked.

The **execute** method pulls a new random quotation every five seconds using the class you injected.

Let's test it out! If you run the application with the command

```
php bin/console app:get-quote
```

and everything goes fine, your console should become a powerful inspirational tool.

The output should be similar to the image below:

![Console app output](https://res.cloudinary.com/samueljames/image/upload/v1566639413/Screenshot-2019-07-19-at-07.03.02-1024x201.png)

## The Simplest Possible Logging That Could Work in Symfony

Now, if remember what I said at the very beginning of the post, you'll know your **random-quote** app lacks an important feature: logging. Once the app leaves your development environment, it will be difficult to know when something's going wrong. For example, if the quote API stops responding to your requests, you'd never realize it.

Symfony comes with a minimal logger that implements [PSR-3](https://www.php-fig.org/psr/psr-3/). PSR is an acronym for "PHP standards recommendation." Its aim is to maintain compatibility between the different logger services that implement the PSR-3 logger interface.

To use this logger, you need to configure it in **config/services.yaml** file. So let's update **config/services.yaml** with:

```yaml
#file config/services.yaml
parameters:
services:
    # default configuration for services in *this* file
    _defaults:
        autowire: true      # Automatically injects dependencies in your services.
        autoconfigure: true # Automatically registers your services as commands, event subscribers, etc.
    logger:
        class: Symfony\Component\HttpKernel\Log\Logger
        arguments: ['info','var/log/dev.log']
    # makes classes in src/ available to be used as services
    # this creates a service per class whose id is the fully-qualified class name
    App\:
        resource: '../src/*'
        exclude: '../src/{DependencyInjection,Entity,Migrations,Tests,Kernel.php}'
    # controllers are imported separately to make sure services can be injected
    # as action arguments even if you don't extend any base controller class
    App\Controller\:
        resource: '../src/Controller'
        tags: ['controller.service_arguments']
```

Now you can use the logger service in your code.

Update **Quote** class with this content:

```php
<?php declare(strict_types=1);

namespace App\Command;

use GuzzleHttp\Client;
use Psr\Log\LoggerInterface;

final class Quote
{
    /**
     * @var Client
     */
    private $client;

    /**
     * @var LoggerInterface
     */
    private $logger;

    /**
     * Quote constructor.
     */
    public function __construct(LoggerInterface $logger)
    {
        $this->logger = $logger;
        $this->client = new Client(['base_uri' => 'https://api.quotable.io', 'verify' => false]);
    }

    /**
     * @return string
     */
    public function getQuote(): string
    {
        $this->logger->info('Getting a new quote');
        $response = $this->client->get('random');
        $contents = json_decode($response->getBody()->getContents(), true);

        $this->logger->info("Got a new quote: {$contents['content']}");

        return $contents['content'];
    }
}
```

Notice that something fundamental has changed about our code. Now the **Quote** class gets a parameter of type **LoggerInterface** through its constructor.

Symfony has a cool feature called[auto-wiring](https://symfony.com/doc/current/service_container/autowiring.html). Autowiring allows you to retrieve an instance of a class from a dependency injection container by type-hinting its interface in functions.

Using this injected dependency, we're able to use the **logger** class. Take a look at the **getQuote** method. You'll see we logged twice: immediately before and after retrieving a quotation.

If you restart the application, you'll see the **logger** class in action. If the previous instance of your app is still running in your console, the new changes won't be available to it yet. You'll need to kill it and then restart by running the code below in your console:

```
php bin/console app:get-quote
```

If you look at the file "**var/log/dev.log"** after restarting the app, you should see some logs already written. The log content should be similar to this:

```
2019-07-19T05:31:09+00:00 [info] Getting a new quote
2019-07-19T05:31:09+00:00 [info] Got a new quote: The aim of life is self-development. To realize one's nature perfectly—that is what each of us is here for.
```

## What Is Logging?

Let's zoom out from the code and think a little more theoretically. What does logging really mean, and [why should you do it](https://www.scalyr.com/blog/why-do-engineers-care-about-logging)?

Colin Eberhardt provides an answer in his post "[The Art of Logging](https://www.codeproject.com/Articles/42354/The-Art-of-Logging)."

> Logging is the process of recording application actions and state to a secondary interface.

So, what are the ramifications of recording this information? Well, logging is all about providing visibility into apps in development or in production. You can't get this visibility without recording certain actions and states.

Have you ever used a desktop or mobile app that crashed unexpectedly? Nine out 10 times this happens, you’ll see a prompt that asks you to click a button and send reports (logs) to the app’s developer. This is necessary because no developer has the sorcery to see what happens in production without logs. Logs become your glasses; they're the key to seeing what happened and when.

But as important as logging is, [if you don't do it right](https://www.scalyr.com/blog/the-10-commandments-of-logging/), it can lead you into trouble.

## How Not to Log

First off, you should know there are ways you shouldn't log. For instance, don't include sensitive data or personally identifying information in your log. User input often contains sensitive data.

Here's an example of an incorrectly done log:

```
[2019-07-11 20:29:59] User with username:johndoe@example.com and password:123345 logged in at 126374846489 with IP 127.0.0.1
```

There are so many things wrong with the example above. For one thing, the log contains sensitive credentials: username and password. This is not a good practice.

If you ever need to log user input that contains sensitive data, you should [mask the sensitive data](https://www.schibsted.pl/blog/logback-pattern-gdpr/).

## How to Log Meaningfully

Logs can grow extensive over time. It's tedious to sift through enormous numbers of log entries to find a specific message of interest. Even worse is if you find the message, but you don't have enough context to know what really happened.  A log that that lacks context provides no value to its users.

That's why you don't want to log like this:

```
[2019-07-11 20:29:59] User login failed
[2019-07-11 20:29:59] Registration was successful
```

When you log, ask yourself two questions:

1. What purpose would this log serve?
2. Does the log include enough context to be useful?

### Structure Your Log

Before you choose a structure for your log, it's important to understand who its users will be. Most likely, its readers will be humans and machines.

Since that's the case, your log structure should be in a format that's readable by humans and can be parsed by machines.

Most importantly, you'll want to stay consistent with your choice. For example, if you choose to log in JSON format, ensure you do so every time.

### Use Log Levels

Log levels help you put log messages into groups that you or other users can sort through. You can think of log levels as tags or labels you attach to messages to prioritize them.

There are several log levels, and each serves a different purpose. If you want to learn more, you're in the right place. The Scalyr blog has a detailed post on [what log levels are they are and how they help you](https://www.scalyr.com/blog/logging-levels/).

## Evolving Your Approach

In the example above, you built a small application and implemented the minimal amount of logging possible in Symfony. However, many details are still missing. For example:

1. Our application logs everything to file. This approach can grow large with time and eat up disk space. If you wanted to use an application like this regularly, you'd need to implement a [log rotation](https://en.wikipedia.org/wiki/Log_rotation) strategy.
2. In this application, we logged to a single file. But there could still be some form of grouping. For example, you could log emergency, critical, and debug logs in different files.
3. In production, you'd probably want to use a [managed log aggregation service](https://www.scalyr.com/blog/log-aggregation-help/). But in a development environment, logging to file on your local machine could be enough.
4. You could set up an alert mechanism for critical or security logs. Aside from logging, you'd want to receive an alert by email or pager so you could react quickly.

As you can see, you could make significant improvements to this logging system.

### Using a Logging Framework: Monolog Logger

[Monolog](https://github.com/Seldaek/monolog) is a modern and popular logger for PHP. It implements a PSR-3 logger interface for high interoperability. Symfony integrates seamlessly with Monolog.

To use Monolog, you'd need to install the **monolog-bundle** using Composer:

```
composer require symfony/monolog-bundle
```

After installation, you'll see three new files in the config directory: **config/packages/dev/monolog.yaml**, **config/packages/test/monolog.yaml**, and **config/packages/prod/monolog.yaml**.

Symfony maintains separate logging configurations for different environments. Usually, you wouldn't handle log messages in production the same way you would in a development or test environment.

### Log Channels

Monolog brings some level of organization to your logs in the form of channels. Channels allow you to group your log messages. Channels are configurable, so you can decide how you want to handle a certain log channel.

You could have channel names like **general**, **core**, **application**, or any name you want. Symfony comes with some default channel names, including **doctrine**, **event**, and **security**. To check the channels that come bundled with Symfony, run the command below:

```
php bin/console debug:container monolog
```

As a result, you'll see a list.

### Log Handlers

If you look at **config/packages/dev/monolog.yaml** file, you'll see a channel named **main**. It logs messages of minimum level **debug** to file. It uses stream handler, which is defined as type **stream**.

```yaml
monolog:
    handlers:
        main:
            type: stream
            path: "%kernel.logs_dir%/%kernel.environment%.log"
            level: debug
            channels: ["!event"]
```

Handlers are a set of classes that either modify or write log messages to a destination: a text file, an external log aggregation service, a database, or an email. A logger instance could have *n* handlers defined. When you log a message, your message transverses the handler stack. In other words, your log keeps moving from one handler to another until it has been handled by all of the handlers in the logger instance. A handler could modify a log message, log, or do nothing with it, based on the level of the message and the level the handler is configured to handle.

Want to see an exhaustive list of possible handlers? Take a look at a complete [Monolog configuration file](https://github.com/symfony/monolog-bundle/blob/master/DependencyInjection/Configuration.php).

Previously, we configured our app to use the minimal loggers that came bundled with Symfony in **config/services.yaml.**

To swap Symfony's minimal logger to the Monolog logger, you would need to change the logger object in **config/service.yaml** on line 13 to:

```yaml
#file: config/services.yaml
logger:
    alias: 'monolog.logger'
```

Once you restart the app, it will use Monolog logger to log messages. This opens new possibilities.

You may want to notify the development team on Slack when a critical event happens. All you need to do is to configure a Slack handler as:

```yaml
#config/packages/dev/monolog.yaml
monolog:
    handlers:
        main:
            type: stream
            path: "%kernel.logs_dir%/%kernel.environment%.log"
            level: debug
            channels: ["!event"]
        slack:
            type: slackwebhook
            webhook_url: "https://hooks.slack.com/services/XXXX/XXXX/XXXXXX"
            channel: "#your-team-channel"
            level: critical
```

When you log a critical error message like this, your message posts to the Slack channel you configured.

```php
$this->logger->critical('Authentication service is could not be reached: aborting application', [
    'requestID' => 'XXXX-XXX-XX',
  	'IP' => 'XX.XX.XX.X'
  	'username'  => 'XXXX'
]);
```

### Log Rotation

Logs can grow huge over time. If you log to disk without a log rotation strategy in place, you could run of out disk space.

You can configure Monolog logger to log in to separate files every day. And it's not only that. You can also set how many of those files to keep at a time. This ensures that logs don't eat up too much of your disk.

To rotate your log, update **config/packages/dev/monolog.yaml** with this:

```yaml
#file: config/packages/dev/monolog.yaml
monolog:
    handlers:
        main:
            type: rotating_file
            path: "%kernel.logs_dir%/%kernel.environment%.log"
            level: debug
            max_files: 20
            channels: ["!event"]
```

## Conclusion

Today you've learned about Symfony, the simplest possible logging, and how to log the right way. You built a little console application and integrated the most popular logging framework in PHP.

You can learn more by playing with the console app. For example, you could:

1. Play with different log levels.
2. Configure more Monolog handlers.
3. Integrate with managed log aggregating services, such as [Scalyr](http://blog.scalyr.com/2017/08/log-aggregation-help/).
4. Create a custom log handler.

To expand your knowledge further, read about[the logging practices you should adopt](https://www.scalyr.com/blog/application-logging-practices-adopt/)and [how not to destroy your log files](https://www.scalyr.com/blog/common-ways-people-destroy-their-log-files/).

Want to look at the source code? You can find it on [GitHub](https://github.com/abiodunjames/Random-Quote).

Keep logging!