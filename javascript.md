# Basic of Javascript

## [Syntax](https://github.com/adriensieg/Python-Infrastructure/blob/master/javascript.md#syntax-1)
- **Variables**:`let`, `const`, `var` (scoping differences)
- **Data types**: Number, String, Boolean, Null, Undefined, Symbol, BigInt
- **Type coercion** and **equality**: `==` vs `===`
- **Operators**: Arithmetic, comparison, logical, assignment, ternary
- **String operations**: Template literals `(${})`, string methods
- **Comments**: `//`, `/* */`

## [Control Structures](https://github.com/adriensieg/Python-Infrastructure/blob/master/javascript.md#control-structures-1)
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
- Currying

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

## Looping Arrays

### Classic for loop

``` javascript
const arr = [1, 2, 3];
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

### `for`...`of` – Modern & clean

``` javascript
for (const item of arr) {
  console.log(item);
}
```

### `.forEach()` – Functional

``` javascript
arr.forEach(item => console.log(item));
```

### `.map()` – Returns transformed array
``` javascript
const doubled = arr.map(item => item * 2);
``` 
### `.filter()` – Filters values
``` javascript
const even = arr.filter(item => item % 2 === 0);
``` 
### `.reduce()` – Aggregates to single value
``` javascript
const sum = arr.reduce((acc, val) => acc + val, 0);
```

## Looping Objects

### `for...in` – Old but works
``` javascript
const obj = { a: 1, b: 2 };
for (const key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(key, obj[key]);
  }
}
```
### `Object.keys()` + `forEach`
``` javascript
Object.keys(obj).forEach(key => {
  console.log(key, obj[key]);
});
```

### `Object.entries()` + `for...of`
``` javascript
for (const [key, val] of Object.entries(obj)) {
  console.log(`${key}: ${val}`);
}
```

### Looping Maps
``` javascript
const map = new Map([
  ['a', 1],
  ['b', 2]
]);

// Using for...of
for (const [key, value] of map) {
  console.log(key, value);
}

// Using forEach
map.forEach((value, key) => {
  console.log(key, value);
});
```

### Looping Sets
``` javascript
const set = new Set([1, 2, 3]);

for (const item of set) {
  console.log(item);
}

// or
set.forEach(item => console.log(item));
```

### Looping Strings - Strings are iterable.

``` javascript
const str = "hello";

for (const char of str) {
  console.log(char);
}
```

### Looping NodeLists (e.g., from document.querySelectorAll)

``` javascript
const elements = document.querySelectorAll('div');

// Using for...of
for (const el of elements) {
  console.log(el);
}

// Using forEach
elements.forEach(el => console.log(el));
```

### Using `while` and `do...while`
``` javascript
let i = 0;

while (i < 3) {
  console.log(i);
  i++;
}

do {
  console.log(i);
  i++;
} while (i < 5);
```

- Use **while** when:
  - You may want to skip the loop entirely if the condition is false.
  - Example: Polling until a condition is met.
    
- Use **do...while** when:
  - You want to run the code at least once, regardless of the condition.
  - Example: Prompting the user until valid input is given.

``` javascript
let i = 6;

