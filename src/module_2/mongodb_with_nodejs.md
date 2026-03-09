# MongoDB with Node.js

So far we have been working with MongoDB through the shell. In a real application, your code needs to connect to the database, run operations, and handle the results programmatically. The MongoDB Node.js driver lets you do this from JavaScript.

## The MongoDB Node.js Driver

The official MongoDB driver is an npm package called `mongodb`. To use it in a project, initialize a project with npm and install the package:

```bash
npm init -y
npm install mongodb
```

This adds the `mongodb` package to your `node_modules` folder and records it in your `package.json`.

## Async and Await

Before we connect to MongoDB, we need to cover a JavaScript concept that has not come up yet in this course: asynchronous code.

Most of the code you have written so far runs line by line, top to bottom. Each line finishes before the next one starts. But some operations take time. Connecting to a database, reading a file, or making a network request all involve waiting for something external to respond. JavaScript does not stop and wait by default. If you call a function that takes time, JavaScript moves on to the next line immediately, even if the result is not ready yet.

`async` and `await` give you a way to handle this. When you put `await` in front of a function call, JavaScript pauses that function until the operation finishes and gives you the result. Without `await`, you would get back a Promise object (a placeholder for a future value) instead of the actual data.

```js
// Without await: result is a Promise, not the actual document
const result = collection.findOne({ name: "Anika" });
console.log(result); // Promise { <pending> }

// With await: JavaScript waits for the query to finish
const result = await collection.findOne({ name: "Anika" });
console.log(result); // { _id: ..., name: "Anika", ... }
```

There is one rule: `await` can only be used inside a function marked with `async`. This is how JavaScript knows the function contains asynchronous code.

```js
async function main() {
    const result = await collection.findOne({ name: "Anika" });
    console.log(result);
}

main();
```

If you try to use `await` outside an `async` function, you will get a syntax error.

You saw this pattern in Lab 5. The entire lab was wrapped in an `async function main()` with `await` on every database call. That is the standard structure for scripts that talk to a database, and we will keep using it throughout this module.

## Connecting to a Database

To connect to MongoDB from Node.js, you create a `MongoClient` and call `connect()`:

```js
const { MongoClient } = require("mongodb");

async function main() {
    const uri = "mongodb://localhost:27017";
    const client = new MongoClient(uri);

    await client.connect();

    const db = client.db("myDatabase");
    const collection = db.collection("myCollection");

    // ... do work here ...

    await client.close();
}

main();
```

`MongoClient` is imported from the `mongodb` package using destructuring. The `uri` is the connection string for your local MongoDB instance. After connecting, you select a database with `client.db()` and a collection with `db.collection()`. Both are created automatically if they do not exist yet.

## Running CRUD Operations

The CRUD methods you used in the shell work the same way through the driver. The only difference is that each method returns a Promise, so you need `await`.

```js
// Insert
const result = await collection.insertOne({
    name: "Yuki",
    role: "developer",
    experience: 3
});
console.log("Inserted:", result.insertedId);

// Find
const person = await collection.findOne({ name: "Yuki" });
console.log(person);

// Update
await collection.updateOne(
    { name: "Yuki" },
    { $set: { experience: 4 } }
);

// Delete
await collection.deleteOne({ name: "Yuki" });
```

The method names, filters, and update operators are all identical to what you have been using in `mongosh`.

```admonish note title="find() Returns a Cursor"
In the shell, `find()` prints results directly. In Node.js, `find()` returns a cursor, which is a pointer to the result set. To get the documents as an array, call `.toArray()` on the cursor:

    const developers = await collection.find({ role: "developer" }).toArray();
```

## Error Handling

Database operations can fail. The server might not be running, a query might be malformed, or the connection might drop. Wrap your database code in a `try/catch/finally` block to handle these cases:

```js
const { MongoClient } = require("mongodb");

async function main() {
    const uri = "mongodb://localhost:27017";
    const client = new MongoClient(uri);

    try {
        await client.connect();

        const db = client.db("shop");
        const products = db.collection("products");

        await products.insertOne({
            name: "Notebook",
            price: 4.99,
            inStock: true
        });

        const item = await products.findOne({ name: "Notebook" });
        console.log(item);
    } catch (err) {
        console.log("Error:", err);
    } finally {
        await client.close();
    }
}

main();
```

The `try` block contains your database operations. If anything goes wrong (connection failure, invalid query, server timeout), execution jumps to the `catch` block. The `finally` block runs regardless of success or failure, making it the right place to close the connection. If you forget to close connections, your application may run out of available connections or hang when exiting.
