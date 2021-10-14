# Day 4 of 100daysofcode - Hoisting

When you execute a piece of JavaScript code, the JavaScript engine creates a global execution context.

The global execution context has two phases: The memory creation phase and the code execution phase.

During the creation phase, the JavaScript engine stores the function declarations and variables within the execution context.

This behaviour of JavaScript is known as Hoisting.

So, in JavaScript, variables and function declarations will be hoisted. Let me take an example:

```javascript
// variable hoisting
number = 7;
console.log(number); // logs 7
var number;

// function histing
console.log(getNumber(7)); // logs 7

function getNumber(number) {
  return number;
}
```

If you run this script, it will not give an error; this is perfectly valid Javascript code.

Now let's get behind the scenes. Let me put the debugger at the very first line of the script, and watch the `number` variable and the `getNumber()` function carefully.

![Hoisting ](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/hoisting-debug-1.png)

When the debugger is at the number 1, the variable `number` and the function declaration `getNumber()` are already hoisted. Where the variable gets an initial value of `undefined` for the time being. Whereas, the function `getNumber()` it's declaration is completely hoisted.

Now, let's move the debugger to line number 3: Here the JavaScript engine replaces the `undefined` value with the actual value `7` to the variable `number`, hence it logs 7

![Hoisting](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/hoisting-debug-2.png)

And for the function `getNumber()` it's declaration already been hoisted, if you pass `x` number it will return `x`, hence it logs 7

I hope, you get the idea, if not let me take another example:

```javascript
number = 7;
console.log(number); // logs 7
var number;
```

The code is very straightforward, it assigns a number to a variable and then prints it into the console. But when the JavaScript engine sends the code to the compiler, the compiler sees a different code: the code will be transformed into

```javascript
var number;
number = 7;
console.log(number); // logs 7
```

The above phenomenon is known as hoisting.

I know some of you might be thinking, hey you are just declaring variables with `var` keyword. `let` and `const` are not hoisted.

`let` and `const` are too hoisted but they are kept in a temporal dead zone for the time being. Let me show you

As you can see I declared a variable `myName` using the `let` keyword, and try to access it before initialization. I hold the program at the very first line of execution. Here you see `myName` variable is hoisted and assigned an initial value of `undefined` this phenomenon happens only in the case of hoisting.

![Hoisting ](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/hoisting-debug-3.png)

Still, do you have doubts? let me clear all your doubts. If I continue this program what error will occur?

ReferenceError: Cannot access 'myName' before initialization

What does this error mean? it explains that the variable `myName` is already in the memory heap and has not been initialized.

If the variable was not hoisted, I mean not in the memory heap it will result in ReferenceError: `variable` is not defined.

From the above examples, it is clear that `let` and `const` are too hoisted. But kept in a temporal dead zone for the time being.

So far we know that the variables and the functions are hoisted in Javascript.

> Note: Function expression and Arrow functions are not hoisted.

Let me take an another example:

```javascript
let result = add(10, 15);
console.log(result);

// Normal function
function add(a, b) {
  return a + b;
}

// function expression
let add2 = function (a, b) {
  return a + b;
};

// Arrow function
let add3 = (a, b) => {
  return a + b;
};

let result2 = add2(10, 15);
console.log(result2);

let result3 = add3(10, 15);
console.log(result3);
```

I took same function in three diffrent ways, normal function `add()`, function expression `add2()` and the arrow function `add3()`

Now, let me hold the program at the first line of its execution:

![Hoisting](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/hoisting-debug-4.png)

As you can see the normal function `add()` is hoisted. And we are free to use this function before its declaration. While the rest of the two are `undefined` at the moment if you trying to access them it will cause TypeErrr: `add2` is not a function. But we already defined `add2()` function using function expression.

The same applies for `add3()` function. From here, it is clear that the function expression and arrow functions are not hoisted.

## Summary

- JavaScript hoisting occurs during the memory creation phase of the execution context.

- The JavaScript engine hoists the variable declaration with the initial value as `undefined` using the `var` keyword. Whereas, `let` and `const` variable declarations are too hoisted but kept in a temporal dead zone for time being.

- Function expressions and Arrow functions are not hoisted.