do {
  console.log(i);
  i++;
} while (i < 5);
```
 
- First Run (before checking condition): i = 6
- It prints 6
- Then i++ → i = 7
- ❓ Now check the condition:
- i < 5 → is 7 < 5? ❌ False
- Since the condition is false, the loop stops after just one run.
    
| Feature              | `while`                  | `do...while`                 |
| -------------------- | ------------------------ | ---------------------------- |
| Condition checked... | **before** each loop run | **after** the first run      |
| Guaranteed 1 run?    | ❌ No                     | ✅ Yes                        |
| Use when...          | You only run *if needed* | You must run *at least once* |

#### Summary

| Structure | Preferred Loop                  | Notes                                             |
| --------- | ------------------------------- | ------------------------------------------------- |
| Arrays    | `for...of`, `.forEach()`        | `for...of` if using `break`, `continue`           |
| Objects   | `Object.entries()` + `for...of` | Avoid `for...in` unless checking `hasOwnProperty` |
| Maps      | `for...of` or `.forEach()`      | Both are clean                                    |
| Sets      | `for...of` or `.forEach()`      | Easy and idiomatic                                |
| Strings   | `for...of`                      | Strings are iterable                              |
| NodeLists | `for...of` or `.forEach()`      | Modern browsers support both                      |


### When to Avoid Certain Loops
- ❌ Avoid `for...in` on arrays: Iterates inherited properties, not ideal.
- ❌ Avoid `.forEach()` if you need break/continue/return.
- ✅ Use `for...of` for most iterable-safe, readable code.
- ✅ Use `.map()`, `.filter()`, `.reduce()` for functional, declarative patterns.


## Conditions

### if, else if, else

if (x > 10) {
  // do something
} else if (x > 5) {
  // do something else
} else {
  // fallback
}

### Ternary Operator ? : – For simple expressions

``` javascript
const result = score > 50 ? "Pass" : "Fail";
```

### switch – For multiple fixed cases (especially strings/numbers)

``` javascript
switch (day) {
  case 'Mon':
    console.log("Start of week");
    break;
  case 'Fri':
    console.log("Weekend coming");
    break;
  default:
    console.log("Midweek");
}
```

### Short-circuiting with && or ||

``` javascript
isLoggedIn && showDashboard(); // if true, executes
isAdmin || showLogin();        // if false, executes
```

### Nullish Coalescing ??
``` javascript
const name = inputName ?? "Guest"; // if inputName is null or undefined
```

### Optional Chaining ?. with Condition

if (user?.profile?.email) {
  console.log("Email found");
}

## Common Conditional Patterns
🔸 Check for truthy/falsy
js
Copy
Edit
if (value) { ... } // truthy values
if (!value) { ... } // falsy: false, 0, "", null, undefined, NaN
🔸 Check for existence
js
Copy
Edit
if (typeof obj !== 'undefined') { ... }
🔸 Type-safe comparisons
js
Copy
Edit
if (x === 10) { ... }  // ✅ strict equality
🔸 Check array length
js
Copy
Edit
if (arr.length > 0) {
  console.log("Array is not empty");
}

## Conditionals on Data Structures

### Array

```javascript
// ✅ Check if empty
if (arr.length === 0) { ... }

// ✅ Check if includes item
if (arr.includes('admin')) { ... }

// ✅ Some / Every
if (arr.some(user => user.active)) { ... }
if (arr.every(score => score >= 50)) { ... }

// ✅ Conditional map/filter
const result = isAdmin ? users.map(...) : users.filter(...);
```

### Object
``` javascript
// ✅ Check property exists
if ('name' in obj) { ... }
if (obj.hasOwnProperty('name')) { ... }

// ✅ Check value

if (obj.role === 'admin') { ... }

// ✅ Optional chaining

if (obj?.settings?.darkMode) { ... }

// ✅ Check if object is empty
if (Object.keys(obj).length === 0) { ... }
```

### Map
``` javascript
const map = new Map();
map.set('a', 1);

if (map.has('a')) {
  console.log(map.get('a'));
}
```
### Set
``` javascript
const set = new Set(['a', 'b']);

