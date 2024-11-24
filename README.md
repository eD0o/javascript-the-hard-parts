# 3 - Closure

## 3.1 - Introduction

### 3.1.1 - Lexical Scoping & Execution Context

A fundamental concept in JavaScript that involves `lexical scoping and execution context`.

Lexical Scoping: Refers to `how a function remembers the variables that are in scope` at the time of its creation, not just when it is called.

Execution Context: When a `function is invoked, a new execution context is created, which includes local memory` (or variable environment). Once `the function finishes execution, this context is deleted`, except for the returned value.

```js
// Lexical Scoping and Execution Context
function outerFunction() {
  let outerVariable = "I am from outer function!"; // outerVariable is in the lexical scope of outerFunction

  function innerFunction() {
    console.log(outerVariable); // innerFunction can access outerVariable due to lexical scoping
  }

  innerFunction(); // Calling innerFunction
}

outerFunction(); // Calling outerFunction
```

Explanation:

1 - Lexical Scoping:

- innerFunction has access to outerVariable because of lexical scoping, which means it remembers the variables available in the scope where it was created (i.e., inside outerFunction), not just when it is executed.

2 - Execution Context:

- When outerFunction is called, a new execution context is created with its own memory (local variables). Inside outerFunction, the innerFunction has access to the execution context of its parent function due to lexical scoping.

### 3.1.2 - Use Cases and Benefits

Memoization: A performance optimization `technique where functions "remember"` previous results to avoid redundant calculations, making code more efficient.

Design Patterns: Many JavaScript design patterns (like the module pattern, iterators, partial application, and currying) rely on closures for `maintaining state and managing asynchronous tasks`.

State in Functions: `Closure allows a function to have a persistent memory, meaning it can "remember"` its previous execution states and influence its future behavior.

## 3.2 - Returning Functions

- When a function is called, a live store of data (local memory or variable environment) is created for that specific execution.
- After the function finishes execution, its local memory is typically deleted, except for the returned value.

Persistent Data Between Executions:

- By returning a function from another function, we can enable a function to `hold onto live data even after its execution context is destroyed`.

- Returning a function allows us to create "functions with memories" by preserving references to their parent function's local memory.
  > This is foundational for **closures** in JavaScript.

```js
function createFunction() {
  function multiplyBy2(num) {
    return num * 2;
  }
  return multiplyBy2;
}
const generatedFunc = createFunction(); // `multiplyBy2` is returned and assigned to `generatedFunc`.
const result = generatedFunc(3); // 6

// The `generatedFunc` is the `multiplyBy2` function returned from `createFunction`.
// It retains access to variables and scope of `createFunction` at the time it was created, thanks to closures.
```

> generatedFunc is the result of a single execution of createFunction. While it no longer depends on createFunction being called again, it `retains access to the variables and scope of createFunction at the time it was created, thanks to closures`.

Another Example:

```js
function createCounter() {
  let count = 0; // Persistent state stored in the closure
  return function increment() {
    count += 1; // Accesses the `count` variable in its parent's scope
    return count;
  };
}

const counter = createCounter(); // Executes `createCounter` and returns `increment`
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3

const anotherCounter = createCounter(); // Creates a new, independent closure
console.log(anotherCounter()); // 1
console.log(anotherCounter()); // 2
console.log(counter()); // 4
```

Why Use This Pattern?

1. Persistent Memory:

The `returned function can "remember" the environment it was created in, enabling it to access variables` from its parent function's scope.

2. Dynamic Function Creation:

`Functions can be dynamically generated with specific behaviors` based on the logic in the parent function.

3. Cache and State Management:

Useful for implementing `caching, memoization, or encapsulating private state` within a function.

4. Encapsulation of Private Variables:

This pattern allows for the `creation of private variables that are accessible only to the returned function`, providing better modularity and data protection.

## 3.3 - Nested Function scope

This example introduces the `mechanics of scope and variable` lookup in nested functions, setting the stage for exploring closures, where a function retains access to variables in its defining scope even after that scope has closed.

```js
function outer() {
  let counter = 0;
  function incrementCounter() {
    counter++;
  }
  incrementCounter();
}
outer();
```

