---
layout: post
title: 'Laravel Developers: What you need to know  before you start your first Lumen
  project'
author: 'Samuel'
categories: PHP
comments: true
date: '2018-03-31 11:10:27'
tags:
- api
- laravel
- lumen
- php
---


Lumen is one of the fastest PHP frameworks built from Laravel components and made especially for building APIs or web services. It’s faster and can handle more request per second than Laravel itself.

This is because Lumen is stripped down to bare minimum thus sacrificing flexibility for speed. By default, some of the features you enjoy in Laravel are not enabled.

As your API grows, you find yourself constantly yearning for the flexibility Laravel provides that are not available in Lumen.  This post is aimed to bring these features to your attention, get you up and running, and hasten your transitioning.

#### **Enabling Facades in Lumen**

Facades in Laravel provide access to objects in the container providing a terse and expressive syntax. By default, facades are commented out in Lumen.

To use facades in lumen, open ```bootstrap/app.php```and uncomment  ```$app->withFacades() ``` as follows:

```php
//bootstrap/app.php
$app->withFacades();
```

#### **Registering a new package in Lumen**

Package registration in Lumen slightly different from that of Laravel. To register a new package, you will first need to install it with the composer.

Open ```bootstrap/app.php ``` and register the service provider as follows:
```php
//Register example package service provider 
$app->register('PackageAnotherNamespaceExampleExampleServiceProvider');
```

Then, facade as:
```php
if (!class_exists('Example')) { class_alias('PackageAnotherNamespaceFacadesExample', 'Example'); }
```
#### **Enabling Eloquent**

One of the most used features of Laravel is the Eloquent ORM. It provides a clean and expressive way to interact with your data. In Lumen, eloquent is disabled by default.

To enable eloquent, open ```bootstrap/app.php ``` and uncomment the line
```$app->withEloqunet()```

```php
// bootstrap/app.php
$app->withEloquent();
```
#### **Bringing config folder back to Lumen**

Strangely, when you first install Lumen, our little-beloved ‘config folder’ in Laravel appears to be nowhere.   This is because, in Lumen,  settings are defined in the .env file.  As your application grows, this file becomes excessive bulky, lengthy and of course difficult to maintain (My opinion).

To enjoy the flexibility of a config folder in Lumen just as you are  in Laravel

1. Create a folder with the name ‘config’ at the project root.
2. Define your configuration files as you would in Laravel. For example, let’s define our application settings as follows:
```php
//config/app.php 
return [ 'name' => 'Example API', 'email' => 'samuel@example.com', 'locale' => env('APP_LOCALE', 'en'), ];
```

3. Register the config file in  ```bootstrap/app.php```as follows:

```php
$app->configure('app'); //register config/app.php $app->configure('filesystem'); //register config/filesystem.php
```

#### Make lumen validation messages more expressive

When requests fail validation in Laravel, human-friendly messages are returned to guide the user.  In Lumen, error messages are returned like this.

![](http://res.cloudinary.com/samueljames/image/upload/v1524270390/lumen_jecvfq.png)To have more definitive error messages, you will need to create a ‘lang’ folder in *resources* folder as shown below.![](http://res.cloudinary.com/samueljames/image/upload/v1524270389/language-file_hxvrv5.png)Update  *validation.php *with this [content](https://github.com/laravel/laravel/blob/master/resources/lang/en/validation.php).   You should also ensure that app locale is set appropriately for translation service to use.

```php
//config/app.php
'locale' => env('APP_LOCALE', 'en'),
```
And there you go.

Happy Coding.
