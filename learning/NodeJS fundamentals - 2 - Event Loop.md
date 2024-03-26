### Event Loop basics

JavaScript is single-threaded, meaning only one task can execute at a time. This can lead to issues, especially when performing time-consuming operations that block the main thread, resulting in an unresponsive user interface.

To mitigate this, browsers provide a Web API, including features like the DOM API and setTimeout, enabling asynchronous, non-blocking behavior.

### The Call Stack

When a function is invoked, it's added to the call stack—a fundamental component of the JavaScript engine. The call stack operates on a Last In, First Out (LIFO) principle, akin to a stack of pancakes. Once a function completes its execution, it's popped off the stack.

```
+------------------------+
|       Call Stack       |
|------------------------|
|                        |
|                        |
|        functionA()     |
|        functionB()     |
|        functionC()     |
|        main()          |
+------------------------+
```


### Understanding setTimeout and the Timer module

Consider the following example:

```javascript
function respond() {
    return setTimeout(() => { return 'Hey' }, 1000);
}

const timer = respond();
```

Here, the `respond` function returns a `setTimeout` function. The `setTimeout` function is part of the Web API (timer module in NodeJS), allowing the delay of tasks without blocking the main thread. The callback function passed to `setTimeout` is scheduled to execute after 1000 milliseconds.

### The Callback Queue

After the timeout period elapses, the callback function isn't immediately added to the call stack. Instead, it's placed in the callback queue, awaiting its turn for execution.

### Event Loop in Action

When the call stack is empty, indicating that all previously invoked functions have completed, the event loop kicks in. It fetches tasks from the callback queue and adds them to the call stack for execution.

```
Initial State:

     +------------------------+
     |       Call Stack       |
     +------------------------+
                |
                v
     +------------------------+
     |     Global Context     |
     +------------------------+

Execution:

     +------------------------+
     |       Call Stack       |
     |------------------------|
     |       respond()        |
     |     setTimeout()       |
     +------------------------+
                |
                v
     +------------------------+
     |     Timers Module      |
     |------------------------|
     |     setTimeout()       |
     +------------------------+

After setTimeout() is called:

     +------------------------+
     |       Call Stack       |
     +------------------------+
                |
                v
     +------------------------+
     |       respond()        |
     +------------------------+

     +------------------------+
     |       Timer Queue      |
     +------------------------+
                |
                v
     +------------------------+
     |   setTimeout callback  |
     +------------------------+

Event Loop adds setTimeout callback to the Call Stack after 1000ms:

     +------------------------+
     |       Call Stack       |
     |------------------------|
     |  setTimeout callback   | ==> Hey
     +------------------------+
                |
                v
     +------------------------+
     |       Call Stack       |
     +------------------------+                

```

### Example Execution

Let's examine the following code snippet:

```javascript
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"), 500);
const baz = () => console.log("Third");

bar();
foo();
baz();
```

1. We invoke `bar`, which returns a `setTimeout` function. `bar` and `setTimeout` are added to the call stack and then popped off.
2. Meanwhile, `foo` is invoked and logs "First" to the console. `foo` is then popped off.
3. Next, `baz` is invoked, logging "Third" to the console.
4. As the event loop detects an empty call stack after `baz` returns, it adds the callback from `setTimeout` to the call stack.
5. Finally, "Second" is logged to the console.

Visualization: 
```
Initial State:

     +------------------------+
     |       Call Stack       |
     +------------------------+
                |
                v
     +------------------------+
     |     Global Context     |
     +------------------------+

Execution:

After bar() is called and setTimeout() is scheduled:

     +------------------------+
     |       Call Stack       |
     |------------------------|
     |          bar()         |
     +------------------------+
                |
                v
     +------------------------+
     |       Timer Queue      |
     |------------------------|
     |    setTimeout() (500ms)|
     +------------------------+

After foo() is called:

     +------------------------+
     |       Call Stack       |
     +------------------------+
                |
                v
     +------------------------+
     |          foo()         | ==> Prints First, foo() removed
     +------------------------+

     +------------------------+
     |       Callback Queue   |
     +------------------------+
                |
                v
     +------------------------+
     |  setTimeout callback   |
     +------------------------+

After baz() is called:

     +------------------------+
     |       Call Stack       |
     +------------------------+
                |
                v
     +------------------------+
     |          baz()         | ==> Prints Third, baz() removed
     +------------------------+

After 500ms

     +------------------------+
     |       Call Stack       |
     |------------------------|
     |    setTimeout callback | ==> Prints Second, setTimeout callback removed
     +------------------------+

```

Explanation:
- Initially, `bar()` is called, which schedules a `setTimeout()` function to execute after 500 milliseconds.
- Then, `foo()` is called.
- After `foo()`, `baz()` is called.
- Eventually, after the specified time interval (500ms), the callback function from `setTimeout()` is executed.
- The order of console output will be "First", "Third", and "Second", due to the asynchronous nature of `setTimeout()`.

### How timer module works in NodeJS? 

In Node.js or web browsers, the Timer Module is responsible for handling asynchronous timing operations, such as `setTimeout()` and `setInterval()`. Overall, the Timer Module allows JavaScript to handle time-dependent operations asynchronously, ensuring that your code can perform tasks at specific intervals without blocking the execution of other code. Here's how the Timer Module typically works:

1. **setTimeout() Function:**
   - When you call `setTimeout(callback, delay)`, the Timer Module schedules the `callback` function to be executed after the specified `delay` milliseconds.
   - It sets up a timer internally and continues executing other code.
   - After the specified delay, the Timer Module places the callback function in the Task Queue (or sometimes referred to as the Timer Queue), ready for execution.

2. **setInterval() Function:**
   - Similarly, `setInterval(callback, delay)` schedules the `callback` function to be repeatedly executed at intervals specified by `delay`.
   - It works similar to `setTimeout`, but instead of executing the callback once after a delay, it repeatedly schedules the callback at specified intervals.

3. **Event Loop:**
   - Meanwhile, the main thread continues executing other synchronous tasks.
   - When the call stack is empty and there are tasks waiting in the Task Queue (such as timer callbacks), the Event Loop moves these tasks from the Task Queue to the Call Stack for execution.
   - This ensures that asynchronous tasks, like callbacks from `setTimeout()` or `setInterval()`, are executed when their respective delays expire without blocking the main thread.

4. **Execution of Callbacks:**
   - Once a callback is moved from the Task Queue to the Call Stack, it is executed in the main thread.
   - If the callback is a long-running task, it might delay the execution of subsequent callbacks in the queue.

5. **Clearing Timers:**
   - You can cancel a timer using `clearTimeout()` for `setTimeout()` or `clearInterval()` for `setInterval()`. This removes the scheduled task from the Timer Module's queue, preventing it from executing.


### References
- https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif 

### tags:
#NodeJS
#NodeJSEventLoop
#EventLoop