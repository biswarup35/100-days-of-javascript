# Day 7 of 100 Days of Code - Function as a first-class citizens

Did you know in JavaScript, functions are first-class citizens?
Today, we will study what is a first-class function means and what we can do with it.

One of the most dynamic features of JavaScript is that it has first-class functions. A first-class object sometimes referred to as a first-class citizen, is an object that supports all of the operations generally allowed to other objects. Javascript functions are themself types of objects. Hence the functions can be stored in a variable, passed around, return from other functions, and even can hold their properties.

Therefore, a JavaScript developer can leverage powerful design patterns such as higher-order functions, partial function applications, callbacks, closures and more.

A first-class function can be expected to support the same operations we would expect from other objects.

So what are these operations?

- It can be stored in a variable.

- It can be passed as arguments to functions.

- It can be returned by functions.

- It can be stored in some data structures.

- It can hold its properties and methods.

Let's see them one by one:

```javascript
// Declartion
function speak(string) {
  return string;
}

console.log(speak("Good Moring")); // Logs "Good Moring"
```

Functions can be stored in a variable:

```javascript
const greeting = speak;

console.log(greeting("Good Afternoon")); // Logs "Good Afternoon"
```

Functions can be passed as the argument;

Can be returned by a function.

```javascript
function message(fn, message) {
  return fn(message);
}

const sayHello = message(greeting, "Hello");
const greetGoodMorining = message(greeting, "Good Moring");
console.log(sayHello); // Logs "Hello"
console.log(greetGoodMorining); // Logs "Good Morning"
```

Functions can be stored in data structure:

```javascript
const fn = [speak];
console.log(fn[0]("Wooo hooooo")); // Logs "Wooo hooooo"
```

Functions can hold their properties:

```javascript
speak.haveCar = "BMW Z5";
speak.running = () => console.log("runing...");

console.log(speak.haveCar); // Logs "BMW Z5"
speak.running(); // Logs "runing..."
```

## Use of First-class function

I already told first-class function let us leverage powerful design patterns. It helps to write more readable, dynamic and more concise code. Let's take a few examples:

### Higher-order functions

A higher-order function is a function that accepts functions as parameters and or returns a function. In other words, higher-order functions do work on other functions. Below `.map(double)` is an example of higher-order function: accepts `double()` function as an argument.

```javascript
function double(num) {
  return num * 2;
}

const nums = [3, 6, 9];
const doubledNums = nums.map(double); //[6,12,18]
```

We don't have to rely on in-built JavaScript higher-order functions like `.map()`, `.filter()`, `setTimeout()`, `addEventListener()` etc. We can make our own higher-order functions:

```javascript
function addition(a, b) {
  return a + b;
}
function subtraction(a, b) {
  return a - b;
}

function applicator(fn, a, b) {
  return fn(a, b);
}
const sum = applicator(addition, 10, 5);
console.log(sum); // Logs "15"

const sub = applicator(subtraction, 10, 5);
console.log(sub); // Logs "5"
```

### Partial function application.

Partial application is a fancy term for prefilling arguments to a function. It simplifies a function by prefilling arguments so that the newly-returned function accept fewer arguments.

```javascript
function talkTo(person) {
  return function (message) {
    return `${message}, ${person}`;
  };
}

const talkToPuja = talkTo("Puja");

const sayHello = talkToPuja("Hello"); // prefilled with "Puja"
const sayHi = talkToPuja("Hi"); // prefilled with "Puja"

console.log(sayHello); // Logs "Hello, Puja"
console.log(sayHi); // Logs "Hi, Puja"
```

## Summary

- Functions are first-class citizens in JavaScript.
- They can be stored in a variable.
- They can be passed as arguments to functions.
- They can be returned by a function.
- They can be stored in some data structure.
- They can hold their properties and methods.

That's it for today, next day we will study [callbacks and closures](https://github.com/biswarup35/100-days-of-javascript/tree/main/day-eight). If you have anything to say feel free to use [Discussion tab](https://github.com/biswarup35/100-days-of-javascript/discussions) or you can connect me on [Twitter](https://twitter.com/BiswarupBouri).
