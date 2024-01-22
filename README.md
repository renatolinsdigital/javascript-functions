# Javascript functions - Essential notes by Renato Lins

JavaScript functions make up the essential 20% of code knowledge that brings about 80% of effectiveness in using the language. They empower developers by letting them bundle and reuse code, acting as the key element in creating projects that are not just functional but also neat, well-organized, and super efficient. In this guide, I'll share some essential points about JavaScript functions that might help in understanding the language better and serve as the foundation for good coding practices.

### 1. Function Parameters and Arguments

Parameters are placeholders in a function's declaration that represent the values the function expects to receive when it is called. Arguments are the actual values passed to the function when it is invoked.

```javascript
function addNumbers(a, b) {
    return a + b;
}

const result = addNumbers(5, 3); // Here, 5 and 3 are arguments.
```

### 2. Return Statement

Functions often produce a value that can be used elsewhere in the code. The return statement is used to specify the value that the function should output when it is executed.

```javascript
function multiply(a, b) {
    return a * b;
}

const product = multiply(4, 6); // The function returns 24.
```

### 3. Function Hoisting

Function declarations are hoisted, which means they are moved to the top of their scope during the compilation phase. This allows you to call a function before its actual declaration in the code.

```javascript
greet("Alice"); // This works even though the function is called before its declaration.

function greet(name) {
    console.log(`Hello, ${name}!`);
}
```

### 4. Definition: Function Declaration & Function Expression

__Function Declarations:__

A function declaration is a way to define a named function using the function keyword. This function has a specific name that you give it. One unique feature of function declarations is "hoisting." This means that you can call the function before you actually write its definition in your code. JavaScript will move the function declaration to the top of its scope during execution, making it accessible anywhere within that scope. Example:

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}
```

__Function Expressions:__

A function expression involves creating a function as part of an expression. This can make your code more flexible and allows you to assign functions to variables or use them as arguments for other functions (like callbacks). A function expression can be either anonymous (without a name) or named. When a function expression is anonymous, it's typically used when you only need to use the function in one place and don't plan on reusing it extensively. When it's named, you can refer to the function by its name within the expression and wherever the expression is used. Example:

```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

### 5. Function Scope

JavaScript has three main scopes: global, local/function and block scopes. Variables declared inside a function are only accessible within that function, creating a localized environment.

```javascript
function printMessage() {
    const message = "Hello!";
    console.log(message); // "Hello!" is printed.
}

console.log(message); // Throws an error, 'message' is not defined.
```

### 6. Regular Functions vs Arrow Functions

Regular functions and arrow functions are two ways to define functions in JavaScript, each with its own syntax and behavior. Here's a comparison between them:

__Regular Function:__

```javascript
function multiply(a, b) {
    return a * b;
}
```

__Arrow Function:__

```javascript
const multiply = (a, b) => a * b;
```

 __Differences:__

* Syntax: Regular functions use the function keyword followed by a name and parameter list. Arrow functions use a more concise syntax with parameters placed within parentheses, followed by an arrow (=>) and the function body.

 * __this__ binding: Regular functions adjust their this dynamically, depending on where they are called. Meanwhile, arrow functions skip creating their own this context and just take it from the surrounding code (the lexical scope) where they're defined. Let's check these commented examples:

```javascript
// 1 - Example with Regular function call:

// Defining the regular function.
function regularFunction() {
  console.log("'this' in regularFunction refers to the global object");
}

// Calling regularFunction directly.
regularFunction();

// Creating an object 'obj' with a method 'method' that points to regularFunction.
const obj = {
  method: regularFunction,
};
// Calling the method 'method' on 'obj'.
obj.method(); // 'this' in regularFunction now refers to the object 'obj' in this context

// 2 - Example with Arrow function call:

// Defining the arrow function.
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

To simplify: In arrow functions the value of __this__ is determined by where it is defined, not where it is used.

 __Arguments Object:__

Regular functions have access to the arguments object, which holds all arguments passed to the function.
Arrow functions do not have their own arguments object. They inherit the arguments from the enclosing scope.

 __Usage:__

Regular functions are suitable for methods in objects or functions that need their own this context.
Arrow functions are often used for concise one-liner functions, callbacks, and in scenarios where you want to preserve the surrounding this context.

 __Return Behavior__:

Regular functions allow more complex function bodies and have an explicit return statement.
Arrow functions with a single expression can have an implicit return, meaning the expression's value is automatically returned without needing a return keyword.

### 7. Anonymous Functions

Anonymous functions are functions without a name. They can be used directly where they're needed or assigned to variables.

```javascript
const square = function(x) {
    return x * x;
};

