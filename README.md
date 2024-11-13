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