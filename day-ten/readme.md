## What is `this` in JavaScript?

Figuring out `this` in JavaScript can be very challenging. In JavaScript `this` behaves differently than other standard languages like Java, C++ etc. `this` points to the instance of the current object in these languages. But in JavaScript, the value of `this` is determined at the run time. Today we will be learning `this` keyword along with`.call()`,`.apply()` and `.bind()` methods.

First, let's know why the value of `this` is determined at the run time in JavaScript.

In JavaScript `this` is the context of function execution, or better say the context of a function invocation. In JavaScript there are four types of function invocation:

- Function invocation
- Method invocation
- Constructor invocation
- Indirect invocation or Apply invocation

The value of `this` depends on any of these invocation patterns, thus most of the javascript developers find it confusing. Understanding `this` would definitely make you a better developer. So don't skip `this` let's learn `this` together.

Before we start let me remind you what happens when a function execution context is created.

When a function execution context is created the JavaScript engine supplies an additional two parameters: `this` and `arguments` to every function execution context. The value of `this` is determined by the invocation pattern.

![this](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/this-debug-1.png)

That's it! The first step is done, now have to understand how `this` behaves with respect to these four invocation patterns.

<hr>

## 1. `this` in Function invocation

### Function invocation means

When a function is not the property of an object, then it is invoked as a function:

```js
function sayHi() {
  console.log("Hello!");
}
sayHi(); // Function invocation pattern
```

![this in function invocation](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/this-function.png)

When a function is invoked with this pattern, `this` is bound to the global object (or undefined in the case of strict mode).

A common mistake the JavaScript developers encounter with function invocation is believing that `this` value is lexically bounded to the inner functions (regular functions) as well.

Let me show you what I mean:

```js
const person = {
  firstName: "John",
  lastName: "Doe",
  getName: function () {
    console.log(this === person); // true
    function getFullName() {
      console.log(this === person); // false
      console.log(`${this.firstName} ${this.lastName}`);
    }
    return getFullName(); // Function invocation pattern
  },
};

person.getName();
```

```
Output:
TypeError: Cannot read property 'firstName' of undefined
```

In the above example inside the outer function `this` is equal to the `person` object. Because `person.getName()` is a method invocation on an object thus `this` equal to the `person`. But `getFullname()` function defined inside the `getName()` function, at this point you might expect to have `this` as `person` object while invoking `getFullName()` function too.

But the `getFullName()` is a function invocation (not a method invocation). Thus, there `this` is the global object (or undefined in the case of strict mode). Though the outer function has `this` context as a `person` object, it does not have any influence on the inner function (in the case of regular function).

The problem can be solved using the arrow function:

```js
const person = {
  firstName: "John",
  lastName: "Doe",
  getName: function () {
    console.log(this === person); // true
    const getFullName = () => {
      console.log(this === person); // true
      console.log(`${this.firstName} ${this.lastName}`);
    };
    return getFullName(); // Function invocation pattern
  },
};

person.getName();
```

```
Output:
John Doe
```

> NOTE: The context of the inner function (except arrow functions) depends only on its own invocation type, but not on the outer function's context.

The problem is solved because the arrow function does not create its own execution context. It resolves `this` lexically.

There is another way to solve this issue by explicitly adding `this` context to a function (regular function) invocation with the help of `.call()` and `.apply()` methods. Let me show you:

```js
const person = {
  firstName: "John",
  lastName: "Doe",
  getName: function () {
    console.log(this === person); // true
    function getFullName() {
      console.log(this === person); // true
      console.log(`${this.firstName} ${this.lastName}`);
    }
    return getFullName.call(this); // Indirect invocation pattern
  },
};

person.getName();
```

Here, the `getFullName.call(this)` function is explicitily setting the `this` context to its outer function's `this` context.

The first argument of `Function.prototype.call(thisArg, ...args)` method is the `this` context, rest are the arguments of the function. We will have more examples later.

> Takeaway: `this` is the global object (or undefined in the case of strict mode) in a function invocation.

<hr>

## 2. `this` in Method Invocation

### Method invocation means

When a function is stored as a property of an object, we call it a method

![this in method invocation](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/this-method.png)

When a method is invoked, `this` is bound to that object. If an invocation expression contains a refinement (dot expression or [subscript] expression) it is invoked as a method.

```js
const counter = {
  count: 0,
  increment: function () {
    console.log((this.count += 1));
  },
};

counter.increment(); // Method invocation pattern
counter.increment(); // Method invocation pattern
counter.increment(); // Method invocation pattern
```

```
Output:
1
2
3
```

A common mistake the JavaScript developers encounter with method invocation is that by extracting the method from an object into a separate variable. Let me show you what I mean:

```js
const counter = {
  count: 0,
  increment: function () {
    console.log((this.count += 1));
  },
};
// Extracting increment() method
// Storing it in a separate variable.
const count = counter.increment;
count(); // Function invoation pattern
```

