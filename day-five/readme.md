# Day 5 of 100 Days of Code - Scope in Javascript

## What is a scope?

The ability to access variables and functions is known as the scope in programming. JavaScript has three types of scope: global scope, local scope and block scope.

Global Scope => If a variable or a function is accessible from anywhere i.e globally, then it has global scope.

Let me take an example:

```javascript
let message = "Good Morning";

(function greeting() {
  (function sayGoodMoring() {
    console.log(message); // => global scope
  })();
})();
//logs
// Good Morning
```

Local Scope (aka function scope) => The variables you declare inside a function are local to that function. And it cannot be accessed outside of the function, this is known as a local scope or function scope.

Let me take an example:

```javascript
let message = "Good Morning";

(function greeting() {
  let message = "Good Evening";
  (function sayGoodEvening() {
    console.log(message); // => local/function scope
  })();
})();

console.log(message);

// logs
// Good Evening
// Good Morning
```

Block Scope => To understand block scope, we must know what is a block?

Anything you see inside `{ }` curly bracket is a block. It can be `if`, `else`, `for`, `while` etc.

Let me take an example:

```javascript
let message = "Good Morning";

if (true) {
  let message = "Good Evening";
  var drink = "Coffee";
  let phone = "Samsung";
  console.log(message);

  if (true) {
    console.log(phone); // Samsung
  }
}
if (true) {
  console.log(phone); //ReferenceError: phone is not defined
}
console.log(message); // Good Morning
console.log(drink); // Coffee
console.log(phone); // ReferenceError: phone is not defined
```

The block scope is quite similar to the function scope. The declared variables are only accessible inside the block.

You might notice the variable `drink` is declared using `var` did not have block scope and it is accessible outside the block.

This is the flaw of the `var` keyword in JavaScript, it somehow manages to function scope but it did not have the block scope. Hence, in ES6 the `let` and `const` keywords are introduced to fix this flaw.

The `let` and `const` has block scope. Variables declared using `let` can be rea-initialized. But variables declared with `const` cannot be re-initialized.

> Tip: Try to declare variables using the `const` keyword whenever possible.

You might have a question, in the case of block scope and function scope the declared variables have access to the local. What if I try to access the variable from an inside function or block?

The answer is yes, it can be accessed from the inner function/block no matter how deep you go.

I already used the `phone` variable from an insider bock. Let's get a bit deeper with another example.

```javascript
let message = "Good Morning";

(function morning() {
  let drink = "Let's have a cup of tea";
  console.log(message);
  console.log(drink);
  console.log("---------");
  (function trunOnComputer() {
    let computer = "Computer turned on";
    console.log(message);
    console.log(drink);
    console.log(computer);
  })();
  console.log(computer);
})();
```

The code is very straightforward, now pay attention to these three variables I declared `message`, `drink` and `computer`.

![Scope](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/scope-debug-1.png)

When the debugger is at `turnOnComputer()` function all three of these variables are assessable. As you can see the `turnOnComputer()` function is at the top of the call stack and all three variables are accessible from here.

As soon as the `turnOnComputer()` lefts the call stack, the variable `computer` becomes not accessible from outer function `morning()` let's see this in action:

![Scope](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/scope-debug-2.png)

I hold the program at line no 14. Here, the function `turnOncompter()` completed its execution and left the call stack. At this moment the `computer` variable is not accessible, it throws ReferenceError: computer is not defined.

From this observation, we found that the most inner function/block has access to the most outer function/block's property i.e variables and functions.

This phenomenon is known as lexical scope, we will study that later.

There is one more interesting thing in scope, let me take an example:

```javascript
var message = "Hello";
let food = "Spring Roll";

if (true) {
  var message = "Hi";
  var food = "Pasta";

  console.log(message);
  console.log(food);
}

console.log(message);
console.log(food);
```

Can you guess the output? You don't have to, cause it will throw a SyntaxError: Identifier 'food' has already been declared.

In this program, an interesting thing just happened.

First the variable `message` has been shadowed. When we declared this variable it was initialized with `Hello`, the same variable has been re-declared and initialized with `Hi`. Within the `if` block it will console log `Hi`, outside the `if` block it should console log `Hello` right?

No! the variable has been shadowed, the initial value is replaced when the variable is re-declared. Now it will console log `Hi`

But when we try the shadow the `food` variable we got an error. The `food` has already been declared, and cannot be re-declared. It was illegal Shadowing. The is the major flaw of the `var` keyword later fixed using `let` and `const` keywords.

The reason, we declared the `food` variable as a global variable, so we will be accessing it from different parts of our program, thus it should not be shadowed, we are allowed to declare the same variable in function/block scope. But we cannot shadow it hence it prevented from re-declaration.

That is why it is recommended to always declare variables with the `let` and `const` keywords.

That's it all about the scope, next day we will study lexical scope.
