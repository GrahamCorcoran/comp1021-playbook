# Introduction to Express

Express is a web framework for Node.js. It gives you a structured way to handle HTTP requests: define a URL, specify an HTTP method, and write a function that runs when that request comes in.

## Installing Express

Express is installed through npm like any other package:

```bash
npm install express
```

Once installed, you load it in your file with `require`:

```js
const express = require("express");
```

## Creating a Server

To create an Express application, call `express()`. Then tell it which port to listen on:

```js
const express = require("express");
const app = express();

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

Running this with `node server.js` starts a server on port 3000. It does not do anything yet because no routes are defined.

## Defining Routes

A route tells Express what to do when a request comes in for a specific URL and HTTP method. The two you will use most are `app.get()` and `app.post()`.

```js
app.get("/hello", (req, res) => {
    res.send("Hello!");
});
```

The first argument is the path. The second is a callback function that receives the request (`req`) and response (`res`) objects. When a `GET` request comes in for `/hello`, Express calls that function.

`res.send()` sends a plain text response. For APIs, you will typically use `res.json()` instead, which sets the correct `Content-Type` header and serializes a JavaScript value as JSON:

```js
app.get("/status", (req, res) => {
    res.json({ ok: true });
});
```

## The req and res Objects

`req` (request) contains information about the incoming request: the URL, any parameters, query strings, headers, and the body.

`res` (response) is how you send something back. A few methods you will use:

| Method | Description |
|--------|-------------|
| `res.json(value)` | Send a JSON response |
| `res.send(text)` | Send a plain text response |
| `res.status(code)` | Set the HTTP status code (chain with `.json()`) |

Setting a status code and sending JSON together looks like this:

```js
res.status(404).json({ error: "Not found" });
```

The default status code is `200 OK`. You only need to set it explicitly when it should be something else.

## Route Parameters

Routes can include dynamic segments called route parameters. You define them with a colon:

```js
app.get("/users/:id", (req, res) => {
    const id = req.params.id;
    res.json({ id: id });
});
```

A request to `/users/42` would set `req.params.id` to `"42"`. Parameters are always strings, so convert them if you need a different type.

## Route Order

Express matches routes in the order they are defined. The first route that matches the request gets called. Define more specific routes before less specific ones to avoid unexpected matches.

## A Complete Example

```js
const express = require("express");
const app = express();

app.get("/greet/:name", (req, res) => {
    res.json({ message: "Hello, " + req.params.name });
});

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

A `GET` request to `/greet/Priya` returns:

```json
{ "message": "Hello, Priya" }
```
