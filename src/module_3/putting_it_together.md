# Putting It Together

The previous pages covered each concept in isolation. This page shows what a complete Express + MongoDB server looks like when everything is assembled.

The server below handles three endpoints for a small bookstore: fetching all books, looking up a single book by ID, and creating a new book.

```js
const express = require("express");
const cors = require("cors");
const { MongoClient, ObjectId } = require("mongodb");

const app = express();
app.use(cors());
app.use(express.json());

const client = new MongoClient("mongodb://localhost:27017");
let books;

app.get("/books", async (req, res) => {
    try {
        const all = await books.find().toArray();
        res.json(all);
    } catch (err) {
        res.status(500).json({ error: "Database error" });
    }
});

app.get("/books/:id", async (req, res) => {
    if (!ObjectId.isValid(req.params.id)) {
        return res.status(400).json({ error: "Invalid book ID" });
    }

    try {
        const book = await books.findOne({ _id: new ObjectId(req.params.id) });

        if (!book) {
            return res.status(404).json({ error: "Book not found" });
        }

        res.json(book);
    } catch (err) {
        res.status(500).json({ error: "Database error" });
    }
});

app.post("/books", async (req, res) => {
    const { title, author } = req.body;

    if (!title || !author) {
        return res.status(400).json({ error: "title and author are required" });
    }

    try {
        const book = { title, author };
        const result = await books.insertOne(book);
        res.status(201).json({ ...book, _id: result.insertedId });
    } catch (err) {
        res.status(500).json({ error: "Database error" });
    }
});

async function start() {
    await client.connect();
    const db = client.db("bookstore");
    books = db.collection("books");
    app.listen(3000, () => {
        console.log("Server running on port 3000");
    });
}

start();
```

A few things worth noting about the overall structure:

- Routes are registered at the top level, outside `start()`. This keeps the startup logic separate from the route definitions.
- `books` is declared at the top level with `let` and assigned inside `start()` after the connection resolves. Because `app.listen()` is called after that assignment, the server does not accept requests until `books` is ready.
- Validation runs before the `try/catch`. A missing field is a client error (`400`), not a server error (`500`).
- Each early return on an error response prevents Express from trying to send a second response after the function continues.
