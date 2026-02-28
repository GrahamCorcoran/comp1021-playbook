# Introduction to JavaScript

JavaScript is the programming language we will use throughout this course. If you are coming from a language like C++, many of the core concepts will feel familiar. Variables, operators, and data types all exist in JavaScript, but the syntax and behaviour differ in some important ways.

JavaScript does not require you to declare types when creating variables. It uses dynamic typing, meaning the type of a variable is determined by the value it holds, and can change during execution.

## Variables

Variables in JavaScript are declared using one of three keywords: `let`, `const`, or `var`.

### let

`let` is the standard way to declare a variable in JavaScript. It creates a variable that can be reassigned.

```js
let score = 0;
score = 10;
```

Variables declared with `let` are block-scoped, meaning they only exist within the block (such as a function or an `if` statement) where they are declared.

### const

`const` declares a variable that cannot be reassigned after it is set. Use `const` when you have a value that should not change.

```js
const pi = 3.14159;
pi = 3; // This will cause an error
```

If a value should stay the same throughout your program, prefer `const` over `let`. It makes your code easier to read because anyone looking at it knows that value will not change.

```admonish info title="const and Mutability"
`const` prevents reassignment of the variable, but it does not make the value itself immutable. If the value is an array or an object, you can still modify its contents.

    const colours = ["red", "blue"];
    colours.push("green"); // This works
    colours = ["yellow"];   // This causes an error

The variable `colours` still points to the same array. You are changing what is inside the array, not replacing the array itself. This is why you will often see `const` used with arrays and objects in real-world code.
```

### var

`var` is the original way to declare variables in JavaScript. You will see it in older code and in many online examples.

```js
var name = "Fred";
```

In this course, we use `let` and `const` instead of `var`. The main reason is scoping: `var` is function-scoped rather than block-scoped, which can lead to unexpected behaviour. You do not need to understand the details of this difference right now, but if you see `var` in examples online, know that `let` is the modern replacement.

## Data Types

JavaScript has several built-in data types. The most common ones you will work with are:

| Type | Example | Description |
|------|---------|-------------|
| String | `"Hello"` | Text, wrapped in quotes |
| Number | `42`, `3.14` | Integers and decimals (no separate types) |
| Boolean | `true`, `false` | Logical values |
| Null | `null` | An intentionally empty value |
| Undefined | `undefined` | A variable that has been declared but not assigned a value |

Unlike C++, JavaScript does not distinguish between integers and floating-point numbers. Both `42` and `3.14` are just `Number`.

You can check the type of a value using `typeof`:

```js
let age = 25;
console.log(typeof age); // "number"

let name = "Jane";
console.log(typeof name); // "string"
```

## Operators

Most operators in JavaScript work the same way as in C++. Arithmetic operators (`+`, `-`, `*`, `/`, `%`) and assignment operators (`=`, `+=`, `-=`) behave as you would expect.

The key difference is with comparison operators. JavaScript has two types of equality check:

| Operator | Name | Description |
|----------|------|-------------|
| `==` | Loose equality | Compares values after converting types |
| `===` | Strict equality | Compares values and types without conversion |

```js
let a = 5;
let b = "5";

console.log(a == b);  // true (string "5" is converted to number 5)
console.log(a === b); // false (number and string are different types)
```

Use `===` (strict equality) by default. Loose equality can produce unexpected results because of the type conversion. The same applies to `!==` (strict not equal) over `!=` (loose not equal).

```admonish note title="Why doesn't C++ have loose equality?"
In C++, every variable has a type declared at compile time. If you compare an `int` to a `string`, the compiler will either reject it or you have to explicitly convert the type yourself. JavaScript is dynamically typed, so a variable can hold any type of value at any time. This means two values of completely different types can end up being compared, and JavaScript will try to convert them automatically with loose equality (`==`). Strict equality (`===`) avoids this by checking the type first and returning `false` immediately if the types do not match.
```

## Console Output

`console.log()` is the primary way to output information in JavaScript. It prints to the terminal when running with Node, or to the browser's developer console when running in a browser.

```js
console.log("Hello, world!");
```

You can output multiple values by separating them with commas:

```js
let course = "COMP1021";
let year = 2026;
console.log("Welcome to", course, year);
// Welcome to COMP1021 2026
```

You can also join strings together using the `+` operator:

```js
let name = "Graham";
console.log("Hello, " + name + "!");
// Hello, Graham!
```

The preferred way to include variables in strings is with template literals. Template literals use backticks (`` ` ``) instead of quotes, and variables are inserted using `${}`:

```js
let name = "Graham";
let course = "COMP1021";
console.log(`Hello, ${name}! Welcome to ${course}.`);
// Hello, Graham! Welcome to COMP1021.
```

Template literals are easier to read than string concatenation, especially when you have multiple variables. Basic concatenation with `+` still works and you will see it in plenty of code, but template literals are the modern standard.

## Comments

Comments in JavaScript work similarly to C++.

Single-line comments use `//`:

```js
// This is a comment
let x = 10; // This is also a comment
```

Multi-line comments use `/* */`:

```js
/*
  This comment spans
  multiple lines.
*/
```

Comments are ignored when the code runs. Use them to explain parts of your code that are not obvious from reading the code itself.
