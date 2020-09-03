---
title: What is Node JS?
description: Some insights into Node JS
date: "2020-09-03T23:46:37.121Z"
---

## What is Node JS?

* It is an event-driven JavaScript runtime built on Chrome's **V8** JavaScript Engine.

* It is **non-blocking** and **asynchronous**.

* It is **single-threaded**.

### What is V8 Engine?

> V8 is Google’s open source high-performance JavaScript and WebAssembly engine, written in C++. It is used in Chrome and in Node.js, among others. It implements ECMAScript and WebAssembly, and runs on Windows 7 or later, macOS 10.12+, and Linux systems that use x64, IA-32, ARM, or MIPS processors. V8 can run standalone, or can be embedded into any C++ application.

Source: [V8](https://v8.dev/)

### What does event-driven mean?

> event-driven programming is a programming paradigm in which the flow of the program is determined by events such as user actions (mouse clicks, key presses), sensor outputs, or messages from other programs/threads.

Source: [Wikipedia](https://en.wikipedia.org/wiki/Event-driven_programming)

Sounds about right. To add, the event-driven architecture in Node JS is built upon the `Publish-Subscribe (pub-sub)` or `Observer` pattern. Therefore, you will see a lot of these in the Node JS API.

```javascript
worker.on('listening', (address) => {
  worker.send('shutdown');
  worker.disconnect();
  timeout = setTimeout(() => {
    worker.kill();
  }, 2000);
});

worker.on('disconnect', () => {
  clearTimeout(timeout);
});
```
This example above is taken from the `Cluster` [documentation](https://nodejs.org/dist/latest-v12.x/docs/api/cluster.html).

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

myEmitter.on('event', () => {
  console.log('an event occurred!');
});
```
This example is taken from the `Events` [documentation](https://nodejs.org/dist/latest-v12.x/docs/api/events.html).

### How does event-driven work?
> In an event-driven application, there is generally a main loop that listens for events, and then triggers a callback function when one of those events is detected.

Source: [Wikepedia](https://en.wikipedia.org/wiki/Event-driven_programming)

### How is Node JS non-blocking and asynchronous?

#### Synchronous Operation
In a normal synchronous world, a block of code is typically executed in sequence. It follows that each line is a blocking operation. Ordinarily, if the JavaScript code does not call any external  **API** or perform any **I/O** operation, that's fine.

However, if a line of code makes an **I/O** (reading or writing to disk, network calls) call, which takes a long time to complete, then the rest of the JavaScript code will not run. In this scenario, the `event loop` is **blocked**. The implication is **poor** performance.

#### Asynchronous Operation
In a normal asynchronous world, we can convert that synchronous line of code that makes the **I/O** call to an asynchronous one. This ensures that the **event loop** is not blocked and the rest of the Javascript code can be executed.

Therefore, once the **I/O** operation is complete, the **event loop** will pick up the result callback from the **message** queue and execute it as soon as the current execution context is freed up. The implication is **better** performance because we free up the time taken to wait for the **I/O** operation to complete and use that time to execute the other lines of JavaScript code, thereby achieving parallelism.

#### Sync and Async APIs
Node JS provides both `sync` and `async` APIs which you can use based on your use case. And one such example is the following:

```javascript
const fs = require('fs');

// synchronous operation
const data = fs.readFileSync('/file.md'); // blocks here until file is read

// asynchronous operation
fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
});
```

To be clear that you understand the difference between synchronous and asynchronous code execution, let's expand on the above example:

```javascript
// synchronous operation
const data = fs.readFileSync('/file.md'); // blocks here until file is read
console.log(data);
moreWork(); // will run after console.log

// asynchronous operation
fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
  console.log(data);
});
moreWork(); // will run before console.log
```
### Node JS is single threaded

To be exact and precise, JavaScript code execution is **single threaded** (worker threads have recently been introduced).

There are several background threads that run in the background to handle **I/O** or **process** operations, which allows for JavaScript code execution to be asynchronous.

Imagine if **V8** has only one thread, it would be impossible for **V8** to achieve **non-blocking** or **asynchronous** operations.

#### Webworkers

With the addition of Webworkers, is it still true that Node JS is no longer **single-threaded**?

The short answer is yes and no.

Even though Webworkers allow you to create multiple threads, the use case is mainly to enable users to execute computationally heavy JavaScript code in parallel. For **I/O** operations, the recommended way is still the usage of asynchronous APIs.

For more information, we will cover that in another blog post.

### Conclusion

Node JS is fast. And it excels for applications that expects high concurrent connections. Don't take my word for it though - Tinker with it, try it out for yourself.

Personally, what I like about using Node JS is how easy it is to get it up and  running because you don't need **1000** LOC just to get started.

Have fun playing with it!