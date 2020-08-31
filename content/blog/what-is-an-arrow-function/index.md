---
title: What is an arrow function?
description: Explanation on what an arrow function is
date: "2020-08-31T23:46:37.121Z"
---

An arrow function is a new (or not so new) feature in JavaScript ES6, along with many other interesting features. Not to be confused with `Java`'s arrow operator, which looks like this `->`, JavaScript's arrow function is also known as `fat arrow`.

```javascript
// ES5 (pre arrow function)
const filteredTotal = total.filter(function(d) { return d > 10; });

// ES6 (arrow function)
const filteredTotal = total.filter(d => d > 10);
```

Clearly, the latter example looks and feels cleaner, and definitely more concise. The arrow function is actually nothing more than syntatic sugar. Rarely do you see people choosing normal functions over arrow functions these days, unless they are stuck with legacy ES5 code base or browser.

ALthough the 2 examples produce the same result, there is a main difference between normal function and arrow functions that needs to be understood properly.

1. Arrow functions do not have their own `this` value. The value of this inside an arrow function is inherited from the enclosing lexical scope (the immediate outer scope).

2. They do not get their own `arguments` object, `super`, `new.target`.

**Point 2** is pretty self explanatory so I will pass over it.

Let's take a look at the implications of **Point 1**.

```javascript
const obj = {
  a: 10,
  b: 20,
  getA: () => return this.a,
  getB: () => return this.b
};

// both returns True
obj.a === 10;
obj.b === 20;

// both returns False. Why?
obj.getA() === 10;
obj.getB() === 20;
```

The last 2 examples returns `False` because `this` value in `getA()` and `getB()` refers to the `window` object because arrow functions do not have their own `this` value. Therefore, `this.getA()` and `this.getB()` both return undefined.

Maybe you thought of a clever way to work around this rule - You decide to rebind the `this` value. Yes, that's right! That should work, you figured.

Unfortuntately, JavaScript always find a way to frustrate you. You cannot rebind the `this` value for `getA()` and `getB()` even if you wanted to. Well, you technially can rebind it. It will just be ignored.

Why is the arrow function specified and implemented this way? I must admit that I am not aware of the discussions surrounding the specifications, but here's a [start](https://esdiscuss.org/topic/a-few-arrow-function-specification-issues) if anyone wishes to explore further.

```javascript
// the following is allowed but the this value will not be updated
// definitely good to have a warning in the future to let users know that
// rebinding this is ignored

const getAFunc = obj.getA;

// .call invokes the func immediately
getAFunc.call(obj);
// .bind does not invoke the func
getAFunc.bind(obj)
getAFunc();
```

How to solve this issue then? Use normal functions. For every other scenario, you may use arrow functions.

For more information, you can refer directly to the [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).



