---
layout: post
title: 'Observer Pattern: Object Oriented PHP'
author: 'Samuel'
categories: PHP
comments: true
date: '2018-01-11 21:38:52'
tags:
- observer-pattern
- php
- oop
---

![Observer Pattern](https://res.cloudinary.com/samueljames/image/upload/v1524270397/observer-pattern_beqcbs.jpg)
Design patterns itself, are repeatable solutions to commonly occurring problems in software design, one of which is observer pattern and usually applicable to an abstraction with two aspects such that a change in one object requires a change in one or multiple objects.

If you are a frequent framework user like me, chances are high that you are already applying observer pattern in your code, be it observing a model or listening to certain actions.

Modern PHP frameworks like Laravel make use of several design patterns including observer pattern. It’s important that one knows the principles and why these patterns are used to be able to apply them effectively when faced with similar problems that can best be solved with design patterns.

For instance, when Facebook asks you that famous question: “what’s on your mind?” and you scribble some sentences down. You are notified automatically when a friend reacts to your post- That is observer pattern in action.

“Observer pattern defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated
automatically”
— Gang of Four

## Implementation
The Standard PHP Library provides interfaces called SplObserver and SplSubject as a standard template for implementing observer pattern in PHP.

### SplSubject Interface

 
```php
interface SplSubject {

/**
* Attach an SplObserve
* @param SplObserver $observer
* @return void
*/
public function attach (SplObserver $observer);

/**
* Detach an observer
* @param SplObserver $observe
* @return void
*/
public function detach (SplObserver $observer);

/**
* Notify an observer
* @return void
*/
public function notify ();

}
``` 

### SplObserver Interface

```php
interface SplObserver {

/**
* Receive update from subject
* @param SplSubject $subject
* @return void
*/
public function update (SplSubject $subject);

}
 ```

There are four methods that can be implemented in the interfaces defined above and they are the primary make up of observer pattern.

Function ```attach()``` provides a way we can register or associate an observer (subscriber) with a subject (publisher) and ```detach()``` is used to detach a disinterested observer from a publisher. Method ```notify()``` notifies all subscribed observers of state change which then calls ```update()``` on each observer.

### Observable/Subject/Publisher Implemenation

```php
use SplSubject;
use SplObserver;

class Publisher implements SplSubject
{
    /**
     * @var array
     */
    protected $linkedList = array();

    /**
     * @var array
     */
    protected $observers = array();


    /**
     *
     * @param string
     */

    protected $name;

    /**
     *
     * @param string
     */

    protected $event;


    public function __construct($name)
    {

        $this->name = $name;
    }

    /**
     *  Associate an observer
     *
     * @param SplObserver $observer
     * @return Publisher;
     */

    public function attach(SplObserver $observer)
    {
        $observerKey = spl_object_hash($observer);
        $this->observers[$observerKey] = $observer;
        $this->linkedList[$observerKey] = $observer->getPriority();
        arsort($this->linkedList);
    }

    /**
     * @param SplObserver $observer
     * @return void
     */
    public function detach(SplObserver $observer)
    {
        $observerKey = spl_object_hash($observer);
        unset($this->observers[$observerKey]);
        unset($this->linkedList[$observerKey]);
    }

    /**
     * @return void
     */
    public function notify()
    {
        foreach ($this->linkedList as $key => $value) {
            $this->observers[$key]->update($this);
        }
    }

    /**
     * Set or update event 
     *
     * @param $event
     * @return void
     */
    public function setEvent($event)
    {
        $this->event = $event;
        $this->notify();
    }

    /**
     * @return string
     */
    public function getEvent()
    {
        return $this->event;
    }

    public function getSubscribers()
    {
        return $this->getSubscribers();
    }

}
 ```

First, we define ```attach()``` method that accepts an observer instance as an argument. Just like the name implies, it associates an observer with the subject. We implement ```detach()``` method which is used to detach an observer from the subject.

The ```notify()``` function loops through the linked list, obtains subscriber’s key, use the key to find respective observer and then calls ```update()``` on the observer.
Note that the linked list is sorted to ensure that an observer with highest priority is notified first.

Function ```setEvent()``` accepts an argument of an event we want to broadcast and then calls ```notify()``` which in turn calls ```update()``` in each observer.


### Observer/Subscriber Implementation

```php
use SplObserver;
use SplSubject;

class Observer implements SplObserver
{
    /**
     * @var  string
     */
    protected $name;

    /**
     * @var int
     */
    protected $priority = 0;


    /**
     * Accepts observer name and priority, default to zero
     */
    public function __construct($name, $priority=0)
    {
        $this->name =$name;
        $this->priority =$priority;
    }

    /**
     * Receive update from subject and print result
     *
     * @param SplSubject $publisher
     * @return void
     */
    public function update(SplSubject $publisher){

        print_r($this->name.': '. $publisher->getEvent(). PHP_EOL);

    }

    /**
     * Get observer priority
     * 
     * @return int
     */
    public  function getPriority(){
        return $this->priority;
    }

}
 ```

Our observer instance accepts 2 arguments: observer name and priority. We then implement function ```update()``` which accepts **SplSubject** as argument, calls ```getEvent()``` and output the result to console.

### Usage
Let’s assume we are broadcasting an event to a team on Slack, some of whom are on a vacation and can only be reached via mail. In addition, we also want a live feed of this event on a dashboard screen located in the office.

This is how we can achieve that using observer pattern:
```php
$noty = new Publisher('NotificationPublisher');

$email = new Observer('EmailObserver', 50);
$slack = new Observer('SlackObserver', 10);
$dashboard = new Observer('DashboardObserver',30);

// Attach observers
$noty->attach($email);
$noty->attach($slack);
$noty->attach($dashboard);
// Broadcast event
$noty->setEvent("Server LNX109 is going down");
 ```


**Output**:

```
EmailObserver: Server LNX109 is going down
DashboardObserver: Server LNX109 is going down
SlackObserver: Server LNX109 is going down
```
In a real application scenario, you might want to make an abstract class of both Publisher and Observer that can be extended.

Observer pattern favors decoupling of a relationship between publisher (observable) and observers but debugging can be a bottleneck because flow of control is implicitly between observers and publisher.