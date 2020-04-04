---
layout: post
title: A Pocket Guide to Destructuring Assignment in ES6
date: '2019-03-28 20:26:35'
author: 'Samuel'
categories: JavaScript
comments: true
tags:
- javascript
- es6
- destructuring
---

Today, we will be exploring destructuring assignment — one of the ES6 cool features. It provides a clean way of extracting items of an array or object or anything iterable.

#### #1 Destructuring basics

There are 2 sides to destructuring in ES6: the source and the template. The ‘source’ is the object or array to be "destructured" and it’s always on the right-hand side while the left-hand side is the template used in destructuring the ‘source’.



![img](https://cdn-images-1.medium.com/max/1600/1*Yip_EbziXMV8__CO_h_ahw.png)

In ES5, you could extract values from an array of `fruits` as follows:

```js
var fruits = ['apple', 'banana', 'carrot'];
var a = fruits[0];
var b = fruits[1];
var c = fruits[2];

console.log(a); //apple
console.log(b); //banana
console.log(c); //carrot
```

We can do the same as above in a more precise way in ES6:

```js

let [a, b, c] = ["apple", "banana", "carrot"];
console.log(a); //apple
console.log(b); //banana
console.log(c); //carrot
```

Destructuring works for arrays, objects and anything iterable. You can selectively extract the values you want from objects even strings.

Given an object`subscribers `👇 below, we can extract `email` and `name` from the object and probably send an email or do something cooler. 😁

```js
let subscribers = [
  {
    id: 1,
    name: "John Doe",
    email: "johndoe@example.com",
    age: 27
  },
  {
    id: 2,
    name: "Mark Lukas",
    email: "marklukas@example.com",
    age: 40
  }
];

subscribers.forEach(subscriber => {
  let { name, email } = subscriber;
  console.log(`Emailing ${name}<${email}>`);
  
  //Emailing John Doe<johndoe@example.com>
  //Emailing Mark Lukas<marklukas@example.com>
});

```

What about a deeply nested object? yeah! you can extract values from deeply nested objects. 👇

```js
let subscribers = [
  {
    company: "Google",
    employee: {
      name: "John Doe",
      email: "johndoe@example.com",
      age: 27
    }
  },
  {
    company: "Microsoft",
    employee: {
      name: "Mark Lukas",
      email: "marklukas@example.com",
      age: 40
    }
  }
];

subscribers.forEach(subscriber => {
  let { employee: { name, email }} = subscriber;
  
  console.log(`Emailing ${name}<${email}>`);
  //Emailing John Doe<johndoe@example.com>
  //Emailing Mark Lukas<marklukas@example.com>
});

```

You can destructure strings because they are iterable and anything iterable can be destructured! Remember?

```js
let fruits ='fruits';
 let[a,b,c,d,e,f] = fruits;

 console.log(a); //f
 console.log(b); //r
 console.log(c); //u
 console.log(d);//i
 console.log(e);//t
```

#### #2 Swapping variable values

Swapping variables in ES6 is simpler, there is no need for a third variable that serves as a value holder.

```js
let a = "apple";
let b = "banana";

[a, b] = [b, a];

console.log(a); //banana
console.log(b); //apple

```

#### #3 Skipping variable value

While destructuring, you can skip values you do not need with a comma.

```js
let fruits =["apple","banana","carrot","Dewberries","Eggfruits","Farkleberry"];

let [,b,c] = fruits; //skip "apple"
console.log(b); //banana
console.log(c); //carrot

let[a,,,d] = fruits; //skip "apple","banana" and "carrot"

console.log(d); //Dewberries
```

#### #4 Rest operator

Rest operator is represented by three dots (…) and allows iterables to be collapsed into a single element array. You can use rest operator to pull values you want and combine the rest into one array.

```js
let fruits =["apple","banana","carrot","Dewberries","Eggfruits","Farkleberry"];

let [a,b, ...others] = fruits;

console.log(a); //apple
console.log(b); //banana 
console.log(others); //[ 'carrot', 'Dewberries', 'Eggfruits', 'Farkleberry'
```

#### #5 Multiple return values

Unlike Go, Javascript does not support multiple return values natively. One can mimic multiple return values by taking advantage of destructuring assignment.

```js
function getFruits(){
    return ["apple","banana","carrot"];
}

let [a,b,c] = getFruits();

console.log(a); //apple
console.log(b); //banna
console.log(c);//carrot
```

#### #6 Default destructuring value

If the extracting pattern does not match any key in the "source", `undefined`will be returned.

```js
var fruits = { a: "apple", b: "banana" };
var {c} =fruits;

console.log(c); //undefined
```

Alternatively, you can specify a default value.👇

```js
//set default
var {c='carrot'} = fruits;
console.log(c); //carrot

let {a,b=1} ={};
console.log(a); //undefined
console.log(b); //1
```

You can specify a default value in arguments too.

```js
function food ({a, b:fallback='banana'}){
    console.log(`I ate ${a} for breakfast and ${fallback} for dinner`)
}

food({a:'apple',b:'carrot'}); //I ate apple for breakfast and carrot for dinner

food({a:'apple'}); //I ate apple for breakfast and banana for dinner
```

#### Conclusion

While destructuring assignment will save you a few lines of code and some typing efforts, it is important to note that not all browsers support it at the time of writing this post. If you need to support old browsers like IE11, you can with the help of compilers like [Babel](https://babeljs.io/). You can also check [here](http://kangax.github.io/compat-table/es6/) to see if your browser supports ES6. Happy destructuring. 🎉 🎉