---
title: Set and Map in JavaScript
date: 2020-01-10 22:57:00 Z
published: false
categories:
- Node.js
tags:
- JavaScript
- Set & Map
author: Samuel
comments: true
layout: post
---

`Map` and `Set` are some of my favorites data structure in Javascript.  In this post, we'll explore what they are and how to use them in your daily life.

## Set

A `Set` is a collection of values with no keys. Each value can only occur once.  I like to use `Set` when I want to keep a collection of non-duplicated values.

 Set has many inbuilt methods and properties, such as:

- `add(value)` - adds a value to the collection.
- `delete(value)` - deletes a value from the collection.
- `has(value)` - checks if a value exist in the collection. 
- `clear()`  deletes everything in the collection. 
- `size`  returns the size of the collection.

**Working with Set**

```js
let set = new Set();
    set.add(4);
    set.add(5);
```

`add()` returns an instance of `Set`.  This means, you can chain `add()`.

```js
set.add(6)
  .add(7)
  .add(1);
```

Check if a value exists in a `Set`:

```js
set.has(10); //false
set.has(5); //true
```

Delete a value in a set:

```js
set.delete(7); // Set {4,5,6,1}
```

Get the size of the collection:

```js
set.size;  //4
```

You can also initialise a `Set` by  passing an iterable in to the constructor.

```js
let names = ["Paul", "Abiodun", "Ade", "Rita"];
let set = new Set(names);
```

Since `Set` is iterable, you can iterate on `Set` as follows:

```js
for (let value of set){
    console.log(value)
}

// You can also use forEach:
set.forEach((value) => {
  console.log(value);
});
```

## Map

A `Map` is a collection of key-value pairs.  `Map` is similar to `Object` in the sense that both allow you to assign values to keys, retrieve values using keys, and delete keys.

Before `Map` came to be, `Object` was an alternative for `Map` in JavaScript.

 However, there are differences between `Objec`t and `Map`.  
Some of the differences are:

1. Keys in `Map` can be anything (including objects, functions..., you name it) while keys in object can only be strings or symbols.
2. `Map` has a `size` attribute that allows you to know the size of the a `Map`. In `Object,` you would have to compute the size yourself. 
3. `Map` is iterable, and the keys are ordered. The order of the insertion is maintained.

`Map` has the following properties and methods:

- `set(key, value)` - sets a value by the key.
- `delete(key)` - deletes a value from the collection by the key.
- `has(key)` - checks if a key exist in the collection .
- `clear()` - deletes everything in the collection.
- `size`  - returns the size of the collection.
- `values()`  - returns an iterator that contains values for each element in the collection.
- `entries()`-  returns an iterator that contains an array `[key, value]` for each element in the Map.
- `get(key)` -  returns a value associated with `key`. 



**Working with Map**

You can initialize a `Map` without supplying an iterable in the constructor as follows:

```js
let myMap = new Map();
```

Unlike `Object`, anything can be a key in `Map`.

```js
myMap.set(1, "one");
myMap.set("name", "Samuel");
myMap.set({}, "Object");

let key = () => console.log("hello world");
myMap.set(key, "function as key");

```

You can also initialise a`Map`  by passing an iterable in to it's constructor.

```js
let fruits = [
    ['a', 'Apple'],
    ['b', 'Banana' ],
    ['c', 'Carrot'],
    ['d', 'Dragon fruits']
];

let myMap = new Map(fruits);
```

Checking if a key exists in  `Map`:

```js
map.has('a'); //true
map.has('z'); //false
```

Deleting a value by its key:

```js
map.delete('a'); 
```

Getting the size of `Map`:

```js
map.size;  //3
```

Getting a value by key in ` Map`:

```js
map.get('c'); //Carrot
```

You can iterate on `Map` as follows:

```js

let myMap = new Map(fruits);

for (let [key, value] of myMap){
    console.log(`${key} -> ${value}`)
}

// You can also use forEach:
myMap.forEach((value,key) => {
    console.log(`${key} -> ${value}`)
});
```



