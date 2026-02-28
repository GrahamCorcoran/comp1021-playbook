# Functions in JavaScript

Functions are a way to group reusable code together. Instead of writing the same logic multiple times, you write it once inside a function and call it whenever you need it.

If you have written functions in C++, the concept is the same. The syntax is different, but the idea of defining a block of code that accepts input and produces output carries over directly.

## Declaring Functions

A function in JavaScript is declared using the `function` keyword, followed by a name, parentheses, and a block of code inside curly braces.

```js
function greet() {
    console.log("Hello!");
}
```

To run the code inside the function, you call it by name with parentheses:

```js
greet(); // prints "Hello!"
```

A function will not run until it is called. The declaration on its own does nothing.

Here is a more complete example with the components labeled:

```js
// "add" is the function name
function add(num1, num2) { // num1 and num2 are parameters
    // everything in here is the function body
    return num1 + num2; // the return statement sends a value back
}

let result = add(3, 4); // 3 and 4 are arguments
console.log(result); // 7
```

## Parameters and Arguments

Parameters are the variables listed in a function's declaration. Arguments are the actual values you pass in when calling the function.

```js
function multiply(a, b) { // a and b are parameters
    return a * b;
}

multiply(5, 3); // 5 and 3 are arguments
```

You can have as many parameters as you need, or none at all. If a function is called with fewer arguments than it has parameters, the missing ones will be `undefined`.

```js
function introduce(name, age) {
    console.log(`My name is ${name} and I am ${age} years old.`);
}

introduce("Priya"); // My name is Priya and I am undefined years old.
```

```admonish warning title="Note the Missing Argument Above"
It's worth highlighting the above code snippet. Unlike most other languages, JavaScript will not stop you from calling a function with the wrong number of arguments. Missing parameters silently become `undefined`, which can lead to unexpected results like `NaN` (Not a Number) when used in calculations. If something isn't working and you can't figure out why, check that you're passing all the expected arguments.
```

## Return Values

The `return` keyword sends a value back from a function to wherever it was called. You can store this value in a variable or use it directly.

```js
function square(n) {
    return n * n;
}

let result = square(6);
console.log(result); // 36
console.log(square(4)); // 16
```

A function can only return once. When a `return` statement is reached, the function stops executing immediately. Any code after the `return` will not run.

```js
function check(value) {
    if (value > 10) {
        return "big";
    }
    return "small";
    console.log("this will never run");
}
```

If a function does not have a `return` statement, it returns `undefined` by default.

## Scope

Scope determines where a variable is accessible in your code. In the Introduction to JavaScript chapter, we mentioned that `let` and `const` are block-scoped. Functions create their own scope.

Variables declared inside a function are only accessible within that function:

```js
function calculateTotal(price, tax) {
    let total = price + (price * tax);
    return total;
}

calculateTotal(50, 0.13);
console.log(total); // Error: total is not defined
```

The variable `total` exists inside `calculateTotal` and cannot be accessed outside of it.

Variables declared outside of a function are accessible inside it:

```js
let taxRate = 0.13;

function calculateTotal(price) {
    return price + (price * taxRate);
}

console.log(calculateTotal(50)); // 56.5
```

This works, but relying on variables from outside a function can make your code harder to follow. Where possible, pass values in as parameters so the function is self-contained.
