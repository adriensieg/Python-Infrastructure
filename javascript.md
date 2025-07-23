# Basic of Javascript

## [Syntax](https://github.com/adriensieg/Python-Infrastructure/blob/master/javascript.md#syntax-1)
- **Variables**:`let`, `const`, `var` (scoping differences)
- **Data types**: Number, String, Boolean, Null, Undefined, Symbol, BigInt
- **Type coercion** and **equality**: `==` vs `===`
- **Operators**: Arithmetic, comparison, logical, assignment, ternary
- **String operations**: Template literals `(${})`, string methods
- **Comments**: `//`, `/* */`

## Control Structures
- **Conditionals**
  - `if`, `else`,`else if`
  - `switch` statement
- **Loops**:
  - `for, while, do...while
  - `for...of`, `for...in`
  - `.forEach()`, `.map()`, `.filter()`, `.reduce()`
- **Jumps**
  - `break`,
  - `return`,
  - and `throw`
    
## [Functions](https://github.com/adriensieg/Python-Infrastructure/blob/master/javascript.md#functions-1)
- Function declarations
- Function expressions
- Arrow functions (=>)
- Default parameters
- Rest (...args) and spread syntax
- Callback functions
- Scope and closures
- Immediately Invoked Function Expressions (IIFE)
- Higher-Order Functions

## [Data Structures](https://github.com/adriensieg/Python-Infrastructure/blob/master/javascript.md#data-structures-1)
- **Arrays**: CRUD operations, iterating, sorting, destructuring
- **Objects**: Key-value pairs, accessing, updating, deleting
- **Shorthand notation**
- **Object destructuring**
- `Sets` and `Maps`

- **Primitive Structures**: `Arrays`, `Objects`
- **Linear Data Structures**: `Stacks`, `Queues`, Linked Lists
- **Non-linear Data Structures**: Trees (Binary, BST), Graphs
- **Hash-based Structures**: Hash Tables (`Maps`, `Sets`)
- **Built-in Structures**: `Map`, `Set`, `WeakMap` / `WeakSet`

## Error Handling
- `try...catch` block
- `throw` statement
- Custom error creation
- `finally` clause

## Objects and Prototypes
- Object creation patterns
- `this` keyword (behavior differs from Python!)
- Constructor functions
- Prototypes and prototype chain
- Object.create()
- ES6 Classes and Inheritance
- `super`, `extends`, `constructor`

## Asynchronous
- Callbacks
- Promises
- `.then()`, `.catch()`, `.finally()`
- `async` / `await`
- Fetch API (for HTTP requests)
- Error handling in async code
- **Event Loop**
  - JavaScript engine > single-threaded
  - Task queue / Callback queue
  - Events, timers, user interactions
  - Call stack

## DOM Manipulation (Minimal - for Node focus)
- `document.querySelector`, `getElementById`, etc.
- Event listeners: `.addEventListener`
- DOM traversal and updates

## Working with JSON
- `JSON.parse()`, `JSON.stringify()`

## Server-side
- **Networking**
  - `fetch()`
  - Server-Sent Events
  - WebSockets
- **Storage**
  - `localStorage` and `sessionStorage`
  - `Cookies`
  - `IndexedDB`
- **Worker Threads** and **Messaging**
  - Worker Objects
  - The Global Object in Workers
  - Importing Code into a Worker
  - Worker Execution Model
  - `postMessage()`, `MessagePorts` and MessageChannels
  - Cross-Origin Messaging with `postMessage()`

## [Class & OOP Basics](https://github.com/adriensieg/Python-Infrastructure/blob/master/javascript.md#classes--oop-basics)
- Class Basics
- Constructor
- Methods
- `this` Keyword
- Inheritance (`extends` and `super`)
- Static Methods
- Getters and Setters
- Private Fields (`#field`)

## Node.js
- CommonJS modules (require, module.exports)
- ES Modules (import, export)
- Built-in Node.js modules: `fs`, `path`, `http`, etc.
- Event-driven architecture
- Setting up Express (basic routing)
- Working with files & HTTP
- Middleware basics
- Environment variables

---

# Syntax

**Primitive Types**: These are **atomic** and **immutable values**.

