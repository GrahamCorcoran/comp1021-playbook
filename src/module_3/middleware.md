# Middleware

Middleware are functions that run between an incoming request and your route handler. They can inspect or modify the request, attach data to it, or send a response early. You register middleware with `app.use()`.

## express.json()

By default, Express does not parse the body of incoming requests. If a client sends a `POST` request with a JSON body, `req.body` will be `undefined` unless you tell Express to parse it.

`express.json()` is built-in middleware that reads the request body, parses it as JSON, and attaches the result to `req.body`:

```js
app.use(express.json());
```

This line goes near the top of your file, before your routes. With it in place, any `POST` request with a JSON body will have that data available on `req.body`:

```js
app.post("/echo", (req, res) => {
    console.log(req.body); // { name: "Tariq", age: 25 }
    res.json(req.body);
});
```

Without `app.use(express.json())`, `req.body` is `undefined` and reading from it will cause errors.

## CORS

Browsers block JavaScript from making requests to a different origin (domain, port, or protocol) than the page the script is loaded from. This is called the same-origin policy.

When a frontend application running at one origin (for example, `http://localhost:5173`) tries to call your Express server at a different origin (`http://localhost:3000`), the browser will block the request unless your server explicitly allows it.

CORS (Cross-Origin Resource Sharing) is the mechanism that allows this. You configure it on the server by sending specific response headers that tell the browser which origins are permitted.

The `cors` npm package handles this for you:

```bash
npm install cors
```

```js
const cors = require("cors");
app.use(cors());
```

With no arguments, `cors()` allows requests from any origin.

```admonish note title="Why CORS Matters for This Course"
The COMP 205 frontend runs on a different port than your Express server. Without CORS enabled, the browser will block every request the frontend makes to your API.
```

## Middleware Order

Middleware runs in the order you register it. Register `express.json()` and `cors()` before your routes so they apply to every incoming request:

```js
const express = require("express");
const cors = require("cors");

const app = express();

app.use(cors());
app.use(express.json());

app.post("/orders", (req, res) => {
    // req.body is parsed, CORS headers are set
    res.json({ received: req.body });
});

app.listen(3000);
```
