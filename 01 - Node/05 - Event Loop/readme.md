In Node.js, the event loop is a crucial concept that allows asynchronous, non-blocking I/O operations to be handled efficiently. The event loop is a core part of the Node.js runtime, and it enables the execution of code in a single-threaded, event-driven manner.

**Event Queue:** When you execute your Node.js program, it starts by processing the main module. Asynchronous operations, such as file I/O, network requests, or timers, are scheduled to run in the future. These operations are placed in the event queue.

**Event Loop:** The event loop is a continuously running process that checks the event queue for tasks that are ready to be executed. It picks up tasks one by one and runs them.

**Execution of Tasks:** Each task is executed in a non-blocking way. If a task involves I/O operations, the event loop moves on to the next task while waiting for the I/O operation to complete. This allows Node.js to handle many concurrent connections without the need for threading.

**Callbacks and Promises:** Callback functions are often used to handle the completion of asynchronous tasks. When an asynchronous task is completed, the associated callback function is pushed to the event queue, and the event loop executes it.
With the introduction of Promises and async/await in recent versions of Node.js, developers can also work with asynchronous code in a more synchronous style, making it easier to reason about and maintain.

Example
```
console.log('Start');

setTimeout(() => {
  console.log('Timeout callback');
}, 1000);

fs.readFile('example.txt', 'utf8', (err, data) => {
  console.log('File read callback');
});

console.log('End');
```

In this example, the console.log('Start') and console.log('End') statements will be executed first. The setTimeout and fs.readFile functions initiate asynchronous operations. While waiting for these operations to complete, the event loop continues to execute the remaining synchronous code. When the asynchronous operations are finished, their respective callback functions are pushed to the event queue and executed by the event loop.