```
Output:
TypeError: Cannot read property 'count' of undefined
```

When the method is called `count()`, it detached from the original object, you might think that `this` is the object `counter` on which the method was defined.

If the method is called without an object then a function invocation happens, where `this` is the global object (or undefined in case of strict mode)

To resolve this issue we have to set the `this` context to the `counter` object explicitly, for that we have `.call()`, `.apply()` and `.bind()` methods. Generally for that situation `.bind()` method is more preferable. Let me show you what I mean:

```js
const counter = {
  count: 0,
  increment: function () {
    console.log((this.count += 1));
  },
};
//Binding `this` context to the `counter` object
const count = counter.increment.bind(counter);
count();
count();
count();
```

```
Output:
1
2
3
```

Here, we are binding `this` value explicitly to the `increment()` method with the help of the `.bind()` method. This works exactly the same as the `.call()` method except for the indirect invoking. Basically `.bind()` method returns a function that can be invoked later.

There is another example of method extraction, even senior developers got deceived sometimes. Let me show you what I mean:

```js
class Greet {
  constructor(message) {
    this.message = message;
  }
  greeting() {
    console.log(this.message);
  }
}
const hello = new Greet("Hello!"); // Constractor invocation pattern
setTimeout(hello.greeting, 1000);
```

You might believe that `setTimeout(hello.greeting, 1000)` will call the `hello.greeting()` which logs the greeting message from the `hello` object.

Unfortunately, the method is got separated from its object `hello` when passes as a parameter: `setTimeout(hello.greeting)`.

The following cases are equivalent:

```js
setTimeout(hello.greeting, 1000);
```

```js
const extractedGreeting = hello.greeting;
setTimeout(extractedGreeting);
```

When the separated `greeting()` is invoked as a function, `this` is the global object (or undefined in the case of strict mode). So the message does not log correctly.

To solve the problem we use the `.bind()` method. If the separated method is bound with the hello object, the context problem will be resolved.

```js
class Greet {
  constructor(message) {
    this.message = message;
  }
  greeting() {
    console.log(this.message);
  }
}
const hello = new Greet("Hello!"); // Constractor invocation pattern
setTimeout(hello.greeting.bind(hello), 1000);
```

```
Output:
Hello!
```

`hello.greeting.bind(hello)` returns a new function that executes exactly like `greeting()` but has `this` as `hello` even in a function invocation.

The above problem alternately can be resolved using the arrow function:

```js
class Greet {
  constructor(message) {
    this.message = message;
  }
  greeting = () => {
    console.log(this.message);
  };
}
const hello = new Greet("Hello!"); // Constractor invocation pattern
setTimeout(hello.greeting, 1000);
```

The problem is resolved because the arrow function binds `this` context lexically.

> Takeaway: `this` is the object that owns the method in a method invocation.

<hr>

## 3. `this` in Constrcator Invocation

### Constractor invocation means

If a function is invoked with the `new` prefix, then a new object will be created, and `this` will be bunded to the new object.

![this in constructor invocation](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/this-constructor.png)

```js
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

Person.prototype.getFullName = function () {
  console.log(`${this.firstName} ${this.lastName}`);
};

const jane = new Person("Jane", "Doe"); // Constractor invocation pattern
jane.getFullName();
```

Here, the `new Person()` is making a constructor call where the `this` context is the `jane` object. With the help of `Person.prototype` we are inheriting the `getFullName()` method inside the `Person` constructor.

A common mistake the JavaScript developers encounter with constructor invocation is forgetting about the `new` keyword. The reason: some JavaScript functions create instances not only when invoked as a constructor, but also when invoked as functions (factory functions)<br>
Let me give you an example:

```js
function Person() {}

function makeObject(Constractor) {
  return new Constractor();
}

const bob = makeObject(Person); // Function invocation pattern
const rob = new Person(); // Constractor invocation pattern

console.log(bob instanceof Person); //true
console.log(rob instanceof Person); //true
```

A more practical example:

```js
const reg1 = new RegExp("[0-9]"); //Constractor invocation pattern
const reg2 = RegExp("[0-9]"); // Function invocation pattern

console.log(reg1 instanceof RegExp); //true
console.log(reg2 instanceof RegExp); //true
```

Using a function invocation to create objects is a potential problem(excluding factory functions), because some contractors may omit the logic to initialize the object when the `new` keyword is missing. So, make sure to use the `new` keyword when a constructor function invocation is expected.

Let me show an example of how we got deceived:

```js
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}
Person.prototype.getFullName = function () {
  console.log(`${this.firstName} ${this.lastName}`);
};

const jane = new Person("Jane", "Doe"); // Constractor invocation pattern
jane.getFullName(); // Logs "Jane Doe"
```

The moment we forget the `new` keyword the whole context changes because now the function is invoked as function invocation. Therefore, `this` is bound to the global object (or undefined in the case of strict mode). So remember to use the `new` keyword when it is expected.

