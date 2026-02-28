# Introduction to Node.js

[Node.js](https://nodejs.org/) is a tool that lets you run JavaScript outside of a web browser. Instead of needing an HTML page to execute your code, you can run JavaScript directly from your terminal. 

## Installing Node.js

To check if Node.js is installed, open a terminal and run:

```bash
node --version
```

If you see a version number, you're good to go. If not, follow the official installation guide at [https://nodejs.org/](https://nodejs.org/en/download). The LTS (Long Term Support) version is recommended.

Installing Node.js also installs npm, which is covered below.

## Running JavaScript with Node

To run a JavaScript file with Node, use the `node` command followed by the file name:

```bash
node myFile.js
```

Node reads the file, executes the JavaScript inside it, and outputs any results to the terminal. If your code includes `console.log()`, the output will appear in the terminal where you ran the command.

```js
let greeting = "Hello from Node";
console.log(greeting);
```

```bash
node myFile.js
Hello from Node
```

Unlike running JavaScript in a browser, there is no web page involved. Node executes the code and exits. This makes it useful for writing scripts, building tools, and running server-side applications.

## npm

npm (Node Package Manager) is a command-line tool that comes bundled with Node.js. It is used to manage packages, which are third-party libraries written by other developers that you can use in your own projects.

Most real-world projects rely on external packages rather than building everything from scratch. For example, if your application needs to connect to a database or run a web server, there are well-tested packages available that handle this for you. npm is how you find, install, and keep track of those packages.

### Initializing a Project

To start using npm in a project, you first initialize it:

```bash
npm init
```

This walks you through a series of prompts and creates a `package.json` file in your project folder. If you want to skip the prompts and accept the defaults, you can use:

```bash
npm init -y
```

The `package.json` file keeps track of your project's metadata and its dependencies. When your project uses external packages, they are recorded here so that anyone else working on the project knows what is required.

### Installing Packages

To install a package, use `npm install` followed by the package name:

```bash
npm install express
```

This does two things:

1. Downloads the package into a folder called `node_modules` in your project directory.
2. Adds the package to the `dependencies` list in your `package.json`.

You can then use the package in your code with `require()`, which loads the package so you can use it in your file:

```js
const express = require("express");
```

The `require()` function looks for the package by name inside your `node_modules` folder and returns it. You store the result in a variable (in this case `express`) and use that variable to access the package's functionality.

The `node_modules` folder can get large. You should not include it when submitting work or committing to git. Since your dependencies are listed in `package.json`, anyone can recreate the `node_modules` folder by running:

```bash
npm install
```

This reads `package.json` and downloads everything listed in it.