if (set.has('a')) {
  console.log("Set contains 'a'");
}
```

### String
``` javascript
if (str.includes('error')) { ... }
if (str.startsWith('http')) { ... }
if (str.trim() === '') { ... } // empty check
```

### Function Guard Clauses (Best Practice)
Instead of:
``` javascript
if (user) {
  if (user.isActive) {
    doSomething();
  }
}
```
✅ Use guard clause:
``` javascript
if (!user?.isActive) return;
doSomething();
```

### 🧠 Elegant Patterns

✅ Early Return (Avoid nesting)
``` javascript
function handle(user) {
  if (!user) return;
  if (!user.active) return;
  // clean, readable
}
```

✅ Use ternary only for short expressions
``` javascript
const status = age >= 18 ? "Adult" : "Minor"; // ✅
``` 
Not this:

``` javascript
age >= 18 ? doAdultStuff() : doMinorStuff(); // ❌ too complex
```

✅ Use switch for fixed known values
``` javascript
switch (status) {
  case 'pending':
  case 'approved':
  case 'denied':
    // clean branching
    break;
}
```














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
  
- **Scope** is the area in a program **where a variable can be seen and used**.
- In JavaScript, **scopes** can be **nested**, like boxes inside boxes. This means that a function (or block) inside another can access variables from the outer one.
- There are different types of scopes:
  - **Global scope** (available everywhere),
  - **Module scope** (limited to a file or module), and
  - **Block scope** (limited to {} blocks like inside functions, loops, or if statements).
 
- A **closure** happens when a function is **created inside another function**, and it remembers the variables from the **outer function**, **even after the outer function has finished running**.
- **Closures** let **inner functions keep using variables** that would **otherwise be gone** — like the function took a memory snapshot of those variables when it was created.

#### Analogy: 
- Think of **scope** like **rooms in a building** — you can only use tools (variables) that are in your room or in any room you're nested inside.
- A closure is like someone who **walked out of the room** but brought a walkie-talkie that **still talks to the tools inside** — even if the room no longer exists for anyone else.

#### Why Should we Care?
- `Scope`
  - Prevents **name clashes** (like two variables named `count` interfering).
  - Makes your code **modular**, **safe**, and easier to debug.
  - Helps enforce information hiding — **only exposing what’s necessary**.

- `Closures`
  - Enables **data privacy** and **encapsulation** (think of private variables).
  - Lets functions **remember data without global variables**.
  - Powers **callbacks**, **event handlers**, **factory functions**, and **currying**.

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

#### 1. Private Variables in a Module (Encapsulation)
- **Closures** allow you to **hide sensitive data inside a function**. Business-wise: this means **safer**, **more secure**, and **less buggy code**.
- You protect **internal data** like balance from being **accessed** or **tampered** with directly.

``` javascript
function createBankAccount() {
  let balance = 1000;

  return {

    deposit(amount) {
      balance += amount;
      return balance;
    },

    withdraw(amount) {
      if (amount <= balance) {
        balance -= amount;
        return balance;
      }
      return "Insufficient funds";
    },

    getBalance() {
      return balance;
    }

  };
}

const account = createBankAccount();
account.deposit(500);       // 1500
account.withdraw(200);      // 1300
console.log(account.balance); // ❌ undefined (private!)
```
#### 2. Factory Functions for Custom Logic
You can use closures to generate specialized tools or handlers based on input — very useful in e-commerce, dashboards, or UIs.

``` javascript
function createDiscount(rate) {
  return function(price) {
    return price - (price * rate);
  };
}

const summerSale = createDiscount(0.2); // 20% off
summerSale(100); // $80
```
#### 3. Event Handlers and Asynchronous Behavior
In web apps, **closures** are essential for **handling events** or **delayed tasks correctly**.

``` javascript
function setupButton() {
  let clicks = 0;

  document.getElementById("myBtn").addEventListener("click", function () {
    clicks++;
    console.log(`Clicked ${clicks} times`);
  });
}
```
#### 4. Currying / Partial Application (Functional Programming)
This is about building more **flexible**, **composable** functions.

``` javascript
function multiply(a) {
  return function(b) {
    return a * b;
  };
}

const double = multiply(2);
double(5); // 10
```

## Immediately Invoked Function Expressions (IIFE)

📌 A function that runs as soon as it’s defined - Like a firework — you light it (define) and it explodes (runs) instantly.

``` javascript
(function () {
  console.log("I run immediately!");
})();
```

## Higher Order functions
https://www.freecodecamp.org/news/higher-order-functions-in-javascript-explained/

A **higher order function** is a function that takes **one** or **more functions as arguments**, or **returns a function as its result**.

## Currying
https://www.geeksforgeeks.org/python/currying-function-in-python/


**Currying** is a functional programming technique where **a function with multiple arguments** is **transformed into a sequence of functions**, each taking **a single argument**.

#### Mathematical Illustration
- In **currying**, a function that **takes multiple inputs** is broken down into **a series of single-argument functions**, each **processing one input at a time** and ultimately returning the final result. For example:

$f(x, y) = x^3 + y^3$  
$h(x) = x^3$  
$h(y) = y^3$  
$f(x, y) = h(x) + h(y)$

Let’s create a composite function where $a(x) = b(c(d(x)))$, meaning that 
- function $d(x)$ is executed first,
- its output is passed to $c(x)$ and
- finally, $b(x)$ processes the result.

``` python
def change(b, c, d):
    def a(x):
        return b(c(d(x)))
    return a

def km_to_m(dist):
    """ Function that converts km to m. """
    return dist * 1000

def m_to_cm(dist):
    """ Function that converts m to cm. """
    return dist * 100

