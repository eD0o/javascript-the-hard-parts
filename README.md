# 1 - Principles

## 1.1 - Thread Execution

- JavaScript `processes code line-by-line`, a concept known as the "thread of execution."

  - Each line of code is executed in sequence, enabling JavaScript to perform actions as it reads through the code.

```js
console.log("Start"); // 1 - Executes first
const num = 2; // 2 - Defines `num` with value 2
console.log(num); // 3 - Logs 2
```

Memory

- JavaScript stores data (like variables, constants, and even functions) in memory, allowing us to reference and reuse them later.
  - Memory is the area where values and code snippets are saved, providing access to those values as the code executes.

```js
const name = "Alice"; // Stored in memory with label `name`
function greet() {
  return `Hello, ${name}!`; // Refers to `name` in memory
}
console.log(greet()); // Outputs "Hello, Alice!"
```

## 1.2 - Functions

A function being run is like a mini program.

In JavaScript, an `execution context is created to run code, particularly within functions`. Each execution context has two parts:

When a `function is invoked, a new execution context is created, separate from the global execution context that runs the overall file of code`. Inside a function, parameters act as placeholders for values passed in as arguments. When a function is called, JavaScript:

- Assigns arguments to parameters.
- Executes each line within the function.
- Returns the result to the global context if specified.

> Only one thread of execution exists, so JavaScript processes one line at a time, creating new contexts as functions are invoked and returning to the global context after execution.

```js
// Function to create a user profile with full name
function createUserProfile(firstName, lastName) {
  // Local variable to store the full name
  const fullName = `${firstName} ${lastName}`;

  // Return the full name as an object
  return { fullName };
}

// Global variables that hold two different user profiles
const user1 = createUserProfile("Jane", "Doe"); // First invocation
const user2 = createUserProfile("John", "Smith"); // Second invocation

// Displaying the full names of both users
console.log(user1.fullName); // Output: "Jane Doe"
console.log(user2.fullName); // Output: "John Smith"
```

- `Each function call creates a new execution context with its own memory space`, so firstName, lastName, and fullName do not interfere between calls.
- The global variables user1 and user2 `store the returned results from each function invocation separately`.
- `Each invocation is isolated`, so modifying one user profile `does not affect the other`.