| Type        | Description                               | Example                           |
| ----------- | ----------------------------------------- | --------------------------------- |
| `Number`    | Numeric data (integers & floats)          | `42`, `3.14`, `-0.5`              |
| `BigInt`    | For arbitrarily large integers            | `123456789012345678901234567890n` |
| `String`    | Textual data                              | `"Hello"`, `'World'`              |
| `Boolean`   | Logical true/false                        | `true`, `false`                   |
| `undefined` | A variable declared but not assigned      | `let x; // x is undefined`        |
| `null`      | Explicit absence of value                 | `let user = null;`                |
| `Symbol`    | Unique identifier (often for object keys) | `let sym = Symbol('id')`          |
| `bigint`    | Same as `BigInt` (used with `n` suffix)   | `100n`                            |

**Non-Primitive (Object) Types**: These are **mutable** and can hold **collections of values**.

| Type       | Description                          | Example                     |
| ---------- | ------------------------------------ | --------------------------- |
| `Object`   | Key-value pairs                      | `{ name: "John", age: 30 }` |
| `Array`    | Ordered list                         | `[1, 2, 3]`                 |
| `Function` | Reusable code block                  | `function greet() { ... }`  |
| `Date`     | Date & time handling                 | `new Date()`                |
| `RegExp`   | Pattern matching                     | `/\d+/`                     |
| `Map`      | Key-value with any type keys         | `new Map()`                 |
| `Set`      | Unique values                        | `new Set([1,2,3])`          |
| `WeakMap`  | Like `Map` but with weakly held keys | `new WeakMap()`             |
| `WeakSet`  | Like `Set` but weak references       | `new WeakSet()`             |
| `Error`    | Error object for exceptions          | `new Error("Oops!")`        |

# Control Structures

# Functions

## Function Declarations
Named functions declared using the `function` keyword. They are **hoisted** — **usable before their definition**.

``` javascript
function greet(name) {
  return `Hello, ${name}`;
}

greet("Alice"); // "Hello, Alice"
```

**Hoisted** means you can call a function **before you actually write it in the code**, and it will still work. They are moved to the top of their scope before the code is run.

``` javascript
sayHello(); // This works!

function sayHello() {
  console.log("Hello!");
}
```

## Function Expressions
Functions stored in a variable. Not hoisted — **can’t be used before declaration**.

``` javascript
const greet = function(name) {
  return `Hi, ${name}`;
};

greet("Bob"); // "Hi, Bob"
```

## Arrow Functions (=>)
A concise function syntax with no own this binding.
Great for **short**, **anonymous** functions or callbacks. **Avoid for object methods**.
**Fast**, **lightweight**, but not ideal for **heavy tasks** (like handling `this`)

``` javascript
const greet = (name) => `Hey, ${name}`;
greet("Charlie"); // "Hey, Charlie"
```

**Arrow functions** behave differently from **regular functions** when it comes to the `this` keyword:
- **Arrow functions** do **not** have their own `this`.
- Instead, they **inherit** `this` from their **lexical (outer) scope**.

This makes arrow functions **simpler** and **shorter**, but also **less flexible in certain cases** — especially when working with objects, classes, or event handlers where you need a proper this.

❌ Arrow Function (not ideal here):

``` javascript
const person = {
  name: "Alice",
  greet: () => {
    console.log(`Hi, I'm ${this.name}`);
  }
};

person.greet(); // "Hi, I'm undefined"
```

Arrow function inherits `this` from the **outer scope** (likely **window** or **global**), **not** `person`.
- So `this.name` is **undefined**.

Regular Function (better here):

``` javascript
const person = {
  name: "Alice",
  greet: function () {
    console.log(`Hi, I'm ${this.name}`);
  }
};

person.greet(); // "Hi, I'm Alice"
```

- **Best Use Cases for Arrow Functions**
``` javascript
const numbers = [1, 2, 3];
const doubled = numbers.map(n => n * 2); // ✅ Arrow = concise
```
## Default Parameters
Allow setting fallback values for function parameters.
When you want to avoid undefined or provide optional parameters.

``` javascript
function greet(name = "Guest") {
  return `Welcome, ${name}`;
}

