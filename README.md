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

## 1.3 - Call Stack

The call stack in JavaScript is a mechanism for `managing function execution and tracking the program’s current position`.
It works in a `"Last In, First Out"` (LIFO) manner to keep track of active functions.

The call stack helps JavaScript `keep track of "where it is" in the code, ensuring functions execute in the proper order`.

Key Points:

1. **Global Context**: The global execution context is added to the bottom of the call stack when JavaScript starts running.

2. **Adding Functions**: Each time `a function is invoked, it’s added to the top of the call stack`. For example, calling multiplyBy2 with num (3) pushes the function with inputNumber = 3 onto the stack.

3. **Executing and Removing**: `JavaScript executes the function and removes it from the stack upon hitting a return statement`, resuming execution at the previous context.

4. **Nested Calls**: `When a function calls another function, the new function is added to the top of the stack`, and JavaScript only focuses on the topmost function.

5. **Returning to Global**: When all `functions are complete, the stack returns to the global context`.

```js
// 1. Global Context: JavaScript starts in the global context and adds it to the call stack.

console.log("Program Start"); // Step A: This is executed in the global context.

function multiplyBy2(inputNumber) {
  // Step B: multiplyBy2 is called and added to the call stack.
  return inputNumber * 2; // Executes and returns a value, then is removed from the stack.
}

function square(num) {
  // Step C: square is called and added to the call stack.
  return multiplyBy2(num) * num; // Calls multiplyBy2, creating a nested call.
}

function printResult() {
  // Step D: printResult is called and added to the call stack.
  const result = square(3); // Calls square, which calls multiplyBy2.
  console.log("Result:", result); // Logs the final result, then is removed from the stack.
}

printResult(); // Step D: Begins the process by calling printResult.
console.log("Program End"); // Step E: This runs after all other calls finish.
```
