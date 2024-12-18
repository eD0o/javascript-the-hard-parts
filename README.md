# 4 - Understanding Asynchronous JavaScript

## 4.1 - Function Definitions and Calling Functions

We begin by defining two functions: printHello and blockFor1Sec. Here's the initial setup:

```javascript
function printHello() {
  console.log("Hello");
}

function blockFor1Sec() {
  // Simulating a delay with a loop (not using setTimeout here)
  for (let i = 0; i < 1e9; i++) {} // Blocking operation
}
```

At this point, we're setting up the basic functions that we'll use for our demonstration. We haven't yet introduced any asynchronous behavior.

## 4.2 - Understanding setTimeout

Next, we introduce setTimeout to simulate asynchronous behavior. The key concept here is that `setTimeout doesn't actually execute the code immediately, but schedules it to be run after a certain time period.

> Obs: Even if the delay is 0 milliseconds.

```javascript
setTimeout(printHello, 0);
blockFor1Sec();
```

What happens at this point?

- setTimeout schedules printHello to run after 0 milliseconds.
- Although setTimeout schedules printHello to run asynchronously, it won't execute immediately because blockFor1Sec runs synchronously, blocking the call stack until it completes.

Execution Flow

- `printHello is passed to setTimeout, but it isn't immediately executed. It's placed into a callback queue.
- The synchronous code, like `blockFor1Sec, continues running, blocking the execution of other asynchronous code until it finishes.

## 4.3 - Callback Queue and Event Loop

The callback queue `holds functions that are scheduled to run asynchronously. These functions are executed only when the call stack is empty, `ensuring that synchronous code takes priority.

### 4.3.1 - Event Loop

The event loop constantly `checks if the call stack is empty. If it is, it moves functions from the callback queue onto the call stack for execution. This process ensures that all synchronous code is executed first.

```javascript
// Event Loop Concept (simplified)
while (true) {
  if (callStackIsEmpty()) {
    moveFromQueueToCallStack();
  }
}
```

Execution Flow:

1. Calling blockFor1Sec: The function blockFor1Sec runs on the call stack and blocks further execution until it finishes. At this point, nothing from the callback queue (like printHello) can run because the call stack is still occupied.

2. Event Loop Check: While blockFor1Sec is running, the event loop checks if the call stack is empty. Since it's not empty (because blockFor1Sec is still running), the event loop doesn't move printHello to the call stack.

3. Completion of blockFor1Sec: Once blockFor1Sec finishes, the event loop checks the callback queue. Since printHello has been sitting in the queue, it can now be moved to the call stack and executed.

4. Executing printHello: At this point, printHello is executed, and the message "Hello" is logged to the console.

> Functions in the callback queue are executed only after all synchronous code has finished running. This is why printHello isn't executed immediately, even though it was scheduled with setTimeout(0).

```javascript
function printHello() {
  console.log("Hello");
}

function blockFor1Sec() {
  for (let i = 0; i < 1e9; i++) {} // Blocking operation
}

setTimeout(printHello, 0); // Schedules printHello for execution
blockFor1Sec(); // Blocks further execution
```

This demonstrates how the callback queue works in conjunction with the event loop to manage asynchronous code.

## 4.4 - Timeout and Cancellation

In JavaScript, `asynchronous operations can sometimes need to be canceled before they complete. This is especially useful when `handling timeouts or requests that might no longer be needed` due to user interaction or state changes.

### 4.4.1 - Using setTimeout and clearTimeout

setTimeout allows you to schedule a function to be executed after a specified time. However, `you can cancel it using clearTimeout` before the time expires.

```js
const timeoutID = setTimeout(() => {
  console.log("This will not be logged if cleared");
}, 5000);

// Clear the timeout before it runs
clearTimeout(timeoutID);
```

In this example, the timeout is canceled before it has a chance to execute.

### 4.4.2 - Aborting Fetch Requests with AbortController

Sometimes, network requests like fetch need to be canceled. This can be done using the AbortController API, which `allows you to signal cancellation of an asynchronous task.

```js
const controller = new AbortController();
const signal = controller.signal;

fetch("https://api.example.com", { signal })
  .then((response) => response.json())
  .catch((error) => console.error("Request was aborted:", error));

// Abort the fetch request
controller.abort();
```

`Even when canceling asynchronous tasks, the event loop remains essential. If a request is aborted, the event loop will continue to check if tasks need to be executed. If the canceled task had been in the callback queue, it would be ignored.

## 4.5 - Introducing async and await`

Unlike setTimeout, which relies on the callback queue and event loop, async and await provide a more modern and cleaner way of working with asynchronous code. They `allow us to write asynchronous code that looks synchronous, improving readability and reducing the need for callbacks.

### 4.5.1 - async Function

An `async function always returns a Promise. Even if you return a non-Promise value from an async function, `JavaScript will automatically wrap it in a resolved Promise.

```javascript
async function greet() {
  return "Hello"; // This is equivalent to returning a resolved Promise with "Hello"
}

greet().then(console.log); // "Hello"
```

### 4.5.2 - await Expression

The await keyword, used within an async function, `pauses the function's execution until the Promise resolves or rejects. However, `unlike synchronous code, it does not block the main thread or event loop.

```javascript
async function delayedGreeting() {
  console.log("Before delay");

  // Pauses here without blocking the event loop
  await new Promise((resolve) => setTimeout(resolve, 1000));

  console.log("After delay");
}

delayedGreeting();
```

In this example, await is used to wait for the Promise returned by setTimeout. Even though the code looks synchronous, it doesn't block the event loop. `The execution of the function will pause at await and continue after the delay without blocking other code from running`.

### 4.5.3 - Event Loop and async / await`

Although `async and await allow us to write asynchronous code that appears synchronous, they still use the event loop behind the scenes`. Here's how:

1. Calling an async function: `When an async function is called, it returns a Promise`.
2. Using await: The `execution of the async function is paused at each await expression`. This pause happens while the Promise is waiting to resolve, but it doesn't block the event loop.
3. Event Loop Behavior: While waiting for the Promise to resolve, `the event loop continues to process other synchronous or asynchronous tasks`. Once the Promise resolves, the function continues executing from where it left off.

Example: async/await with Blocking Operation

```javascript
async function greet() {
  console.log("Before delay");

  await new Promise((resolve) => setTimeout(resolve, 1000)); // Pauses here for 1 second

  console.log("After delay");
}

async function blockFor1Sec() {
  console.log("Start blocking");
  for (let i = 0; i < 1e9; i++) {} // Blocking operation
  console.log("End blocking");
}

async function main() {
  greet(); // This will log "Before delay", wait for 1 second, then log "After delay"
  blockFor1Sec(); // This blocks execution for 1 second
}

main();
```

In this scenario:

1. greet is called first. It logs "Before delay", then waits for 1 second without blocking the event loop (thanks to await).
2. blockFor1Sec runs next. It blocks the execution thread synchronously, so the event loop can't continue with any other tasks until it finishes.
3. While blockFor1Sec runs synchronously, greet is paused at the await until the Promise resolves.

## 4.6 - Conclusion: async and await vs setTimeout`

While setTimeout schedules tasks to run asynchronously via the event loop, async and await provide a more intuitive way of handling asynchronous code. The key differences are:

- `setTimeout works through the callback queue`, while `async and await work directly with Promise objects`.
- async and await allow for writing asynchronous code in a way that looks and behaves like synchronous code, without blocking the event loop.

`Both methods still rely on the event loop, but async/await provide a cleaner syntax` for handling promises and asynchronous flows.