greet(); // "Welcome, Guest"
```

## Rest (...args) and Spread Syntax

``` javascript
// Rest
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}

// Spread
const nums = [1, 2, 3];
const all = [...nums, 4]; // [1, 2, 3, 4]
```

## Callback Functions
https://www.geeksforgeeks.org/javascript/javascript-callbacks/

A callback function is a function that is passed as an argument to another function and executed later.
- A function can accept **another function as a parameter**.
- Callbacks allow **one function** to **call another at a later time**.
- A callback function can **execute after another function has finished**.

``` javascript
function processUser(name, callback) {
  callback(`Processed: ${name}`);
}

processUser("Dana", console.log); // "Processed: Dana"
```

### Where Are Callbacks Used?

<mark>1. Handling **Asynchronous Operations**</mark>
  - **API requests** (fetching data)
  - **Reading files** (Node.js file system)
  - **Event listeners** (clicks, keyboard inputs)
  - **Database queries** (retrieving data)

<mark>2. Callbacks in Functions Handling Operations</mark>

``` javascript
function calc(a, b, callback) {
    return callback(a, b);
}

function add(x, y) {
    return x + y;
}

function mul(x, y) {
    return x * y;
}

console.log(calc(5, 3, add)); // 8
console.log(calc(5, 3, mul)); // 15
```
<mark>3. Callbacks in Event Listeners</mark>

- JavaScript is **event-driven**, and callbacks handle user interactions like clicks and key presses.
- the anonymous function is a callback that runs when the button is clicked

``` javascript
document.getElementById("myButton").addEventListener("click", function () {
    console.log("Button clicked!");
});
```

<mark>4. Callbacks in API Calls (Fetching Data)</mark>

``` javascript
function fetch(callback) {
    fetch("https://jsonplaceholder.typicode.com/todos/1")
        .then(response => response.json())
        .then(data => callback(data))
        .catch(error => console.error("Error:", error));
}

function handle(data) {
    console.log("Fetched Data:", data);
}

fetch(handle);
```

#### Features of JavaScript Callbacks
- **Asynchronous Execution**: Handle async tasks like API calls, timers, and events without blocking execution.
- **Code Reusability**: Write modular code by passing different callbacks for different behaviors.
- **Event-Driven Programming**: Enable event-based execution (e.g., handling clicks, keypresses).
- **Error Handling**: Pass errors to callbacks for better control in async operations.
- **Non-Blocking Execution**: Keep the main thread free by running long tasks asynchronously.

#### Problems with Callbacks

1. **Callback Hell (Nested Callbacks)** - When callbacks are **nested deeply**, the code becomes unreadable and hard to maintain. As the number of steps increases, the nesting grows deeper, making the code difficult to manage.

2. **Error Handling Issues in Callbacks** - Error handling can get messy when dealing with nested callbacks.

#### Alternatives to Callbacks

1. **Promises** (Fixing Callback Hell) - Promises provide a better way to **handle asynchronous tasks** without deep nesting.
2. **Async/Await** (Cleaner Alternative)
   
``` javascript

// CALLBACK
function step1(callback) {
    setTimeout(() => {
        console.log("Step 1 completed");
        callback();
    }, 1000);
}

function step2(callback) {
    setTimeout(() => {
        console.log("Step 2 completed");
        callback();
    }, 1000);
}

function step3(callback) {
    setTimeout(() => {
        console.log("Step 3 completed");
        callback();
    }, 1000);
}

step1(() => {
    step2(() => {
        step3(() => {
            console.log("All steps completed");
        });
    });
});

// PROMISES -- Fixing Callback Hell

function step1() {
    return new Promise(resolve => {
        setTimeout(() => {
            console.log("Step 1 completed");
            resolve();
        }, 1000);
    });
}

function step2() {
    return new Promise(resolve => {
        setTimeout(() => {
            console.log("Step 2 completed");
            resolve();
        }, 1000);
    });
}

function step3() {
    return new Promise(resolve => {
        setTimeout(() => {
            console.log("Step 3 completed");
            resolve();
        }, 1000);
    });
}

