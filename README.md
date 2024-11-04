# 2 - Functions & Callbacks

## 2.1 - Generalized Functions

1. **Purpose of Functions**:

   - Functions `encapsulate code to allow for easy reuse and maintain DRY` (Don't Repeat Yourself) principles.
   - Rewriting similar functions, like tenSquared, nineSquared, etc., demonstrates repetitive code that's hard to maintain.

```js
function tenSquared() {
  return 10 * 10;
}
tenSquared(); // 100

function nineSquared() {
  return 9 * 9;
}
nineSquared(); // 81
```

2. **Generalization with Parameters**:

   - Instead of hard-coding values, `we can make functions flexible by using parameters`. This allows us to specify values only when we call the function, making the code reusable.

```js
function squareNum(num) {
  return num * num;
}
squareNum(10); // 100
squareNum(9); // 81
squareNum(8); // 64
```

## 2.2 - Higher-Order Functions

A higher-order function is `a function that either takes one or more functions as arguments or returns a function as its result`.

It allows for flexibility by enabling certain parts of functionality to be defined only at runtime. This approach enhances reusability and adaptability, making code more modular and easier to maintain.

```ts
// higher-order function that accepts a function to customize behavior
function applyOperation(num, operation) {
  return operation(num);
}

// callback function to pass as argument
function square(x) {
  return x * x;
}

// callback function to pass as argument
function cube(x) {
  return x * x * x;
}

// usage
console.log(applyOperation(5, square)); // 25
console.log(applyOperation(3, cube)); // 27
```

- applyOperation is a higher-order function because it `takes another function (operation) as an argument`.

> The specific operation (e.g., squaring or cubing) is decided only at runtime when applyOperation is called, making the function highly reusable and adaptable.

`Functions are first class objects`, meaning they can be treated like any other value.

Therefore, they can be:

1. Assigned to variables and properties of other objects
2. Passed as arguments to functions
3. Returned as values from other functions

## 2.3 - Callback Functions

A callback function is `a function passed as an argument to another function`, where it can be invoked later as part of that function's execution.

This mechanism enables greater flexibility in programming by allowing specific tasks or operations to be defined outside the main function logic.

Key Features of Callback Functions:

- Control Sequence of Operations: Callbacks help manage the flow of execution, `allowing certain tasks to run after others are completed`. This is particularly useful in asynchronous programming, where operations may take varying amounts of time to complete.

- Inject Custom Functionality: Callbacks enable developers to pass their own functions as arguments, `allowing for customization of behavior without altering the original function's code`.

Example:

```js
// Higher-order function that takes a callback to execute after validation
function validateUser(user, callback) {
  if (user.isLoggedIn) {
    callback(user.name); // Execute the callback if the user is logged in
  } else {
    console.log("User is not logged in.");
  }
}

// Callback function to greet the user
function greet(name) {
  console.log(`Hello, ${name}!`);
}

// Usage
const user = { name: "Alice", isLoggedIn: true };
validateUser(user, greet); // Output: Hello, Alice!

const user2 = { name: "Bob", isLoggedIn: false };
validateUser(user2, greet); // Output: User is not logged in.
```

## 2.4 - Anonymous and Arrow functions

## 2.4.1 - Anonymous Functions

Anonymous functions are `functions without a name`, often used as one-off functions, particularly in callbacks.

```JS
const arr = [1, 2, 3];
arr.forEach(function (num) {
  console.log(num * 2); // Outputs: 2, 4, 6
});
```

- No specific this binding: `this depends on how the function is called`.
- Can be reused by assigning to a variable.

## 2.4.2 - Arrow Functions

Arrow functions are `a shorter syntax for anonymous functions`, introduced in ES6. They `have lexical (inherited) this and support implicit returns`.

Syntax:

```JS
(param1, param2) => expression
// OR
(param) => { /* code */ }
```

Example:

```JS
const doubled = arr.map((num) => num * 2); // [2, 4, 6]
```

- Lexical this: `Inherits this from the outer function`.
- No prototype or arguments: `Cannot be used as constructors`.

## Key Differences

| Feature    | Anonymous Function          | Arrow Function          |
| ---------- | --------------------------- | ----------------------- |
| **`this`** | Depends on call site        | Lexically inherited     |
| **Syntax** | `function() { /* code */ }` | `(param) => expression` |

`this` Example:

```JS
function Timer() {
  this.time = 0;
  setInterval(() => {
    this.time++; // Refers to `Timer` instance due to lexical `this`
  }, 1000);
}
```

When to Use Arrow Functions: 

- For `concise syntax in inline callbacks`.
- When you need `this from the outer function`.

Arrow functions `simplify syntax and maintain consistent this binding, ideal for short functions` in methods like map, filter, and event handlers.
