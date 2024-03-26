## Execution context and callstack at JavaScript

### What is execution context in Javascript or V8?

In JavaScript, an execution context is an abstract concept that refers to the environment in which JavaScript code is evaluated and executed. It consists of three main components:

1. **Global Execution Context**: This is the default or outermost execution context in which JavaScript code starts running. It includes any code that is not inside any function.

2. **Function Execution Context**: Each time a function is invoked, a new execution context is created for that function. This context includes the function's local variables, parameters, and references to its outer lexical environment (e.g., variables from the enclosing scope).

3. **Eval Execution Context**: When code is executed using the `eval()` function, a new execution context is created for that code. However, the use of `eval()` is discouraged due to security and performance concerns.

Let's illustrate with an example:

```javascript
var globalVariable = 'I am a global variable';

function outerFunction(outerParameter) {
  var outerFunctionVariable = 'I am a variable inside outerFunction';

  function innerFunction(innerParameter) {
    var innerFunctionVariable = 'I am a variable inside innerFunction';
    console.log(globalVariable); // Accessing global variable
    console.log(outerParameter); // Accessing parameter of outer function
    console.log(outerFunctionVariable); // Accessing variable of outer function
    console.log(innerParameter); // Accessing parameter of inner function
    console.log(innerFunctionVariable); // Accessing variable of inner function
  }

  innerFunction('Inner parameter value');
}

outerFunction('Outer parameter value');
```

In this example:

- When the script starts executing, the global execution context is created. It includes the global scope and the `globalVariable`.
- When `outerFunction` is invoked, a new function execution context is created for it. It includes the local variable `outerFunctionVariable` and a reference to the outer lexical environment (the global execution context in this case).
- When `innerFunction` is invoked inside `outerFunction`, a new function execution context is created for it. It includes the local variable `innerFunctionVariable` and a reference to the outer lexical environment (the `outerFunction` execution context in this case).
- As each function completes execution, its execution context is popped off the call stack.

Understanding execution contexts is fundamental to understanding scope, variable visibility, and function invocation in JavaScript.

### What is global scope in V8 engine? 

In the V8 engine, the global scope refers to the top-level scope where variables and functions are defined without being nested within any other function or block. It represents the outermost level of the JavaScript environment, where global objects, functions, and variables are accessible throughout the entire script or application. Any variable or function declared outside of a function or block belongs to the global scope in JavaScript when executed in the V8 engine.

### Explain how callstack and execution environment  works in tandem? 

---

#### Understanding the Call Stack and Execution Context in Node.js: A Beginner's Guide

When working with Node.js, understanding the call stack and execution context is crucial for understanding how your code is executed. In this article, we'll explore how the call stack and execution context work in Node.js and how they relate to the `"main"` field in the `package.json` file.

##### What is the Call Stack?

The call stack is a data structure used by JavaScript engines like V8 (which powers Node.js) to keep track of function calls during the execution of a program. Every time a function is invoked, it's added to the top of the call stack. When a function completes its execution, it's removed from the stack.

##### What is Execution Context?

Execution context is the environment in which JavaScript code is executed. It consists of the scope chain, variable objects, and the `this` keyword. Every time a function is invoked, a new execution context is created, representing the environment in which that function is executed.

##### The `"main"` Field in `package.json`

In Node.js, the `"main"` field in the `package.json` file specifies the entry point for your module. This is the file that will be executed when you import or require your module in another file, or when you run it directly with Node.js.

For example, consider the following `package.json` file:

```json
{
  "name": "my-module",
  "version": "1.0.0",
  "main": "index.js"
}
```

Here, `"main": "index.js"` specifies that `index.js` is the entry point for the module.

##### Example: Understanding the Call Stack and Execution Context in Node.js

Let's walk through an example to understand how the call stack and execution context work in Node.js with the `"main"` field.

Suppose we have the following `index.js` file:

```javascript
// index.js
function greet(name) {
    console.log("Hello, " + name + "!");
}

function main() {
    greet("Alice");
}

main();
```

In this example, `main()` is the main function that we want to execute. When we run `index.js` with Node.js (`node index.js`), the following happens:

1. Node.js starts executing `index.js`.
2. A global execution context is created, representing the environment in which the top-level code is executed.
3. The `main()` function is encountered and called, so it is pushed onto the call stack, and a new execution context for `main()` is created.
4. Inside `main()`, `greet("Alice")` is encountered and called, so it's pushed onto the call stack, and a new execution context for `greet()` is created.
5. Inside `greet()`, a `console.log()` call is executed, printing "Hello, Alice!" to the console.
6. `greet()` completes its execution and is popped off the call stack, and its execution context is removed.
7. Execution returns to `main()` after `greet()` completes, and `main()` completes its execution, so it's popped off the call stack, and its execution context is removed.
8. Finally, the initial call to execute `index.js` is popped off the call stack, and the global execution context is removed, and the program terminates.

### tags:
#NodeJS
#NodeJSExecutionContext
#ExecutionContext