step1()
    .then(step2)
    .then(step3)
    .then(() => console.log("All steps completed"));

// ASYNC/AWAIT

async function processSteps() {
    await step1();
    await step2();
    await step3();
    console.log("All steps completed");
}

processSteps();
```

#### When to Use and Avoid Callbacks?
- Use callbacks when:
  - Handling **asynchronous tasks** (API calls, file reading).
  - Implementing **event-driven programming**.
  - Creating **higher-order functions**.
    
- Avoid callbacks when
  - Code becomes **nested** and **unreadable** (use **Promises** or **async/await**).
  - You need error handling in asynchronous operations (Promises are better).

## Scope and Closures
Use **closures** to create **private state** or **partial functions**.

- **Scope**: determines where variables are accessible.
- **Closure**: an inner function “remembers” variables from outer function.

``` javascript
function outer() {
  let count = 0;
  return function inner() {
    return ++count;
  };
}

const counter = outer();
counter(); // 1
counter(); // 2
```

## Immediately Invoked Function Expressions (IIFE)

## Higher Order functions
https://www.freecodecamp.org/news/higher-order-functions-in-javascript-explained/

A higher order function is a function that takes one or more functions as arguments, or returns a function as its result.

## Arrays

**Arrays**: A collection of elements, accessible by index.

``` javascript
const arr = [1, 2, 3, 4];

// Access
console.log(arr[0]);

// Iterate
arr.forEach(item => console.log(item));

// Add / Remove
arr.push(5);
arr.pop();
```

# Data Structures

## Objects

``` javascript
const person = {
  name: "Alice",
  age: 30,
  greet: function() {
    console.log("Hello!");
  }
};
```

- `name` and `age` are <mark>properties</mark>
- `greet` is a <mark>method</mark> (a function attached to an object)

#### Creating Objects

``` javascript
// Object literal
const obj1 = { a: 1, b: 2 };

// Constructor
const obj2 = new Object();
obj2.a = 1;

// From class (ES6+)
class Person {
  constructor(name) {
    this.name = name;
  }
}
const obj3 = new Person("Bob");
```
- Arrays are objects: 'typeof [1,2,3] === object'
- Functions are objects: 'typeof function(){} === function' (but still an object)
- Almost everything in JS is built on objects

#### Accessing & Modifying Properties

``` javascript
const car = {
  make: "Toyota",
  year: 2020
};

console.log(car.make);       // Dot notation
console.log(car["year"]);    // Bracket notation

car.model = "Camry";         // Add property
delete car.year;             // Remove property
```

### Looping Through an Object

``` javascript
const user = { name: "Eve", age: 22 };

for (let key in user) {
  console.log(`${key}: ${user[key]}`);
}

// Or:
Object.keys(user).forEach(key => {
  console.log(key, user[key]);
});
```

#### Properties

##### Objects Can Be Nested

``` javascript
const user = {
  name: "Tom",
  address: {
    city: "New York",
    zip: 10001
  }
};

console.log(user.address.city); // "New York"
```

##### Built-in Object Utilities

``` javascript
Object.keys(obj);     // Get keys
Object.values(obj);   // Get values
Object.entries(obj);  // Get [key, value] pairs
Object.assign({}, obj); // Clone
``` 

- How object inheritance works with `prototype`
- How to deeply clone or merge objects

- Difference between `WeakSet`, `WeakMap`, and `Object` is key to mastering **memory management** and **reference** handling in JavaScript

| Feature               | `Object`                | `WeakMap`                    | `WeakSet`                              |
| --------------------- | ----------------------- | ---------------------------- | -------------------------------------- |
| Key/value storage     | ✅ Yes                   | ✅ Yes                        | ❌ No (only values, no keys)            |
| Keys must be objects? | ❌ No (string or symbol) | ✅ Yes (only objects)         | ✅ Yes (only objects)                   |
| Garbage collection    | ❌ No                    | ✅ Keys are weakly held       | ✅ Values are weakly held               |
| Iterable?             | ✅ Yes                   | ❌ No                         | ❌ No                                   |
| Use case              | General key-value store | Private data tied to objects | Track object presence (no duplication) |
| Prevents memory leaks | ❌ No                    | ✅ Yes                        | ✅ Yes                                  |

## What is `this` in JavaScript?

| Context                           | `this` refers to                              |
| --------------------------------- | --------------------------------------------- |
| In global scope (non-strict mode) | `window` (in browser) or `global` (Node.js)   |
| In a method (called on an object) | That object                                   |
| In a function (strict mode)       | `undefined`                                   |
| In a function (non-strict mode)   | `window` or `global`                          |
| In an arrow function              | Lexical `this` (i.e., from surrounding scope) |
| In a class                        | The class instance                            |
| With `bind`, `call`, or `apply`   | The explicitly set value                      |


#### 1. Global Scope (non-strict)

``` javascript
console.log(this); // In browser: window
```

#### 2. In a Method
``` javascript
const user = {
  name: "Alice",
  greet() {
    console.log(this.name); // "Alice"
  }
};