```js
const monika = Person("Monika", "Doe"); // Function invocation pattern
monika.getFullName();
```

> Takeaway: `this` is the newly created object in a constructor invocation.

<hr>

## 4. `this` in Indirect invocation (apply invocation)

### Indirect invocation means

When a function is called using the `.call()` or `.apply()` method, it is invoked as an indirect invocation or apply invocation. We have already seen a few examples in our above examples. Do not confuse yourself with the method invocation pattern. We can call our functions with any of these methods, how? Because functions are first-class objects in JavaScript they can have their own properties and methods right? Yes, they do have.

![this in indirect invocation](https://github.com/biswarup35/100-days-of-javascript/blob/main/images/this-indirect.png)

The `this` is the first argument of `.call(thisArg, ...args)` or `.apply(thisArg, [...args])` in an indirect invocation or apply invocation.

So far so good, we know that the JavaScript engine supplies `this` context to each function invocation or execution. With `.call()` and `.apply()` we can explicitly set the `this` context to any function (excluding arrow functions). Let me show you what I mean:

```js
function greet(msg) {
  console.log(`${msg}, ${this.name}`);
}

greet.call({ name: "Dyna" }, "Hi"); // Indirect invocation
greet.apply({ name: "Crockford" }, ["Hello"]); // Indirect invocation
```

Let's take another example:

```js
function talk() {
  console.log(this.sound);
}

const dog = {
  sound: "woof woof!!",
};
const cat = {
  sound: "mew mew!!",
};

//Indirect invocation pattern
talk.call(dog); // woof woof!!
talk.call(cat); // mew mew!!
```

Anyway you get the idea, the `this` value in the first argument in these `.call()`, `.apply()` methods.

We have another method that can change the `this` context explicitly as we did with these `.call()` and `.apply()` methods. We can bind `this` context with any function (excluding the arrow function) and invoke it as a bound function later. Sometimes it is very difficult to identify between a function invocation and a bound function.

Bound functions are usually used for attaching the context of `this` to a method that is separated from its object.

The `this` value is the first argument of the `.bind(thisArg, ...args)` method.

```js
function Animal(name, sound) {
  this.name = name;
  this.sound = sound;
}

Animal.prototype.makeSound = function () {
  console.log(this.sound);
};

const cat = new Animal("cat", "mew mew"); // Constractor invocation pattern
// Method separation
const catSound = cat.makeSound;
// calling separated method
catCound(); // TypeError
```

We already saw such examples, we fixed the problem using using `.bind()` method. The function `catSound()` invoked as a function invocation, thus the `this` value is bound to the global object. To rebound its context `.bind()` method is there to rescue. It not only bind the context but binds tightly, even `.call()` and `.apply()` cannot change its value. Let's take an example:

```js
const cat = new Animal("cat", "mew mew");
const dog = new Animal("Dog", "woof woof");
const catSound = cat.makeSound.bind(cat);

catSound.call(dog); // mew mew
catSound.apply(dog); // mew mew
```

<hr>

## 5. `this` in Arrow function (Bonus)

Arrow functions treat `this` keyword differently. They don't define their context since it doesn't have its own `this` context. Arrow function resolves `this` lexically or in other words it inherits from the parent scope whenever you call this.

So, `this` is the enclosing context where the arrow function is defined.

A common mistake the JavaScript developers encounter with `this` in the Arrow function by declaring method (excluding class-based methods) with an arrow function. Let me show you what I mean:

```js
function Animal(name, sound) {
  this.name = name;
  this.sound = sound;
}

Animal.prototype.makeSound = () => {
  console.log(this.sound);
};

const cat = new Animal("cat", "mew mew");
cat.makeSound(); // Method invocation pattern
```

You might believe that the `makeSound()` method has `this` context to its object `cat`. No! the `makeSound()` is an arrow function and it is defined at the global scope thus `this` context will be the global object (or undefined in case of strict mode).

Though the `makeSound()` invoked as method invocation, the `this` context remains unchanged. The invocation pattern has no influence on the arrow function. Arrow functions resolve `this` lexically. So whenever you working with methods it must be declared using a regular function (excluding class-based methods) and for the inner functions you are free to use the arrow function, it will unnecessary `this` context binding.

So, finally, we understand how `this` behaves with respect to different invocation patterns, and we also understand how arrow functions are different from the regular functions.

## Conclusion

For the regular functions, `this` value has an influence on the invocation patterns.

- In function invocation `this` context bounds the global object.
- In method invocation `this` context bounds that object.
- In constructor invocation `this` context bonds that newly created object.
- In indirect invocation `this` value is the first argument.
- In arrow function `this` resolves lexically

<hr>

Hope You get the idea of the `this` keyword in JavaScript. If you have anything to say feel free to use [Discussion tab](https://github.com/biswarup35/100-days-of-javascript/discussions) or you can connect me on [Twitter](https://twitter.com/BiswarupBouri).
