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