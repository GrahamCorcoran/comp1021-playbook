# JavaScript and Runtimes

In this course we will be using JavaScript heavily, and while doing so we need to understand the difference between JavaScript the programming language, a JavaScript runtime, and the tools we use surrounding these concepts. 

## JavaScript
[JavaScript](https://en.wikipedia.org/wiki/JavaScript) is a programming language that was created to further expand the functionality of HTML and CSS in a web context. It was originally built to run inside web browsers, allowing developers to make web pages interactive.

JavaScript is a scripting language, meaning it is interpreted at the time it runs rather than compiled ahead of time like C++. You write the code, and a runtime reads and executes it directly.

An important distinction to keep in mind is that JavaScript is just the language. Where and how it runs depends on the runtime.

## Runtimes
A JavaScript *runtime* is the process that executes the JavaScript code, and handles the result. Since JavaScript is interpreted, it always needs a runtime to execute. The runtime is responsible for reading your code, understanding it, and producing the output.

There are two main runtimes we will use in this course: Node.js and web browsers. Both run JavaScript, but they provide different capabilities depending on the environment they operate in.

### Node

[Node.js](https://nodejs.org/) is a JavaScript runtime that runs outside of the browser. When you run `node lab3.js` in your terminal, Node is the program reading and executing your JavaScript code.

Under the hood, Node uses Google's [V8 engine](https://v8.dev/), which is the same JavaScript engine used in Google Chrome. V8 handles the actual interpretation and execution of your code. Node wraps V8 and adds functionality that would not be available in a browser, such as reading and writing files, accessing the operating system, and running a web server.

This is a key point: JavaScript in a browser is sandboxed. It can only interact with the web page it is running on. Node does not have this restriction. It runs on your machine with access to your file system, network, and other system resources. This is what makes it suitable for server-side development.

Node also comes with [npm](https://www.npmjs.com/) (Node Package Manager), which is a tool for installing and managing third-party libraries. When you need functionality that is not built into JavaScript or Node itself, npm allows you to pull in packages written by other developers. We will use npm regularly throughout this course.

Node takes JavaScript out of the browser and gives it the tools needed to build backend applications, scripts, and tooling.

### Web Browsers

Web browsers (Chrome, Firefox, Edge, Safari) are the other major JavaScript runtime. When a browser loads a web page that contains JavaScript, the browser's built-in engine executes that code. Chrome uses V8 (the same engine as Node), Firefox uses SpiderMonkey, and Safari uses JavaScriptCore.

Where Node provides access to the file system and operating system, browsers provide access to the web page itself. JavaScript running in a browser can read and modify the page content, respond to user interactions like clicks and key presses, and make network requests to load additional data.

As mentioned earlier, browser JavaScript is sandboxed. It cannot access files on the user's computer or interact with the operating system directly. This is intentional. When you visit a website, the JavaScript on that page should not be able to read your documents or install software. The browser enforces these restrictions for security.

#### HTML DOM

```admonish info title="HTML vs HTML DOM"
The HTML DOM and HTML itself are regularly confused with each other but are not exactly the same. HTML is the markup you write in your `.html` files. The DOM is the live, in-memory structure the browser creates from that HTML. The browser can also modify the DOM after the page loads (and so can JavaScript), meaning the DOM may not always match the original HTML source.
```

The DOM (Document Object Model) is the browser's representation of an HTML page. When a browser loads an HTML file, it parses the HTML and builds a structured model of the page in memory. This model is the DOM.

JavaScript in the browser interacts with the page through the DOM. Every HTML element on the page becomes an object in the DOM that JavaScript can access, modify, or remove. For example, JavaScript can change the text inside a `<p>` tag, add a new `<li>` to a list, or hide an element when a button is clicked.

The DOM is not part of the JavaScript language. It is an API provided by the browser. This is why DOM-related code like `document.getElementById()` works in a browser but not in Node. Node does not have a web page, so it has no DOM.