user.greet();
```

#### 3. In a Regular Function

``` javascript
function show() {
  console.log(this);
}

show(); // global object (non-strict) or undefined (strict mode)
``` 

#### 4. In an Arrow Function

``` javascript
const obj = {
  name: "Bob",
  greet: () => {
    console.log(this.name); // undefined (not bound to `obj`)
  }
};

obj.greet();
```
➡️ Arrow functions do not have their own this; they inherit it from the outer lexical context.

#### 5. Using bind, call, apply

``` javascript
function greet() {
  console.log(this.name);
}

const person = { name: "Charlie" };

greet.call(person); // Charlie
greet.apply(person); // Charlie

const boundGreet = greet.bind(person);
boundGreet(); // Charlie
```

#### 6. In a Class
``` javascript
class Dog {
  constructor(name) {
    this.name = name;
  }

  bark() {
    console.log(`${this.name} says woof`);
  }
}

const d = new Dog("Rex");
d.bark(); // Rex says woof
```
## Callbacks

A **callback** is a **function passed as an argument to another function** to be executed later. 
It's how JavaScript handles **async work**, like **timers**, **I/O**, or **user input**.

- Imagine you're ordering food at a restaurant. You give the waiter your order and say: “Call me when the food is ready.”
- That’s a callback — you're giving them a function to call later, when the task is done.

“Do this... and when you’re done, call this function I gave you.”

#### Why Use Callbacks?
1. Handle **asynchronous actions** (like waiting for a file, or a network request)
2. **Customize** what happens after something completes
3. **Avoid blocking the main thread**

| Feature        | Callback                         | Promise                | async/await            |
| -------------- | -------------------------------- | ---------------------- | ---------------------- |
| Syntax         | Function argument                | `.then()` / `.catch()` | `await` inside `async` |
| Readability    | Can get messy (nested)           | Better chaining        | Clean, readable        |
| Error Handling | Manual (`try/catch` in callback) | Built-in `.catch()`    | `try/catch` block      |

# Promises
Imagine that you’re a top singer, and fans ask day and night for your upcoming song.

To get some relief, you promise to send it to them when it’s published. You give your fans a list. They can fill in their email addresses, so that when the song becomes available, all subscribed parties instantly receive it. And even if something goes very wrong, say, a fire in the studio, so that you can’t publish the song, they will still be notified.

Everyone is happy: you, because the people don’t crowd you anymore, and fans, because they won’t miss the song.
https://javascript.info/promise-basics

## Classes & OOP Basics

| Concept       | Purpose                           | Keyword/Usage         |
| ------------- | --------------------------------- | --------------------- |
| Class         | Object blueprint                  | `class`               |
| Constructor   | Initialization logic              | `constructor()`       |
| Method        | Behavior of objects               | Function inside class |
| `this`        | Reference to current object       | `this.prop`           |
| Inheritance   | Extend other classes              | `extends`, `super()`  |
| Static Method | Utility functions on class itself | `static method()`     |
| Get/Set       | Controlled property access        | `get`, `set`          |
| Private Field | Hidden internal data              | `#field`              |



---

https://www.w3schools.com/nodejs/default.asp

- Web servers and websites
- REST APIs
- Real-time apps (like chat)
- Command-line tools
- Working with files and databases
- IoT and hardware control








