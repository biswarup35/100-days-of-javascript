# Day 9 of 100 Days of Code - Currying and Partial function application.

We know that JavaScript is a multi-paradigm programming language and well suited for a functional programming style. In the functional programming paradigm, there is a concept of Currying that is borrowed from mathematics. Let's not talk about mathematics, we are more interested in Currying in programming. Today we will be learning what the heck is Currying and partial function applications.

## What is Currying?

Currying is a part of the functional programming paradigm, it is an advanced technique of working with functions. In which we can transform a function with multiple arguments into a sequence of nesting functions. Currying does not call a function, it just transforms it.

Sounds confusing right? Let's make it easier to understand for that let me take an example:

Suppose we have a function `someFunction(arg1, arg2, arg3);` which receives three arguments at once. We want to transform that function into three pieces that will receive their respective arguments. After the transformation the above function should look like this `someFunction(arg1)(arg2)(arg3)`.

Humm, sounds intresting. Lets do a parctical, we will curry `sum(a,b,c)` function into callable as `sum(a)(b)(c)`

```js
// The syntax for currying is
function curry(param1) {
  return function (param2) {
    return function (param3) {
      return function (param_n) {
        // Do someting
      };
    };
  };
}

let curried = curry(arg1)(arg2)(arg3)(arg_n);
```

Now lets make a curried version of our `sum(a,b,c)` function:

```js
function curriedSum(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}

const sum = curriedSum(2)(3)(4);
console.log(sum); // Logs "9"
```

