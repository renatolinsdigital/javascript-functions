# Javascript functions - Essential notes by Renato Lins

JavaScript functions make up the essential 20% of code knowledge that brings about 80% of effectiveness in using the language. They empower developers by letting them bundle and reuse code, acting as the key element in creating projects that are not just functional but also neat, well-organized, and super efficient. In this guide, I'll share some essential points about JavaScript functions that might help in understanding the language better and serve as the foundation for good coding practices.

## Table of Contents

1. [Function Basics](#1-function-basics)
2. [Function Hoisting](#2-function-hoisting)
3. [Scope I - Function Scope and Context](#3-scope-i---function-scope-and-context)
4. [Higher-Order Functions](#4-higher-order-functions)
5. [Closures](#5-closures)
6. [Callbacks and Asynchronous JavaScript](#6-callbacks-and-asynchronous-javascript)
7. [Default Parameters and Rest Parameters (ES6 Features)](#7-default-parameters-and-rest-parameters-es6-features)
8. [IIFE (Immediately Invoked Function Expression)](#8-iife-immediately-invoked-function-expression)
9. [Recursion](#9-recursion)
10. [Scope II - The Methods .call, .apply, and .bind](#10-scope-ii---the-methods-call-apply-and-bind)
11. [Generator Functions](#11-generator-functions)
12. [Promises, Async-Await, and How They Relate to Functions](#12-promises-async-await-and-how-they-relate-to-functions)
13. [Function Currying](#13-function-currying)
14. [Memoization](#14-memoization)
15. [Prototype and Functions as Object Constructors](#15-prototype-and-functions-as-object-constructors)
16. [Closures and Data Privacy](#16-closures-and-data-privacy)
17. [Closures and Functional Programming](#17-closures-and-functional-programming)
18. [Closures and Event Handling](#18-closures-and-event-handling)
19. [The 'Event Loop' and How It Relates to Function Calls](#19-the-event-loop-and-how-it-relates-to-function-calls)
20. [Functions as First-Class Citizens](#20-functions-as-first-class-citizens)

### 1. Function basics

There are two common ways to define functions in JavaScript: function declarations and function expressions.

__Function Declaration:__ A function declaration is a way to define a function with the function keyword, followed by the function name, a list of parameters enclosed in parentheses, and a block of code enclosed in curly braces.

```javascript
// Defining the function
function add(x, y) {
  // Calculating the sum of x and y
  const sum = x + y;
  
  // Returning the calculated sum
  return sum;
}

// Calling the function and storing the returned value on a 'const'
const mySum = add(3, 7);

// Displaying the result
console.log(mySum) // Output: 10
```

In this example:

* __add__ is the function name
* __(x, y)__ are the parameters or 'placeholders' that represent the data that the function is receiving when called
* __const sum = x + y;__ represents the function body, where the actual code is executed
* __return sum;__ is result returned after the code is executed

In JavaScript, a function declaration must have a name. The syntax for a function declaration includes the ```function``` keyword followed by the name of the function, a list of parameters enclosed in parentheses, and a block of code enclosed in curly braces. This idea is not true for function expressions (they are nameless), as we will see next.

__Function Expression:__ A function expression is another way to define a function by assigning it to a variable. This can be done using the function keyword or the more modern arrow function syntax (=>).

```javascript
// Example 1 - Using function keyword

// Defining the function while storing it as a value (as part of an expression)
const subtract = function(x, y) {
  return x - y;
};

// Calling the function and storing the returned value on a 'const'
const mySubtract = subtract(7, 3);

// Displaying the result
console.log(mySubtract) // Output: 4
```

```javascript
// Example 2 - arrow function with the arrow syntax

// Also defining the function and storing it as a value
const subtract = (x, y) => x - y;

// Calling the arrow function (nothing changes)
const mySubtract = subtract(7, 3);

// Displaying the result
console.log(mySubtract); // Output: 4
```

In both examples, the functions themselves are nameless, yet we can reference them by associating them to values. Consequently, we call ```subtract()``` as if it were a function, while, in reality, it is a variable storing a nameless function expression. The term "nameless" refers to the fact that the functions themselves don't have a separate identifier, but they are effectively named through the variables to which they are assigned.

__Function declaration vs. function expressions:__ For routine programming tasks, functions can be declared using either named or anonymous approaches. Named functions are particularly beneficial for self-referencing, such as in recursive functions, and they contribute to clearer stack traces during debugging, potentially easing the identification of functions causing issues. In the majority of scenarios, there is no substantial performance difference between named and anonymous functions. Modern JavaScript engines are optimized to efficiently handle both types. However, in certain extreme cases, named functions might exhibit a marginal speed advantage due to potential optimizations in specific JavaScript engines. Nevertheless, arrow functions are often preferred not just for their modern and concise syntax but also for their predictability, especially when developers navigate thru concepts like __hoisting__ and __context__ as we will see later on.

__Types of arrow functions:__ Arrow functions provide a concise syntax for writing functions. There are two main forms of arrow functions: the short syntax for one-liner functions, and the long syntax for functions with multiple statements or requiring a more explicit structure.

* Short Syntax (One-Liner Arrow Functions as seen previously): Here the ```return``` statement is implicit and does not require curly braces.

```javascript
// Short syntax for squaring a number
const square = (x) => x * x;
```

* Long Syntax (Block Body Arrow Functions): The long syntax is used when a function requires multiple statements or a more explicit structure. It involves using curly braces ```{}``` to define a block of code. The ```return``` statement is needed explicitly if the function is expected to return a value.

```javascript
// Long syntax for a function with conditional logic
const greet = (timeOfDay) => {
  if (timeOfDay === 'morning') {
    return 'Good morning!';
  } else {
    return 'Hello!';
  }
};
```

### 2. Function Hoisting

Function hoisting is a behavior in JavaScript where function declarations are moved to the top of their containing scope during the compilation phase. This means that you can call a function before its actual declaration in the code, and the JavaScript engine will still recognize and execute the function call correctly.

In the provided example:

```javascript
greet("Alice"); // This works even though the function is called before its declaration.

function greet(name) {
    console.log(`Hello, ${name}!`);
}
```

The ```greet``` function is called before its declaration, which might seem counterintuitive in other programming languages. However, due to hoisting, during the compilation phase, the JavaScript engine moves the entire function declaration to the top of its scope. Therefore, by the time the code is executed, the function greet is already available, and the function call works as expected. The equivalent hoisted version of the code would be:

```javascript
function greet(name) {
    console.log(`Hello, ${name}!`);
}

greet("Alice"); 
```

This behavior applies specifically to function declarations (using the function keyword). It does not apply to function expressions or arrow functions. While function declarations are fully hoisted and can be used before their declaration, function expressions and arrow functions are only partially hoisted. The variable declarations are hoisted, but the function assignments are not, leading to potential errors if accessed before actual declaration:

```javascript
// This will result in an error
greet("Alice"); // Error: Cannot access 'greet' before initialization

let greet = function(name) {
    console.log(`Hello, ${name}!`);
};
```

Assigning function expressions to ```let``` and ```const``` is not just preferable because they represent modern ways of declaring variables, but also because they enforce more predictable scenarios when we only call functions after they are defined. The error mentioned above arises when encountering the concept of the 'Temporal Dead Zone (TDZ)'. The TDZ is a period during which accessing a variable or constant before its actual declaration results in an error.

```javascript
// Trying to access the variable before declaration
console.log(name); // ReferenceError: Cannot access 'name' before initialization

// Declaration and assignment in the same scope
let name = "Alice";

// Now accessing the variable after declaration
console.log(name); // Outputs: Alice
```

### 3. Scope I - Function scope and context

JavaScript has three main scopes: global, local/function and block scopes. Variables declared inside a function are only accessible within that function scope, creating a localized environment.

```javascript
function printMessage() {
    const message = "Hello!";
    console.log(message); // "Hello!" is printed.
}

console.log(message); // Throws an error, 'message' is not defined.
```

 __'this' binding (context):__ The ```this``` value references the context or scope that the code is considering. Named function declarations, dynamically adjust their this value based on where they are called. In contrast, arrow functions inherit the context from the scope in which they are defined. Let's examine these examples:

```javascript
// 1 - Example with function declaration:

function regularFunction() {
  console.log("'this' in regularFunction refers to the global object");
}

regularFunction();

// Creating an object 'obj' with a method 'method' that points to regularFunction.
const obj = {
  method: regularFunction,
};
// Calling the method 'method' on 'obj'.
obj.method(); // 'this' in regularFunction now refers to the object 'obj' in this context

// 2 - Example with Arrow function call:

const arrowFunction = () => {
  console.log("'this' in arrowFunction still refers to the global object");
};

// Calling arrowFunction directly.
arrowFunction();

// Creating an object 'arrowObj' with a method 'method' that points to arrowFunction.
const arrowObj = {
  method: arrowFunction,
};
// Calling the method 'method' on 'arrowObj'.
arrowObj.method(); 
/* 'this' in arrowFunction still refers to the global object, not 'arrowObj'
 (lexical scoping doesn't create a new 'this' for arrow functions)*/
```

So we have:

__Scope__: In arrow functions the value of ```this``` is determined by where it is defined, not where it is used. In function declarations using the ```function``` keyword, the value of ```this``` is dynamically determined at runtime, depending on how the function is invoked.

__Usage:__ Function declarations are suitable for methods in objects or functions that need their own ```this``` context. Arrow functions are often used for concise one-liner functions, callbacks, and in scenarios where you want to preserve the surrounding this context.

__The 'return' syntax__: Function declarations always have an explicit return statement. Arrow functions with a single expression can have an implicit return, meaning the expression's value is automatically returned without needing a return keyword.

Extra consideation: Function declarations(or regular functions) have access to the arguments object, which holds all arguments passed to the function. Arrow functions do not have their own arguments object. They inherit the arguments from the enclosing scope.

### 4. Higher-Order Functions

Higher-order functions in JavaScript are functions that can either accept other functions as arguments or return functions as their results. Also, we can consider the passed function as a higher-order function, since it operates on another function. This unique feature empowers functions to be treated as dynamic entities within the language, granting greater flexibility in programming. An exemplary case of a higher-order function is the ```operateOnArray``` function:

```javascript
function operateOnArray(arr, operation) {
    const result = [];
    for (const item of arr) {
        result.push(operation(item));
    }
    return result;
}

const numbers = [1, 2, 3];
const doubled = operateOnArray(numbers, (x) => x * 2);
```

Notably, the function ```(x) => x * 2```, passed as an argument to ```operateOnArray```, is itself a higher-order function as it operates on another function. This powerful concept of higher-order functions fundamentally contributes to the versatility and expressiveness of JavaScript programming.

### 5. Closures

A closure in JavaScript occurs when an inner function retains access to variables from its outer (enclosing) function's scope, even after the outer function has completed its execution. This allows the inner function to "remember" and utilize those variables whenever it's called, creating a lasting connection between the inner and outer functions.

```javascript
function createMultiplier(factor) {
  /* The inner function below retains the outer scope's factor value even after
   createMultiplier has finished executing */
  return function (value) {
    return value * factor;
  };
}
```

Upon invoking createMultiplier, the ```factor``` value remains available for utilization by the inner function. Since the inner function is returned (and can be linked to a function expression), we obtain a function capable of receiving values for its internal operations while also accessing the value provided in the outer scope function (in this case, ```factor```).  With this in mind, we can use the closure above to do the following:

```javascript
const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // Outputs 10.
console.log(triple(5)); // Outputs 15.
```

Closures are essential for various programming patterns, like creating private data, implementing data encapsulation, and enabling certain capabilities for both functional and asynchronous programming.

### 6. Callbacks and Asynchronous JavaScript

Functions can be used as callbacks in asynchronous operations to handle actions that might take some time to complete, like fetching data or handling events.

```javascript
function fetchData(callback) {
    // Simulate fetching data from a server
    setTimeout(() => {
        const data = { name: "John", age: 30 };
        callback(data);
    }, 1000);
}

function displayData(data) {
    console.log(data);
}

fetchData(displayData);
```

Note: The problem with depending on callbacks for asynchronous operations is that we might encounter a situation where we need to chain multiple callbacks one after the other, making the code difficult to read and potentially prone to bugs. To tackle this issue, JavaScript introduces the concept of promises, which will be covered later in this tutorial.

### 7. Default Parameters and Rest Parameters (ES6 Features)

In ES6, default parameters and rest parameters were introduced to make functions more flexible.

__Default Parameters:__

You can set default values for function parameters. If a value is not provided when the function is called, the default value is used instead. For example:

```javascript
function greet(name = "User") {
    return `Hello, ${name}!`;
}
```

In this case, if you call ```greet``` without providing a name, it will default to "User". If you call greet("John"), it will use the provided name.

__Rest Parameters:__

 The rest parameter (```...numbers``` in this case) allows a function to accept an indefinite number of arguments as an array. You can then perform operations on this array within the function. For example:

```javascript
function sum(...numbers) {
    // 'numbers' is an array containing all the provided arguments
    return numbers.reduce((acc, val) => acc + val, 0);
}
```

Here, the sum function can take any number of arguments, and it will calculate their sum using the ```reduce``` method. For instance, ```sum(1, 2, 3)``` will return ```6```. In summary, default parameters provide a fallback value if an argument is not provided, and rest parameters allow a function to handle an arbitrary number of arguments as an array, enabling flexible and dynamic behavior.

### 8. IIFE (Immediately Invoked Function Expression)

An IIFE is a function that is defined and executed immediately after its creation. It's often used to create a private scope and prevent variable name clashes.

```javascript
// Define and immediately invoke an IIFE (Immediately Invoked Function Expression)
(function() {
    const privateVar = "This is private";
})();

// Attempting to access privateVar outside the IIFE will result in an error
console.log(privateVar); // Error: privateVar is not defined
```

### 9. Recursion

Recursion is a programming technique where a function calls itself in order to solve a problem. It's particularly useful for solving problems that can be broken down into smaller instances of the same problem. Recursive solutions typically involve two main components:

__Base case(s):__ These are specific conditions where the function does not make a recursive call and instead returns a known and straightforward result. Base cases prevent the recursion from continuing indefinitely and provide a stopping condition.

__Recursive case(s):__ In this part of the function, the problem is broken down into smaller instances of the same problem. The function calls itself with a modified input, and the results are combined to solve the original problem.

Here's a simple example in JavaScript that demonstrates recursion by calculating the factorial of a number:

```javascript
function factorial(n) {
    // Base case
    if (n <= 1) {
        return 1;
    }
    
    // Recursive case
    return n * factorial(n - 1);
}

// Example usage
console.log(factorial(5)); // Outputs 120
```

In this example:

Base case: If n is less than or equal to 1, the function returns 1. This is the stopping condition, preventing the recursion from going on forever.

Recursive case: If n is greater than 1, the function returns n multiplied by the result of calling itself with the argument n - 1. This is the recursive step, where the problem is broken down into a smaller instance.

### 10. Scope II - The Methods .call(), .apply(), and .bind()

In JavaScript, the methods ```.call()```, ```.apply()```, and ```.bind()``` are used to manipulate how a function is executed and what object it should reference. They provide a way to control the context (the value of ```this```) and pass arguments to a function.

- __.call()__: Calls a function with a specified this value and individual arguments.
- __.apply()__: Calls a function with a specified this value and an array of arguments.
- __.bind()__: Returns a new function with a specified this value, which can be called later.

```javascript
const person = {
    firstName: "John",
    lastName: "Doe",
};

function greet(greeting) {
    console.log(`${greeting}, ${this.firstName} ${this.lastName}`);
}

greet.call(person, "Hello"); // Outputs "Hello, John Doe"
greet.apply(person, ["Hi"]); // Outputs "Hi, John Doe"
const boundGreet = greet.bind(person);
boundGreet("Hey"); // Outputs "Hey, John Doe"
```

Why They Can Be Useful:

__Dynamic Context:__ ```.call()``` and ```.apply()``` allow you to set the this value dynamically, enabling you to execute a function within the context of a specific object.

__Partial Function Application:__ ```.bind()``` is often used for partial function application, where you create a new function with certain arguments fixed. This is useful when you want to create a function with some arguments preset for future use.

__Code Reusability:__ These methods contribute to code reusability by allowing you to reuse functions in different contexts without modifying their core logic.

__Method Borrowing:__ With ```.call()``` and ```.apply()```, you can borrow methods from one object and apply them to another object.

### 11. Generator Functions

Generator functions allow you to pause and resume their execution, yielding values one at a time. They are useful for dealing with asynchronous operations and lazy evaluation.

```javascript
function* countToFive() {
    for (let i = 1; i <= 5; i++) {
        yield i;
    }
}

const counter = countToFive();
console.log(counter.next().value); // Outputs 1
console.log(counter.next().value); // Outputs 2
// And so on...
```

When to consider using Generator Functions:

__Lazy Evaluation:__ Generator functions enable lazy evaluation, meaning values are generated on-demand rather than all at once. This can be advantageous when dealing with large datasets or expensive computations, as you can generate values only when needed.

__Asynchronous Operations:__ While ```async-await``` is commonly used for managing asynchronous operations, generator functions offer an alternative approach. Generators can be used with the yield keyword to pause execution and handle asynchronous tasks in a more sequential manner.

__Infinite Sequences:__ Generator functions can represent infinite sequences of values. Traditional loops would require special handling to achieve this, but generators can keep producing values indefinitely using the ```yield``` statement.

__Memory Efficiency:__ For scenarios where you need to process a large dataset, generators allow you to handle one item at a time, reducing memory consumption compared to loading the entire dataset into memory at once.

__Stateful Iteration:__ Generator functions maintain their state between calls, making them suitable for scenarios where the iteration process needs to remember its position.

### 12. Promises, async-await and how they relate to functions

Promises and ```async-await``` are mechanisms for dealing with asynchronous code in a more organized and readable manner. 

__Promisse:__ Promises provide a cleaner way to manage asynchronous operations. The example below demonstrates a function, ```fetchData()```, that returns a Promise. The Promise is used to handle the asynchronous process of fetching data, simulating a server request with a delay. The ```.then()``` method is then used to handle the resolved value (successful result), and ```.catch()``` is used for error handling. A common way to create promisses is to attach a new Promise to the return of a function. In technical terms, we are using the ```new``` keyword to instantiate a new promisse in the shape of the ```Promise``` constructor (that is a built-in Javascript feature).

```javascript
function fetchData() {
    return new Promise((resolve, reject) => {
        // Simulating an asynchronous operation with a random success/failure outcome
        setTimeout(() => {
            const operationResult = Math.random() > 0.5;

            if (operationResult) {
                const data = { name: "Alice", age: 25 };
                resolve(data); // Resolve with data
            } else {
                reject("Operation failed!"); // Reject with an error message
            }
        }, 1000);
    });
}

/* Calling 'fetchData', which returns a Promise with asynchronous operation capabilities.
 Be aware that 'fetchData' is no longer a regular function; it is now a Promise that 
 may resolve with data or reject with an error.*/
fetchData()
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

__async-await__: The ```async-await``` syntax provides a more synchronous-looking way to work with asynchronous code. In the example, the function ```fetchDataAsync()``` is declared as ```async```, and within it, ```await``` is used to pause the execution until the ```fetchData()``` resolves or rejects. This enhances readability by making the asynchronous code appear similar to synchronous code.

```javascript
async function fetchDataAsync() { // 'async' keyword is necessary if we plan to use 'wait'
    try {
        /* 'fetchData' is is a promise that is temporarily paused until it either 
        resolves with a result or rejects with an error. */
        const data = await fetchData(); 
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

fetchDataAsync();
```

### 13. Function Currying

Currying is the process of converting a function that takes multiple arguments into a series of functions that take one argument each.

```javascript
function add(x) {
    return function(y) {
        return x + y;
    };
}

const add5 = add(5);
console.log(add5(3)); // Outputs 8
```

Note that this technique is entirely based on the concept of closures.

### 14. Memoization

Memoization is a technique used to optimize the performance of functions by caching, or storing, their computed results for specific inputs. This helps avoid redundant computations and speeds up the execution of the function. In the context of the Fibonacci sequence, which is a series of numbers where each number is the sum of the two preceding ones (e.g., 0, 1, 1, 2, 3, 5, 8, ...), memoization helps avoid recalculating the same Fibonacci numbers multiple times. The example provided demonstrates how memoization can be applied:

```javascript
// Function to calculate Fibonacci numbers with memoization
function fibonacci(n, memo = {}) {
    // Check if the result for index 'n' is already memoized
    if (n in memo) {
        return memo[n]; // Return the cached result
    }

    // Base case: if 'n' is 0 or 1, return 'n' itself
    if (n <= 1) {
        return n;
    }

    // Recursive calculation with memoization
    memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);
    
    // Return the calculated result for index 'n'
    return memo[n];
}

// Example usage: calculate and output Fibonacci number at index 10
console.log(fibonacci(10)); // Outputs 55
```

To make it clear:

* The memo object stores previously calculated Fibonacci numbers to avoid recomputation.
* If the result for the input n is in memo, it is returned.
* Base cases handle indices 0 and 1, returning n.
* For other indices, the function recursively calculates Fibonacci numbers.
* Calculated results are stored in memo.
* The function returns the Fibonacci number for the input index.

### 15. Prototype and functions as object constructors

In JavaScript, functions can serve as constructors, allowing the creation of objects with specific characteristics. Objects generated from a constructor inherit properties and methods from the constructor's prototype, providing a blueprint for object creation.

```javascript
// Define a constructor function 'Person' that takes a 'name' parameter
function Person(name) {
    // Assign the 'name' parameter to the 'name' property of the object being created
    this.name = name;
}

// Add a 'greet' method to the prototype of the 'Person' constructor
Person.prototype.greet = function() {
    console.log(`Hello, my name is ${this.name}`);
};

// Create instances of 'Person' using the 'new' keyword
const person1 = new Person("Alice");
const person2 = new Person("Bob");

// Call the 'greet' method on the created instances
person1.greet(); // Outputs "Hello, my name is Alice"
person2.greet(); // Outputs "Hello, my name is Bob"
```

To further enhance clarity, we can consider:

* The Person function acts as a blueprint for creating objects with a name property.
* The ```greet``` method is added to the prototype of the Person function, making it accessible to all instances.
* Using the ```new``` keyword creates instances of Person, initializing the ```name``` property for each.
* The ```greet``` method can be called on each instance, displaying a personalized greeting based on the assigned name.

### 16. Closures and Data Privacy

Closures are often used to create private variables and encapsulate data within a function, preventing direct access from the outside world.

```javascript
function createCounter() {
    let count = 0;

    return function() {
        return ++count;
    };
}

const counter = createCounter();
console.log(counter()); // Outputs 1
console.log(counter()); // Outputs 2
```

### 17. Closures and Functional Programming:

Functional programming is a programming paradigm that emphasizes the use of functions as __first-class citizens__ (further explained later on). In this paradigm, closures and higher-order functions play a crucial role. In the example provided, ```temperatureConverter``` is a higher-order function that takes ```baseUnit``` as an argument and returns another function. This returned function, when invoked with a value, performs temperature conversions based on the specified base unit.

```javascript
function temperatureConverter(baseUnit) {
    return function(value) {
        if (baseUnit === 'Celsius') {
            return value * 9/5 + 32;
        } else if (baseUnit === 'Fahrenheit') {
            return (value - 32) * 5/9;
        }
    };
}

const celsiusToFar = temperatureConverter('Celsius');
console.log(celsiusToFar(25)); // Output: 77

const farToCelsius = temperatureConverter('Fahrenheit');
console.log(farToCelsius(98.6)); // Output: 37
```

Here's a breakdown:

* ```temperatureConverter``` is a higher-order function as it returns a function.
* The returned function retains access to the ```baseUnit``` parameter from the outer function, creating a closure.
* ```celsiusToFar``` and ```farToCelsius``` are instances of the returned function, each specialized for a specific temperature unit.
* When these instances are invoked with a value, they perform the temperature conversion as per their base unit.

This example showcases how closures enable the creation of reusable and specialized functions, aligning with the principles of functional programming, allowing for flexibility and composability.

### 18. Closures and Event Handling:

Closures are a powerful concept often employed in event handling to capture and retain the specific state of variables at the moment when an event listener is attached. This ensures that the listener correctly accesses the data it needs even when the event occurs later, potentially when the surrounding function has finished executing.

In the provided code example:

```javascript
function attachHandler(element, eventName) {
    element.addEventListener(eventName, function() {
        console.log(`Event ${eventName} on ${element.id}`);
    });
}

const button = document.getElementById("my-button");
attachHandler(button, "click");
```

The ```attachHandler``` function takes an HTML element (```element```) and an event name (```eventName```) as parameters. Inside the function, a closure is created when the anonymous function (event listener) is defined within the ```addEventListener``` call. This closure captures the values of both element and eventName at the time of the function call. When the event (in this case, a "click" event) occurs on the specified element (in this case, a button with the id ```my-button```), the attached event listener is executed. Since the closure captured the values of ```element``` and ```eventName``` when ```attachHandler``` was called, the listener correctly logs the event details, including the event name and the id of the element. This mechanism ensures that even if the ```attachHandler``` function completes its execution and the surrounding context changes, the event listener still has access to the variables it needs due to the closure's encapsulation of those values.

### 19. The 'Event loop' and how it relates to function calls

JavaScript is a single-threaded language, meaning it has only one execution thread. This thread executes code line by line. If a piece of code takes a long time to run, it can block the entire thread, making the application unresponsive. JavaScript is designed to be __non-blocking__ and uses an __event-driven__ model to handle asynchronous code. This can lead to some behaviors that may seem counterintuitive at first, but I hope it will make sense once we delve into the following concepts:

__Call Stack:__ The call stack is a mechanism that keeps track of the functions being executed. When a function is called, it is added to the top of the stack, and when the function completes, it is removed from the stack. The call stack follows a last-in, first-out (LIFO) order, meaning the last function added is the first one to be executed.

__Callback Queue:__ The callback queue (also known as the task queue or message queue) is where asynchronous callbacks are placed after their associated operations complete. Asynchronous operations, such as those initiated by timers (setTimeout), AJAX/HTTP requests, or event handlers, involve callbacks that are not executed immediately. Instead of blocking the main thread, these callbacks are scheduled to run after the current execution context is finished. When the callback is ready to be executed, it is placed in the callback queue.

__Event Loop:__ The event loop is a continuous process that runs in the background, constantly monitoring the call stack and the callback queue. If the call stack is empty, the event loop takes the first task (callback) from the callback queue and pushes it onto the call stack for execution. This process ensures that asynchronous tasks are executed in a non-blocking manner.

Now we are able to analyse the following situation:

```javascript
console.log("Start"); 

setTimeout(() => {
  console.log("Inside setTimeout callback");
}, 0);

console.log("End");

// Output:
// Start
// End
// Inside setTimeout callback
```

So we will have:

* The ```console.log("Start")``` and ```console.log("End")``` statements are synchronous function calls, and they will be executed immediately in the order they appear.

* The ```setTimeout``` initiates an asynchronous operation by scheduling its callback function to be executed after a minimal delay (even though it's specified as 0).

* The ```console.log("Inside setTimeout callback")``` statement is part of the callback function passed to setTimeout, and its execution is delayed.

While the example uses ```console.log()```, the behavior is similar for other functions. The key point is to demonstrate how asynchronous operations impact the flow of the program and introduce a delay in execution order.

### 20. Functions as first-class citizens:

The concept of "first-class citizens" is a fundamental principle in programming languages, including JavaScript. It refers to the idea that entities such as functions and variables are treated as equal citizens within the language, meaning they can be used, manipulated, and passed around just like any other data type. In simpler terms, in a programming language that treats entities as first-class citizens, you can do the following with functions and variables:

__Assign to Variables:__ You can assign functions to variables just like you would with numbers, strings, or other data types. This is often called "function assignment" or using functions as "first-class values."

__Pass as Arguments:__ You can pass functions as arguments to other functions. This is particularly useful when you want to create more dynamic and flexible code. Functions that accept other functions as arguments are often referred to as "higher-order functions."

__Return as Values:__ Functions can be returned as values from other functions. This enables you to create functions that generate and customize other functions on the fly.

__Store in Data Structures:__ Functions can be stored in data structures like arrays or objects, allowing you to organize and manage functions in a structured manner.

__Composability:__ With these mentioned behaviors, JavaScript accommodates the functional programming paradigm extremely well. The language provides a modern and highly recommended approach to code reuse, commonly known as __composability__. Composability, within the context of functional programming, involves combining smaller, independent functions to generate more advanced functionality. This approach is valued for various reasons:

* Modularity: Composable functions are modular, meaning they are designed to perform a specific task or solve a specific problem. This modular approach makes code easier to understand and maintain. 

* Reusability: Composable functions are reusable components that can be employed in various parts of a program or even in different projects. When functions are designed with a specific purpose and are independent of the larger context, they become versatile tools that can be reused wherever needed.

* Readability: Composing functions allows developers to express complex operations in a more readable and concise manner. By breaking down a problem into smaller, focused functions, the overall logic becomes clearer, and the codebase becomes more comprehensible.

* Scalability: Composable functions contribute to scalable code. As a project grows, developers can easily extend and modify functionality by combining existing functions or introducing new composable functions. This scalability is crucial for managing large or complex code bases.

* Testability: Composable functions facilitate unit testing, as each function can be tested independently. Testing smaller units of code is generally more straightforward than testing large, monolithic components. This promotes a robust testing strategy and ensures the reliability of the software.

### Conclusion:

Functions are the backbone of JavaScript, providing a powerful mechanism for code organization, reusability, and project structuring. By understanding the various aspects of functions, from basic syntax to advanced concepts like closures and functional programming, you'll be equipped to write more efficient and maintainable code. Dive in, experiment, and level up! Happy coding! 
