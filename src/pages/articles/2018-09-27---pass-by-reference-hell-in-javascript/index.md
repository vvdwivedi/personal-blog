---
title: Pass by reference hell in JavaScript
date: "2018-09-27T23:46:37.121Z"
layout: post
draft: false
path: "/posts/pass-by-reference-hell-in-js/"
category: "Coder"
tags:
  - "Javascript"
  - "Frontend"
  - "Pitfall"
description: "If you are active with frontend technologies, you might have come across the term 'immutability'. And although this post is not directly going to talk about immutability, I will talk about a common trap that may developers run into."
---

If you are active with frontend technologies, you might have come across the term 'immutability'. And although this post
 is not directly going to talk about immutability, I will talk about a common trap that may developers run into. Before that,
 let's establish two terminologies:

 1. Pass by Value: The parameters you pass to a function are actual values.
 2. Pass by Reference: You call a method and pass a reference or address of the variable as parameter.


Let's also state the fact that

> **Javascript always pass by value** so changing the value of the variable never changes the underlying primitive.

If you missed a key word in the above statement, it's `primitive`. A string or number is a primitive. So when you do something like this
where you modify the values of params passed, it doesn't actually change the original variables;

```javascript
/**
 * pass by value for primitive
 */
function callMe(name, age) {
  name = 'vivek';
  age = 26;
  console.log("the name is: " + name + " and age is: " + age);
}

let name = 'vaibhaw';
let age = 20;

console.log("Initial values are name: " + name + " and age: " + age );

callMe(name, age)
console.log("After function call name: " + name + " and age: " + age);

```

So far, this seems like a natural way of how things should behave. I have kept the name of params same as the
outside variables so that it becomes quite clear that we are not modifying the outside variables even with same name.
But once you pass an object as the parameter, it's no longer a primitive. Consider a situation like this:

```javascript
/**
 * pass by reference for object
 */
function callMeMaybe(anyPerson) {
  anyPerson.name = 'vivek';
  anyPerson.age = 26;
  console.log("call him? call him not? the name is: " + anyPerson.name + " and age is: " + anyPerson.age);
}

let person = {
    name: 'vaibhaw',
    age:20
};

console.log("Initial values are name: " + person.name + " and age: " + person.age );

callMeMaybe(person)
console.log("After calling function name: " + person.name + " and age: " + person.age);
```

Notice that the parameter is named as anyPerson so that even by mistake or some quirk, the outside variable shouldn't update.
And this is a very common pattern where you pass the object to a function, modify it inside the function, maybe to make an API call or
for any other reason and you would expect the original variable to remain untouched. Not realizing that original variable is getting
changed can lead to a lot of errors in code where you wouldn't get JS errors, but the logic would start to fail.

Remember immutability? This concept says that you are not going to mutate certain variables, for example state variables in React. This
provides a very important safety layer in situation where your UI is driven by data in those objects.

**What can I do to prevent this?**

I am glad you asked. You will get this issue with both `Objects` and `Arrays` as both are of type object. You can use any of the following ways
to create a new variable and use that.

```javascript

arr.slice() // gives you a new array with same values
Object.assign({}, obj) // creates a new object

```

These are not just the only ways to save yourself from this hell. But I will let you explore other methods. In the meanwhile, if you want
to play with the code in this post, here is a repl.
<p>
<iframe height="400px" width="100%" src="https://repl.it/@vvdwivedi/reference-hell?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
</p>
