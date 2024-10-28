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
