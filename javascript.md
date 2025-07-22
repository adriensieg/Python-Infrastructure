# Basic of Javascript

## Syntax
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
    
## Functions
- Function declarations
- Function expressions
- Arrow functions (=>)
- Default parameters
- Rest (...args) and spread syntax
- Callback functions
- Scope and closures
- Immediately Invoked Function Expressions (IIFE)

## Data Structures
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
Functions stored in a variable. Not hoisted — can’t be used before declaration.

``` javascript
const greet = function(name) {
  return `Hi, ${name}`;
};

greet("Bob"); // "Hi, Bob"
```

## Arrow Functions (=>)
A concise function syntax with no own this binding.

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

---

https://www.w3schools.com/nodejs/default.asp

- Web servers and websites
- REST APIs
- Real-time apps (like chat)
- Command-line tools
- Working with files and databases
- IoT and hardware control








