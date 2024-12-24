# 5 - Promises

## 5.1 - Introduction

Before ES6, JavaScript lacked a way to track background processes like `setTimeout. It didn't provide a direct connection between what's happening in the browser (background work) and JavaScript's memory`. This made it `challenging to maintain consistency between the two`.

`Promises were introduced in ES6 to address this gap`. The core idea was to allow background processes (like setTimeout) to `have consequences in JavaScript memory, making it easier to track and manage the state` in both the background and JavaScript.

### 5.1.1 - The Two-Pronged Facade Function

A new concept in ES6: two-pronged facade functions.

- One prong `interacts with the web browser` (e.g., making network requests).
- The other prong `returns a Promise object in JavaScript that tracks` the state of the background process.

```js
// Example: The fetch function in JavaScript

fetch("https://api.example.com/data") // This speaks to the internet, sending the network request
  .then((response) => response.json()) // When the response is received, convert it to JSON
  .then((data) => console.log(data)) // Once data is ready, log it
  .catch((error) => console.log(error)); // Catch any error if it occurs
```

In this example:

- The fetch function sends a request to fetch data from the internet (`prong one: interacting with the browser`).
- It returns a Promise object that can be used to handle the result in JavaScript when it's available (`prong two: reflecting the background operation in memory`).

The fetch function is a modern way to interact with the web browser to send network requests (e.g., to fetch data from the internet).

- It sets up the request in the background (in the web browser).
- It `simultaneously returns a Promise object in JavaScript, which will be filled with the response once the background work is complete`.

## 5.2 - Promises Example: fetch

```js
// Declares a function display and stores it in global memory.
function display(data) {
  console.log(data);
}

// Declares a constant futureData (uninitialized at this stage). The fetch call triggers a two-pronged consequence:
const futureData = fetch("https://twitter.com/will/tweets/1");
/**
 * 1. Sets up background work in the web browser.
   2. Creates a Promise object in JavaScript with:
    - value property (initially undefined).
    - onFulfilled property (initially an empty array).
*/

futureData.then(display); // This Promise object is stored in futureData and acts as a placeholder for data when the background task completes.

console.log("Me first!");
```

The Web Browser Work: The `fetch call initiates a network request to the specified URL`. It requires:

- The domain name (e.g., twitter.com) to identify the server.
- The path to locate the specific data on the server.

> By default, fetch uses the HTTP GET method to retrieve data.

On Completion: Once the data is retrieved:

1. `It is stored in the value property of the Promise object` (futureData.value).
2. The Promise object's connection to the background task `ensures synchronization between the web browser and JavaScript` memory.

This powerful mechanism allows JavaScript to handle asynchronous operations seamlessly while maintaining a clear structure for managing results and errors.