def cm_to_f(dist):
    """ Function that converts cm to ft. """
    return dist / 30.48

transform = change(cm_to_f, m_to_cm, km_to_m)
print(transform(565))
```

A Coffee Order Function
- Imagine you're ordering coffee. The process goes like this:
- You choose the **size** of the coffee.
- Then you pick the **type** (like latte or espresso).
- Finally, you decide if you want **sugar** or not.

``` javascript
// Curried function
function orderCoffee(size) {
  return function(type) {
    return function(sugar) {
      return `You ordered a ${size} ${type} with ${sugar ? 'sugar' : 'no sugar'}.`;
    };
  };
}

// Usage:
const order = orderCoffee('Large')('Latte')(true);
console.log(order);  // Output: You ordered a Large Latte with sugar.
```
### ES6 new norms
``` javascript
const sendReq = greet => name => message =>
`${greet} ${name}, ${message}`
sendReq('Hello')('Harry')('Can you please add me as your connection on LinkedinIn?')
```

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

#### Syntax: 
``` javascript
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```

- Its arguments `resolve` and `reject` are **callbacks**
- When the executor obtains the result, be it soon or late, doesn’t matter, it should call one of these callbacks:
  - resolve(value) — if the job is finished successfully, with result value.
  - reject(error) — if an error has occurred, error is the error object.

he executor runs automatically and attempts to perform a job. When it is finished with the attempt, it calls resolve if it was successful or reject if there was an error.

The promise object returned by the new Promise constructor has these internal properties:

- state — initially "pending", then changes to either "fulfilled" when resolve is called or "rejected" when reject is called.
- result — initially undefined, then changes to value when resolve(value) is called or error when reject(error) is called.

So the executor eventually moves promise to one of these states:

<img width="641" height="267" alt="image" src="https://github.com/user-attachments/assets/270b4d4d-25bb-41b3-826f-47264dcff533" />

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


https://medium.com/free-code-camp/bet-you-cant-solve-this-google-interview-question-4a6e5a4dc8ee

## Asynchronous
https://www.youtube.com/watch?v=eiC58R16hb8
- Javascript Engine: Heap, Call Stack
- Web API: fetch, setTimeout, URL, localstorage, sessionstorage, HTMLDivElement, document, IndexDB, XMLhttpRequest
- Event Loop
- Task Queue
- Microtask queue

<img width="2058" height="1852" alt="image" src="https://github.com/user-attachments/assets/8c0a9049-922d-44b7-930a-0bf7fe4c772a" />

# Objected Oriented Program 

### Class Basics
- A class is a **blueprint** for **creating objects** with **shared structure** and **behavior**. It's **syntactic sugar** over JavaScript’s **prototype-based inheritance**.
- When you need to **model real-world entities** with **properties** and **behaviors**, or **create multiple similar objects with shared methods**.
- A class is like a **blueprint for building cars**. Once you have the **blueprint**, you can create many cars (objects) from it.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

const dog = new Animal("Buddy");
dog.speak(); // Buddy makes a noise.
```

### Constructor
- The constructor is a **special method** that **runs automatically** when **a new instance** of the class is created using new.
- To initialize object properties at creation.
- The constructor is like the **assembly station in a factory** — it puts the **initial parts (properties)**  into your object.

```javascript
class User {
  constructor(username) {
    this.username = username;
  }
}

const user1 = new User("Alice");
console.log(user1.username); // "Alice"
```

### Methods
- **Functions** defined inside a class — accessible by class instances.
- To define **reusable behaviors** for the **class instances**.
- Methods are like **buttons on a machine** — press them to make the object do something.

```javascript
class Car {
  constructor(brand) {
    this.brand = brand;
  }

  honk() {
    console.log(`${this.brand} says Beep!`);
  }
}

const car = new Car("Toyota");
car.honk(); // "Toyota says Beep!"
```

### `this` Keyword
- Refers to the **current instance of the class**.
- Use this to access or update properties inside the class methods.
- `this` is like "me" from the **object’s perspective**.

```javascript
class Lamp {
  constructor(color) {
    this.color = color;
  }

  showColor() {
    console.log(`The lamp color is ${this.color}`);
  }
}
```

