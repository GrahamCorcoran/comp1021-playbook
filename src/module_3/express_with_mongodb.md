# Express with MongoDB

Connecting Express to MongoDB follows a pattern you have seen before: create a `MongoClient`, connect it, and then use it inside your route handlers. The difference from standalone scripts is that you connect once when the server starts and keep that connection open for the lifetime of the server.

## Setup

Install both packages if you have not already:

```bash
npm install express mongodb cors
```

## Connecting at Startup

Connect to MongoDB before starting the Express server. This way the connection is ready before any requests come in.

Declare the collection variables at the top level so your routes can reference them. Assign them inside `start()` after the connection resolves, then call `app.listen()`:

```js
const express = require("express");
const cors = require("cors");
const { MongoClient } = require("mongodb");

const app = express();
app.use(cors());
app.use(express.json());

const client = new MongoClient("mongodb://localhost:27017");
let menu, orders;

// routes go here

async function start() {
    await client.connect();

    const db = client.db("pizza_shop");
    menu = db.collection("menu");
    orders = db.collection("orders");

    app.listen(3000, () => {
        console.log("Server running on port 3000");
    });
}

start();
```

Routes are defined at the top level and reference `menu` and `orders` by name. Because `app.listen()` is called after the connection resolves, the server does not accept requests until the collections are assigned. By the time any request arrives, `menu` and `orders` are ready.

## Async Route Handlers

Database operations are asynchronous, so route handler callbacks need to be `async`:

```js
app.get("/menu", async (req, res) => {
    const items = await menu.find().toArray();
    res.json(items);
});
```

Always wrap database calls in `try/catch` so that errors do not crash the server:

```js
app.get("/menu", async (req, res) => {
    try {
        const items = await menu.find().toArray();
        res.json(items);
    } catch (err) {
        res.status(500).json({ error: "Database error" });
    }
});
```

Without it, an unhandled error leaves the request hanging with no response sent.

## ObjectId

MongoDB assigns a unique `_id` to each document when it is inserted. The type is `ObjectId`, not a plain string, which is why a direct string comparison does not work.

When a client sends an ID as a URL parameter (like `/orders/abc123`), it arrives as a string. To query MongoDB by `_id`, you need to convert it to an `ObjectId` first. Add `ObjectId` to your existing `require` line at the top of the file:

```js
const { MongoClient, ObjectId } = require("mongodb");
```

Before constructing the `ObjectId`, check that the string is valid. If the string is malformed, `new ObjectId()` throws an exception. That exception would be caught by the `catch` block and return a `500`, but a bad ID string is a client error, not a server error.

`ObjectId.isValid()` handles this check:

```js
app.get("/orders/:id", async (req, res) => {
    if (!ObjectId.isValid(req.params.id)) {
        return res.status(400).json({ error: "Invalid order ID" });
    }

    try {
        const order = await orders.findOne({ _id: new ObjectId(req.params.id) });

        if (!order) {
            return res.status(404).json({ error: "Order not found" });
        }

        res.json(order);
    } catch (err) {
        res.status(500).json({ error: "Database error" });
    }
});
```

`return` on each early response is important. Without it, Express continues executing the function and will try to send a second response, which causes an error.

## Inserting Documents

For `POST` endpoints, read from `req.body`, build the document, and use `insertOne`:

```js
app.post("/orders", async (req, res) => {
    const { customer, items } = req.body;

    if (!customer || !items || items.length === 0) {
        return res.status(400).json({ error: "customer and items are required" });
    }

    try {
        const order = {
            customer: customer,
            items: items,
            status: "pending"
        };

        const result = await orders.insertOne(order);
        res.status(201).json({ ...order, _id: result.insertedId });
    } catch (err) {
        res.status(500).json({ error: "Database error" });
    }
});
```

`201 Created` is the appropriate status code when a resource is successfully created. The client gets back the full order document including the `_id` that was assigned.

## HTTP Status Codes

| Code | Meaning | When to use |
|------|---------|-------------|
| `200` | OK | Successful GET or default success |
| `201` | Created | Resource was inserted |
| `400` | Bad Request | Invalid input from the client |
| `404` | Not Found | Requested resource does not exist |
| `500` | Server Error | Unexpected failure, usually a database error |

```admonish note title="Validation Comes First"
Put your input validation before the try/catch block. Validation errors (`400`) are not database errors and should not be lumped together with `500` responses.
```
