# The DOM & JavaScript in the Browser

Up to this point, we have been running JavaScript with Node in the terminal. In this chapter, we shift to running JavaScript in the browser, where it can interact with web pages directly.

## HTML Recap

HTML (HyperText Markup Language) is the language used to structure web pages. A basic HTML page looks like this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Page</title>
</head>
<body>
    <h1>Hello</h1>
    <p>This is a paragraph.</p>
</body>
</html>
```

HTML is covered in depth in [COMP 205](https://comp205.grahamcorcoran.ca/module_1/html-overview.html), which is the co-requisite for this course. If you need a refresher on HTML elements, attributes, or page structure, refer to the COMP 205 Playbook.

For this chapter, the key thing to understand is that HTML defines the structure of a page using elements, and those elements can be nested inside each other.

## The DOM

When a browser loads an HTML file, it does not work with the raw text directly. Instead, it parses the HTML and builds a tree structure in memory called the DOM (Document Object Model).

Each HTML element becomes a node in this tree. Elements nested inside other elements become child nodes. For example, given this HTML:

```html
<body>
    <h1>Welcome</h1>
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
</body>
```

The browser builds a tree that looks roughly like this:

```
body
├── h1
│   └── "Welcome"
├── ul
│   ├── li
│   │   └── "Item 1"
│   └── li
│       └── "Item 2"
```

JavaScript in the browser interacts with this tree. When you change an element's text, add a new element, or remove one, you are modifying the DOM. The browser then updates the page to reflect those changes.

## Adding JavaScript to a Page

To run JavaScript in a browser, you include it in your HTML file using the `<script>` tag.

### Inline Scripts

It is possible to write JavaScript directly inside a `<script>` tag in your HTML:

```html
<body>
    <h1>Hello</h1>
    <script>
        console.log("This runs in the browser");
    </script>
</body>
```

You may see this in online tutorials and examples. In this course, we do not use inline scripts. JavaScript should be kept in separate `.js` files. Inline scripts mix your logic into your HTML, which makes both harder to read and maintain.

### External Scripts

Instead of inline scripts, the standard approach is to keep your JavaScript in a separate file and reference it:

```html
<body>
    <h1>Hello</h1>
    <script src="app.js"></script>
</body>
```

```js
// app.js
console.log("This runs in the browser");
```

### Script Placement

Place your `<script>` tag at the bottom of the `<body>`, just before the closing `</body>` tag. This ensures the HTML elements on the page have been loaded before your JavaScript tries to access them. If your script runs before the elements exist, it will not be able to find them.

```html
<body>
    <h1>Hello</h1>
    <p id="greeting">Welcome to the site.</p>

    <!-- Scripts go here, after the content -->
    <script src="app.js"></script>
</body>
```

```admonish note title="Deferred Script Loading"
You may see scripts placed in the `<head>` of a page with a `defer` attribute:

    <head>
        <script src="app.js" defer></script>
    </head>

The `defer` attribute tells the browser to download the script immediately but wait to execute it until the HTML has finished loading. This achieves the same result as placing the script at the bottom of the body. Either approach is acceptable in this course.
```

## Selecting Elements

Before you can do anything with an element on the page, you need to select it. JavaScript provides several methods for this through the `document` object, which represents the entire DOM. The most common is `getElementById`, which selects a single element by its `id` attribute:

```html
<p id="message">Hello there.</p>
```

```js
let element = document.getElementById("message");
console.log(element.textContent); // "Hello there."
```

Each `id` should be unique on the page, so this always returns one element. Other selection methods like `querySelector` and `querySelectorAll` exist for more flexible lookups using CSS selectors, but `getElementById` is sufficient for most of what we will do in this course.

## Modifying Elements

Once you have selected an element, you can change its content. The `textContent` property gets or sets the text inside an element:

```html
<p id="status">Waiting...</p>
```

```js
let status = document.getElementById("status");
status.textContent = "Ready!";
```

The page will now display "Ready!" instead of "Waiting...". JavaScript can also modify an element's styles, attributes, and inner HTML, but for now `textContent` is the main one to be aware of.

## Event Handling

Events are things that happen on the page: a user clicks a button, types in a field, hovers over an element, or submits a form. JavaScript lets you listen for these events and run code in response.

### addEventListener

The standard way to handle events is with `addEventListener`. You call it on an element, pass in the event type and a function to run when the event occurs:

```html
<button id="myButton">Click me</button>
```

```js
let button = document.getElementById("myButton");

button.addEventListener("click", function() {
    console.log("Button was clicked!");
});
```

When the button is clicked, the function runs. This function is called an event handler.

### Common Events

| Event | Fires when... |
|-------|---------------|
| `click` | The element is clicked |
| `input` | The value of an input field changes |
| `submit` | A form is submitted |
| `keydown` | A key is pressed |
| `mouseover` | The cursor moves over the element |

### Using Event Data

Event handlers receive an event object with information about what happened. You access it by adding a parameter to your handler function:

```html
<input type="text" id="nameField" placeholder="Enter your name">
<p id="preview"></p>
```

```js
let input = document.getElementById("nameField");
let preview = document.getElementById("preview");

input.addEventListener("input", function(event) {
    preview.textContent = `You typed: ${event.target.value}`;
});
```

The `event.target` refers to the element that triggered the event. In this case, `event.target.value` gives you the current contents of the input field.

### Putting It Together

Here is a complete example that combines selecting, modifying, and event handling:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Counter</title>
</head>
<body>
    <h1>Counter</h1>
    <p id="count">0</p>
    <button id="increment">Add 1</button>

    <script src="counter.js"></script>
</body>
</html>
```

```js
// counter.js
let count = 0;
let display = document.getElementById("count");
let button = document.getElementById("increment");

button.addEventListener("click", function() {
    count = count + 1;
    display.textContent = count;
});
```

This page displays a number and a button. Each time the button is clicked, the number increases by one. The JavaScript selects the elements, listens for a click, updates the variable, and modifies the DOM to reflect the new value.
