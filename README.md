# Javascript functions - Essential notes by Renato Lins

JavaScript functions make up the essential 20% of code knowledge that brings about 80% of effectiveness in using the language. They empower developers by letting them bundle and reuse code, acting as the key element in creating projects that are not just functional but also neat, well-organized, and super efficient. In this guide, I'll share some essential points about JavaScript functions that might help in understanding the language better and serve as the foundation for good coding practices.

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

The greet function is called before its declaration, which might seem counterintuitive in other programming languages. However, due to hoisting, during the compilation phase, the JavaScript engine moves the entire function declaration to the top of its scope. Therefore, by the time the code is executed, the function greet is already available, and the function call works as expected. The equivalent hoisted version of the code would be:

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

__Scope__: In arrow functions the value of __this__ is determined by where it is defined, not where it is used. In function declarations using the function keyword, the value of this is dynamically determined at runtime, depending on how the function is invoked.

__Usage:__ Function declarations are suitable for methods in objects or functions that need their own this context. Arrow functions are often used for concise one-liner functions, callbacks, and in scenarios where you want to preserve the surrounding this context.

__The 'return' syntax__: Function declarations always have an explicit return statement. Arrow functions with a single expression can have an implicit return, meaning the expression's value is automatically returned without needing a return keyword.

Extra consideation: Function declarations(or regular functions) have access to the arguments object, which holds all arguments passed to the function. Arrow functions do not have their own arguments object. They inherit the arguments from the enclosing scope.

### 4. Higher-Order Functions

Higher-order functions in JavaScript are functions that can either accept other functions as arguments or return functions as their results. Also, we can consider the passed function as a higher-order function, since it operates on another function. This unique feature empowers functions to be treated as dynamic entities within the language, granting greater flexibility in programming. An exemplary case of a higher-order function is the operateOnArray function:

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

Notably, the function ```(x) => x * 2```, passed as an argument to operateOnArray, is itself a higher-order function. It operates on another function—each array element's value, in this case. This powerful concept of higher-order functions fundamentally contributes to the versatility and expressiveness of JavaScript programming.

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

Upon invoking createMultiplier, the 'factor' value remains available for utilization by the inner function. Since the inner function is returned (and can be linked to a function expression), we obtain a function capable of receiving values for its internal operations while also accessing the value provided in the outer scope function (in this instance, 'factor').  With this in mind, we can use the closure above to do the following:

```javascript
const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // Outputs 10.
console.log(triple(5)); // Outputs 15.
```

Closures are essential for various programming patterns, like creating private data, implementing data encapsulation, and achieving certain functionalities in functional programming and asynchronous programming.

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

### 7. Default Parameters and Rest Parameters (ES6 Features)

In ES6, default parameters and rest parameters were introduced to make functions more flexible.

__Default Parameters:__

You can set default values for function parameters. If a value is not provided when the function is called, the default value is used instead. For example:

```javascript
function greet(name = "User") {
    return `Hello, ${name}!`;
}
```

In this case, if you call greet() without providing a name, it will default to "User." If you call greet("John"), it will use the provided name.

__Rest Parameters:__

 The rest parameter (...numbers in this case) allows a function to accept an indefinite number of arguments as an array. You can then perform operations on this array within the function. For example:

```javascript
function sum(...numbers) {
    // 'numbers' is an array containing all the provided arguments
    return numbers.reduce((acc, val) => acc + val, 0);
}
```

Here, the sum function can take any number of arguments, and it will calculate their sum using the reduce method. For instance, ```sum(1, 2, 3)``` will return ```6```. In summary, default parameters provide a fallback value if an argument is not provided, and rest parameters allow a function to handle an arbitrary number of arguments as an array, enabling flexible and dynamic behavior.

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

Recursion is a technique where a function calls itself to solve a problem. It's particularly useful for solving problems that can be broken down into smaller, similar sub-problems. Example:

```javascript
function factorial(n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);
}

console.log(factorial(5)); // Outputs 120
```

### 10. Scope II - The Methods .call, .apply, and .bind

These are methods that can be used to control the context (this) in which a function is executed.

- __call__: Calls a function with a specified this value and individual arguments.
- __apply__: Calls a function with a specified this value and an array of arguments.
- __bind__: Returns a new function with a specified this value, which can be called later.

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

### 12. Promises, async-await and how they relate to functions

Promises and ```async-await``` are mechanisms for dealing with asynchronous code in a more organized and readable manner. 