Okay, that sounds interesting but what are the advantages of using it? The above example explains how currying works, but it did not provide a proper value. Before giving you a real-world example let me create a helper function that will curry any function which has more than one argument. We could use [lodash](https://www.npmjs.com/package/lodash) for that, but we will make our own.

```js
function curry(callback) {
  return function innerCurry(...args) {
    if (args.length >= callback.length) {
      return callback.apply(this, args);
    } else {
      return function (...args2) {
        return innerCurry.apply(this, [...args, ...args2]);
      };
    }
  };
}
```

Okay now let's test our `sum(1,2,3)` function:

```js
const _sum = (a, b, c) => a + b + c;

const curriedSum = curry(_sum);

const sum1 = curriedSum(1, 2, 3);
console.log(sum1); // Logs "6"
const sum2 = curriedSum(1, 2)(3);
console.log(sum2); // Logs "6"
const sum3 = curriedSum(1)(2)(3);
console.log(sum3); // Logs "6"
```

Works perfectly. To understand the benefits first we need to have a real-world example. Let's make a to-do app.

Our todo app is very straight forward it will display `Time`, `Priority` and the `Task`. <br/>Eg. "11.45 AM: Very Important: Push bug-fix brunch"

```js
function _todo(date, priority, task) {
  return `${date.toLocaleString("IN", {
    hour: "numeric",
    minute: "numeric",
    hour12: true,
  })} : ${priority} : ${task}`;
}

const curriedTodo = curry(_todo);
```

So, we have curried our `_todo()` function with the help of our `curry()` helper function.

Let's now use it normally

```js
const todo = curriedTodo(new Date(), "Very Important", "Push bug-fix branch");

console.log(todo);
```

```
Output:
11.00 AM: Very Importportant: Push bug-fix brunch
```

Now, let's use it by currying:

```js
const todo = curriedTodo(new Date())("Very Important")(
  "Push the bug-fix branch"
);

console.log(todo);
```

```
Output:
11.00 AM: Very Important: Push the bug-fix branch
```

As you can see currying does not call (invoke) the function, rather it transform the function into several pieces (equal to the number of arguments) to enable us to use these functions partially.

Lets set todos for a specific time: <br/>
For that, we don't have to pass all arguments at once. We can proceed step by step.

- At step one: We are initializing the time, and it will remain constant for all todos for that specific time.
  <br/>

example:

```
11.00 AM : Important: Debug Promise Bug
11.00 AM : Important : Fix navbar
```

- At step two: For the rest of the two arguments we can provide them at once or we are allowed to provide separately.

```js
const todoNow = curriedTodo(new Date());
// Providing at once
const taskOne = todoNow("Important", "Debug Promise Bug");
console.log(taskOne);

// Providing separately
const todayImportantTodo = todoNow("Important");
const taskTwo = todayImportantTodo("Fix navbar");
console.log(taskTwo);
```

```
Output:
11.56 AM : Important : Debug Promise Bug
11.56 AM : Important : Fix navbar
```

As you can see it outputs the same as the `_todo()` function would do. The same thing can be achieved using `Function.prototype.bind`. Let's see this in action too:

```js
const todoNow = _todo.bind(this, new Date());
const bindTask = todoNow(
  "Less Important",
  "Complete the wireframe of the X website"
);
```

```
Output:
1.20 PM : Less Important : Complete the wireframe of the X website
the X website
```

The difference is nothing, our goal is to provide the arguments step by step. Sometimes we are unable to provide arguments at once due to some events. In that case, currying is the best solution. I feel that the curried version of the function is more concise.

Let's now set today's todo based on priorities:<br/>
Here the time is already fixed, so we don't have to worry about the 1st argument. It is already there, we just need to provide the rest of the two arguments `priority` and the `task`.

```js
const todayLessImportantTodos = todoNow("Less Important");
const taskThree = todayLessImportantTodos("Delete bug-fix brunch");
console.log(taskThree);
```

```
Output:
2.30 PM : Less Important : Delete bug-fix brunch
```

I already told you, once you have the real-world example you begin to realise the benefits of currying. Currying is not only helping us to use the functions partially but it also helps us to achieve data encapsulation and caching. And all these are possible because of the **closure**

If I had to make a list of advantages of Currying it would be:

### Advantages

- More concise code
- More readability and easier to debug
- Makes the function partially useable
- Helps to achieve data encapsulation and caching

<hr>

## Partial Function Applications

In the beginning, I told you we will be learning Currying and Partial function application. We learned what is currying and what it is advantages. But what the heck is the Partial function application?<br/> Well, you might not have realised but we are using partial functions all along. Still, confused? Let me clear all your doubts.

Remember the syntax of currying?

```js
function curry(param1) {
  return function (param2) {
    return function (param3) {
      return function (param_n) {
        // Do someting
      };
    };
  };
}
```

Pay attention closely, the above function will take the `first argument and returns another function which will receive the `second argument`and it will too return another function which will receive the`third argument`and so forth so on until the`nth argument`. It will create a sequence of functions. But if we are using the function like this: `curry(arg1)(arg2)(arg3)` then we are not leveraging the full potential of the currying. Currying helps us to use the function partially.

Let's take another example:

```js
function mul(a) {
  return function (b) {
    return a * b;
  };
}

// We are fixing the first arguement with value 10
const multiplyByTen = mul(10);

// Lets now reuse the first arguemnt.
// We gonna provide only the 2nd argument,
// The first argument will be pre filled.
console.log(multiplyByTen(2)); // 10X2 = 20
console.log(multiplyByTen(3)); // 10X3 = 30
console.log(multiplyByTen(4)); // 10X4 = 40
console.log(multiplyByTen(5)); // 10X5 = 50
```

In the above example when we are initializing the `multiplyByTen()` function we are pre-filling with 10 so that we don't have to pass the first argument (10) each time. This is known as partial function application. Thus, we can use the rest of the function partially.

In the below example:

- `todoNow()` function is pre-filled with `Time` argument.

- `todayImportantTodo()` function is pre-filled with two arguments (Time, Priorty).

- We are allowed to pre-fill the arguments from top to bottom.

```js
const todoNow = curriedTodo(new Date());
// Providing at once
const taskOne = todoNow("Important", "Debug Promise Bug");
console.log(taskOne);

// Providing separately
const todayImportantTodo = todoNow("Important");
const taskTwo = todayImportantTodo("Fix navbar");
console.log(taskTwo);
```

So, that's it all about partial function application. The same functionality can be achieved by using the `Function.prototype.bind()` method.

## Summary

- Currying is an advanced technique of working with functions.

- It transforms a function (with more than one argument) into a sequence of functions. Which takes the rest argument and produce the desired output.

- Currying helps us to achieve more concise code, reusable, more readability, and easier to debugging

- Partial function application is pre-filling the arguments from top to bottom while using each returned function separately.

<hr>

Hope You get the idea of Currying and Partial Function Applications. Next we will be learning more about `this` keyword in JavaScript, `Function.prototype.call()`, `Function.prototype.apply()` and `Function.prototype.bind()`

If you have anything to say feel free to use [Discussion tab](https://github.com/biswarup35/100-days-of-javascript/discussions) or you can connect me on [Twitter](https://twitter.com/BiswarupBouri).