// Arrow functions provide a shorter syntax for anonymous functions.
const square = (x) => x * x;
```

### 8. Higher-Order Functions

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

### 9. Closures

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

### 10. Callbacks and Asynchronous JavaScript

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

### 11. Default Parameters and Rest Parameters (ES6 Features)

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

### 12. IIFE (Immediately Invoked Function Expression)

An IIFE is a function that is defined and executed immediately after its creation. It's often used to create a private scope and prevent variable name clashes.

```javascript
// Define and immediately invoke an IIFE (Immediately Invoked Function Expression)
(function() {
    const privateVar = "This is private";
})();

// Attempting to access privateVar outside the IIFE will result in an error
console.log(privateVar); // Error: privateVar is not defined
```

### 13. Recursion

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

### 14. Call, Apply, and Bind

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

### 15. Generator Functions

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

### 16. Promises and Async/Await

Promises and async/await are mechanisms for dealing with asynchronous code in a more organized and readable manner. 

__Promisse:__

```javascript
function fetchData() {
    return new Promise((resolve, reject) => {
        // Simulate fetching data from a server
        setTimeout(() => {
            const data = { name: "Alice", age: 25 };
            resolve(data);
        }, 1000);
    });
}

fetchData()
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

__async/await__:

```javascript
async function fetchDataAsync() {
    try {
        const data = await fetchData();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

fetchDataAsync();
```

### 17. Function Currying

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

### 18. Memoization

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

### 19. Map, Filter, and Reduce

These are higher-order functions commonly used for working with arrays in a functional programming style.

- __map__: Creates a new array by applying a function to each element of the original array.
- __filter__: Creates a new array with elements that pass a certain condition.
- __reduce__: Applies a function to reduce the array to a single value.

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(x => x * 2);
const even = numbers.filter(x => x % 2 === 0);
const sum = numbers.reduce((acc, val) => acc + val, 0);
```

### 20. Prototype and this Context

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

### 21. Closures and Data Privacy:

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

### 22. Closures and Functional Programming:

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
console.log(celsiusToFar(25)); // Outputs 77

const farToCelsius = temperatureConverter('Fahrenheit');
console.log(farToCelsius(98.6)); // Outputs 37
```

### 23. Closures and Event Handling:

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

### 24. Functions as first-class citizens:

The concept of "first-class citizens" is a fundamental principle in programming languages, including JavaScript. It refers to the idea that entities such as functions and variables are treated as equal citizens within the language, meaning they can be used, manipulated, and passed around just like any other data type. In simpler terms, in a programming language that treats entities as first-class citizens, you can do the following with functions and variables:

__Assign to Variables:__ You can assign functions to variables just like you would with numbers, strings, or other data types. This is often called "function assignment" or using functions as "first-class values."

__Pass as Arguments:__ You can pass functions as arguments to other functions. This is particularly useful when you want to create more dynamic and flexible code. Functions that accept other functions as arguments are often referred to as "higher-order functions."

__Return as Values:__ Functions can be returned as values from other functions. This enables you to create functions that generate and customize other functions on the fly.

__Store in Data Structures:__ Functions can be stored in data structures like arrays or objects, allowing you to organize and manage functions in a structured manner.

JavaScript is an example of a programming language that treats functions as first-class citizens. This feature is why JavaScript can be used for various programming paradigms, such as functional programming, where functions are used extensively to solve problems. 

### Conclusion:

Master JavaScript by mastering functions! They're core for reusing logic, and structuring projects. Learn parameters, returns, and scoping. Grasp declarations, expressions, and the differences between regular and arrow functions. Understand this behavior and higher-order functions. Use IIFE for private scopes. Functions unlock JavaScript's power. Dive in, experiment, and level up! Happy coding! 