### Inheritance (extends and super)
- Classes can **inherit properties** and **methods** from another class using extends. The **super()** call refers to the **parent constructor**.
- To create **specialized versions** of a **generic class**.
- Inheritance is like **a child learning from a parent** — it gets everything the parent knows, and **can add or override some behavior**.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog("Rex");
dog.speak(); // Rex barks.
```

### Static Methods
- Methods that **belong to the class itself**, **not to its instances**.
- For utility or helper methods that don’t need instance-specific data.
- Static methods are like **manuals stored in the blueprint cabinet** — you don’t need to build an object to read them.

```javascript
class MathHelper {
  static square(x) {
    return x * x;
  }
}

console.log(MathHelper.square(5)); // 25
```

### Getters and Setters
- Special methods to get/set values using property-like syntax while running logic behind the scenes.
- When you want controlled access to private or computed properties.
- Like a smart thermostat — you "set" the temperature, but it adjusts things internally.

```javascript
class Circle {
  constructor(radius) {
    this._radius = radius;
  }

  get diameter() {
    return this._radius * 2;
  }

  set diameter(d) {
    this._radius = d / 2;
  }
}

const c = new Circle(5);
console.log(c.diameter); // 10
c.diameter = 20;
console.log(c.diameter); // 20
```

### Private Fields (#field)
- Using #, you can define truly private fields (not accessible outside the class).
- To encapsulate internal logic or sensitive data.
- Like a vault in a bank — only accessible through approved access points.

```javascript
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100
```

### Class Fields (Public & Private)
- You can now define fields directly inside the class (outside constructor) using modern JavaScript syntax.
- To define default values, simplify code, and avoid declaring everything in constructor.
- Fields are like default settings on a machine when it’s fresh out of the factory.

```javascript
class Counter {
  count = 0;      // public field
  #max = 100;     // private field

  increment() {
    if (this.count < this.#max) {
      this.count++;
    }
  }
}
```

### Inheritance Deep Dive (super + override)

- Child classes can inherit and override parent class methods. Use super.method() to call parent behavior.
- To extend logic, but still use part of the original method.
- Like adding toppings on a base pizza — you keep the foundation (super) and customize the rest.

```javascript
class Animal {
  speak() {
    console.log("Generic noise");
  }
}

class Cat extends Animal {
  speak() {
    super.speak(); // calls Animal.speak
    console.log("Meow");
  }
}
```

### Method Overriding
- A subclass redefines a method from the parent class.
- To customize or change inherited behavior.
- Like replacing an ingredient in a recipe — same dish, different flavor.

```javascript
class Shape {
  draw() {
    console.log("Drawing shape");
  }
}

class Circle extends Shape {
  draw() {
    console.log("Drawing a circle");
  }
}
```

### Instance Methods vs Static Methods
- Static is like a company rule; instance is like a personal behavior of an employee.

```javascript
class Utils {
  static isEven(n) {
    return n % 2 === 0;
  }

  describe() {
    return "This is an instance method.";
  }
}

Utils.isEven(4); // ✅ Static
const u = new Utils();
u.describe();    // ✅ Instance
```

### Mixins (Multiple Inheritance Workaround)
- JavaScript doesn't support multiple inheritance, but mixins let you share functionality between classes.
- To reuse logic across unrelated classes.
- Mixins are like plug-ins you attach to a machine to give it extra abilities.

```javascript
let CanEat = Base => class extends Base {
  eat() {
    console.log("Eating...");
  }
};

let CanWalk = Base => class extends Base {
  walk() {
    console.log("Walking...");
  }
};

class Person extends CanWalk(CanEat(Object)) {}

const p = new Person();
p.eat(); // "Eating..."
p.walk(); // "Walking..."
```

### Encapsulation with Closures (Alternative to Private Fields)
- Before #private, devs used closures to encapsulate private variables.
- When using older environments without private field support.
- Closures are like lockboxes — only the keys inside the box can open it.

```javascript
function SecretHolder(secret) {
  return {
    reveal() {
      return secret;
    }
  };
}

const holder = SecretHolder("TOP_SECRET");
console.log(holder.reveal()); // "TOP_SECRET"
```

### Best Practices
- Use class when modeling entities with identity/behavior
- Prefer constructor over init() for state setup
- Use #private or closures for hiding internals
- Avoid deep inheritance trees — use composition (mixins)
- Avoid this inside static methods


