# 

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
- Arrays are objects: 'typeof [1,2,3] === 'object''
- Functions are objects: 'typeof function(){} === 'function'' (but still an object)
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