![](https://i.imgur.com/M3ob82J.jpeg)

1.  Function Execution Context:

    - When outer() is called, a new execution context is created, and the variable counter is initialized to 0 in outer's local memory.
    - The function incrementCounter is defined and stored in outer's local memory.

2.  Nested Function Execution:

    - incrementCounter() is executed immediately within outer().
    - During execution, incrementCounter looks for the variable counter:

      - It first checks its own local memory (incrementCounter's own scope).
      - Failing to find counter there, it steps out to the memory of outer (the function where it was defined).

    - counter++ increments the value stored in outer's scope, showing how inner functions can access and modify variables in their defining scope.

3.  Call Stack and Scope Chain:

    - JavaScript uses a call stack to manage function calls and a scope chain to resolve variable references.

      1. Global context (outer is defined).
      2. outer execution context (creates counter and defines incrementCounter).
      3. incrementCounter execution context (runs its code).

      After each function call finishes, its execution context is removed from the stack.

4.  Conceptual Question: `Why does incrementCounter access counter`?

Is it because:

1. It was defined in outer (closure)?
2. Or because it was executed within outer?

> The answer is closures: the defining context of a function determines what variables it can access.

When `incrementCounter is defined, it captures a reference to its lexical environment` (the scope where it was created).

This `lexical environment includes counter, even if incrementCounter is executed later` or in a different context.

## 3.4 - Retaining Function Memory

Consider the following code:

```js
function outer() {
  let counter = 0; // Local variable
  function incrementCounter() {
    counter++; // Increment the counter
  }
  return incrementCounter; // Return the inner function
}

const myNewFunction = outer(); // Invoke `outer` and assign the result
myNewFunction(); // Increment the counter - output: 1
myNewFunction(); // Increment the counter again - output: 2
```

Closure:

When incrementCounter is returned, it doesn't just include the function code.
It carries a `closure, a "backpack" containing the surrounding local variables (counter)` from its creation environment.

Retaining State:

Even though the outer execution context is removed, the `closure allows the returned function (myNewFunction) to retain access to counter`.
`Each call to myNewFunction increments the counter` variable preserved in the closure.

> The counter variable persists in memory as part of the function's hidden [[Scope]]. It is private and can't be accessed directly, e.g., myNewFunction.counter or myNewFunction.scope.counter won't work.

Lexical Scope:

JavaScript `functions "remember" the scope they were defined in`.
When myNewFunction is invoked, it `looks for counter in the closure before looking at global` memory.

You can create a callback function that accesses the private data inside the closure. Here's an example:

```js
function outer() {
  let counter = 0; // Local variable

  function incrementCounter() {
    counter++; // Increment the counter
    return counter; // Return the updated counter
  }

  // A callback function that accesses the counter
  function getCounterValue() {
    return counter; // Access the current counter value
  }

  return { incrementCounter, getCounterValue }; // Expose both functions
}

const myFunctions = outer(); // Create the closure

// Increment the counter
console.log(myFunctions.incrementCounter()); // Output: 1
console.log(myFunctions.incrementCounter()); // Output: 2

// Use the callback to access the counter
console.log(myFunctions.getCounterValue()); // Output: 2
```

If a new variable is assigned to a function (same closure for multiple labels), it retains the same "backpack" (closure).

```js
function outer() {
  let counter = 0;

  function incrementCounter() {
    counter++;
    return counter;
  }

  return incrementCounter;
}

const myNewFunction = outer();
const mySecondNewFunction = myNewFunction; // Assigning a new label to the function
console.log(myNewFunction()); // Output: 1
console.log(mySecondNewFunction()); // Output: 2
console.log(myNewFunction()); // Output: 3
```

## 3.5 - Closure Techincal Definition

- **Persistent Lexical Static Scope Reference Data**:
  - A formal term for the data carried in the "backpack".
  - Emphasizes how data is preserved and linked to the function through lexical scoping.
- **Closure (umbrella term)**:
  - Refers to the broader concept of a function "capturing" its surrounding lexical environment.
  - Often used interchangeably with "backpack" but includes both the behavior and the data.
- **Variable Environment (Closed-Over Variable Environment)**:
  - Refers to the actual set of variables stored in the closure.
  - Represents the state of variables at the time the function is created and is carried with the function.

Take notes of the exercises link of this module and the 02 as well