__Promisse:__ Promises provide a cleaner way to manage asynchronous operations. The example below demonstrates a function, ```fetchData()```, that returns a Promise. The Promise is used to handle the asynchronous process of fetching data, simulating a server request with a delay. The ```.then()``` method is then used to handle the resolved value (successful result), and ```.catch()``` is used for error handling. A common way to create promisses is to attach a new Promise to the return of a function. In technical terms, we are using the ```new``` keyword to instantiate a new promisse in the shape of the ```Promise``` built-in object constructor.

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

// Calling 'fetchData', which returns a Promise with asynchronous operation capabilities.
// 'fetchData' is no longer a regular function; it is now a Promise that may resolve with data or reject with an error.
fetchData()
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

__async-await__: The ```async-await``` syntax provides a more synchronous-looking way to work with asynchronous code. In the example, the function ```fetchDataAsync()``` is declared as ```async```, and within it, ```await``` is used to pause the execution until the ```fetchData()``` resolves or rejects. This enhances readability by making the asynchronous code appear similar to synchronous code.

```javascript
async function fetchDataAsync() { // 'async' keyword is necessary if we plan to use 'wait' inside of that same function
    try {
        // fetchData is is a promise that is temporarily paused until it either resolves with a result or rejects with an error.
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

Memoization is a technique to optimize functions by caching their results for specific inputs, reducing redundant computations.

```javascript
function fibonacci(n, memo = {}) {
    if (n in memo) {
        return memo[n];
    }
    if (n <= 1) {
        return n;
    }
    memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);
    return memo[n];
}

console.log(fibonacci(10)); // Outputs 55
```

### 15. Prototype and functions as object constructors

In JavaScript, functions can serve as constructors for objects. Objects created from a constructor function inherit properties and methods from the constructor's prototype.

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    console.log(`Hello, my name is ${this.name}`);
};

const person1 = new Person("Alice");
const person2 = new Person("Bob");

person1.greet(); // Outputs "Hello, my name is Alice"
person2.greet(); // Outputs "Hello, my name is Bob"
```

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

Closures are essential in functional programming paradigms. They enable the creation of higher-order functions that can carry data or state between calls, also enabling multiple and flixible usage of functions.

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

### 18. Closures and Event Handling:

Closures are a powerful concept often employed in event handling to capture and retain the specific state of variables at the moment when an event listener is attached. This ensures that the listener correctly accesses the data it needs even when the event occurs later, potentially when the surrounding function has finished executing.

In the provided code example:

```javascript
function attachHandler(element, eventName) {
    element.addEventListener(eventName, function() {
        console.log(`Event ${eventName} on ${element.id}`);
    });
}

const button = document.getElementById("myButton");
attachHandler(button, "click");
```

The attachHandler function takes an HTML element (element) and an event name (eventName) as parameters. Inside the function, a closure is created when the anonymous function (event listener) is defined within the addEventListener call. This closure captures the values of both element and eventName at the time of the function call. When the event (in this case, a "click" event) occurs on the specified element (in this case, a button with the id "myButton"), the attached event listener function is executed. Since the closure captured the values of element and eventName when attachHandler was called, the listener correctly logs the event details, including the event name and the id of the element. This mechanism ensures that even if the attachHandler function completes its execution and the surrounding context changes, the event listener still has access to the variables it needs due to the closure's encapsulation of those values.

### 19. Functions as first-class citizens:

The concept of "first-class citizens" is a fundamental principle in programming languages, including JavaScript. It refers to the idea that entities such as functions and variables are treated as equal citizens within the language, meaning they can be used, manipulated, and passed around just like any other data type. In simpler terms, in a programming language that treats entities as first-class citizens, you can do the following with functions and variables:

__Assign to Variables:__ You can assign functions to variables just like you would with numbers, strings, or other data types. This is often called "function assignment" or using functions as "first-class values."

__Pass as Arguments:__ You can pass functions as arguments to other functions. This is particularly useful when you want to create more dynamic and flexible code. Functions that accept other functions as arguments are often referred to as "higher-order functions."

__Return as Values:__ Functions can be returned as values from other functions. This enables you to create functions that generate and customize other functions on the fly.

__Store in Data Structures:__ Functions can be stored in data structures like arrays or objects, allowing you to organize and manage functions in a structured manner.

JavaScript is an example of a programming language that treats functions as first-class citizens. This feature is why JavaScript can be used for various programming paradigms, such as functional programming, where functions are used extensively to solve problems. 

### Conclusion:

Functions are core for reusing logic, structuring projects and mastering Javascript. Learn parameters, returns, and scoping. Grasp declarations, expressions, and the differences between regular and arrow functions. Understand this behavior and higher-order functions. Use IIFE for private scopes. Learning functions is one of the first steps to unlock JavaScript's power. Dive in, experiment, and level up! Happy coding! 
