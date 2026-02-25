# Functions
Understanding functions is crucial for being able to work with JavaScript. In general, functions are a tool for re-using code. You will use these in all web development you do throughout your careers, as they are ubiquitous in every web development role.

# What is a JavaScript function?
A JavaScript function is a piece of re-usable code that will execute when called. They are broken down into function declarations, and function invoking (or calling). The following JavaScript function adds two numbers and returns the result:

```js
function add(num1, num2) {
  return num1 + num2;
}
```

# Components of a Function
A JavaScript function is made up of several key parts that work together. First, there's the function *name*, which is the identifier you use when calling the function. Then, inside the parentheses, you have *parameters*. These are placeholders for the values (known as arguments) that you pass into the function when you call it.

Next is the function *body*, the block of code inside curly braces {} that tells the function what to do. This is where you write the operations, calculations, or any logic the function should execute. Finally, the return statement is used to send a value back to wherever the function was called. This return value can then be used in other parts of your code.

In this example we used before:
```js
// add below is the function name
function add(num1, num2) { // num1 and num2 are both parameters
  // everything in here is the function body
  return num1 + num2; // this is the function return
}
```

# Other Types of Functions
## Callback Functions
A callback function is a function that is passed as an argument to another function and is executed once a specific task or event is completed. Callbacks allow you to be flexible with the type of functions you write, for example by allowing you to specify custom behaviors that should occur after an operation finishes. They are commonly used in handling events, performing asynchronous tasks like API requests, or executing code after a timer.

# Best Practices
1. Use descriptive names for your functions to clearly indicate their purpose, and keep each function focused on a single task.
2. Be consistent with your formatting to ensure readability and help improve clarity.
3. Incorporate proper error handling, and ensure your functions handle invalid input well if user input is a concern.