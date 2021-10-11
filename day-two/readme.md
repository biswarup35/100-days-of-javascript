# Day [2,3] of 100daysofcode - JavaScript Call Stack

JavaScript engine uses a data structure named call stack to manage execution context: the global execution context and the function execution context.

The call stack works based on the LIFO principle i.e. Last In First Out.

When we execute a script, the JavaScript engine creates a global execution context and pushes it on the top of the call stack.

When a function is called, the JavaScript engine creates a function execution context for the function and pushes it on the top of the call stack and, start executing the function.

If a function calls another function, the javascript engine creates a new function execution context for the function that is being called and pushes it on the top of the call stack.

When the current function is completed, the javascript engine pops it off from the call stack and resume the execution where it left off.

The script will stop executing when the call stack is empty.

Let me take an example:

```javascript
console.log("start");

function add(a, b) {
  return a + b;
}
function average(a, b) {
  return add(a, b) / 2;
}

let avg = average(10, 20);
console.log(avg); // 15

console.log("end");
```

The code is very straightforward, we already know what is going to happen next.

First, a global execution context will be created and then all the function declarations and variables will be stored.

Our goal is to observe the call stack, we already covered the execution context part.

Let's now see this in action:

![call stack](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/call-stack-debug-1.png)

I put the debugger at the very first line of the script in a `node.js` environment.

The call stack now contains a function denoted as `anonymous` this is the global function. At this point, functions and the variables are hoisted. The next day we will be learning about hoisting, as of now let's keep observing the call stack.

According to the theory, the `average()` will be called. Then within this function the `add()` will be called, and they will be stacked together at the top of each other including the global `anonymous` function. Like this:

```
    add()
    average()
    anonymous()
```

since the `add()` will be called at last then it should be at the top of the call stack.

Let's see this in action, by putting the debugger at line number 3:

![call stack](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/call-stack-debug-2.png)

As expected, all three functions are stacked together in the call stack.

At this point time the `add()` completed its execution, now it will pop off from the call stack. Let's see this in action by putting the debugger at line number 6:

![call stack](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/call-stack-debug-3.png)

As expected, the call stack pops off the `add()` from the call stack. Once the `average()` finishes its job this will too be pops off from the call stack. Let's see this in action too:

![call stack](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/call-stack-debug-4.png)

As expected, `add()` and `average()` both the functions are pops off from the call stack as soon as they finished their job.

At this time the `avg` variable will be assigned with its actual value returned from `average()` Once all the function have done their job the call stack will become empty. Let's see this in action by continuing the debugger:

![call stack](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/call-stack-debug-5.png)

As you can see the call stack is now empty. There is nothing to execute anymore.

## Summary

- When a script calls a function, the JavaScript engine puts it on the top of the call stack for execution.
- If a function is called from another function, the JavaScript engine push it on the top of that function in the call stack.
- When a function finishes its job, the JavaScript engine pops it off from the call stack.
- Call stack has a fixed length, if the number of execution exceeded the size of the stack, a stack overflow will occur.

## Stack overflow

For example, if you execute a recursive function that has no exit condition, it will result in a stack overflow error:

```javascript
function run() {
  run();
}
run(); // stack overflow
```
