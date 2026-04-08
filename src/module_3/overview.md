# Module 3: Full-Stack Application Development

In this module, we bring together the JavaScript, Node.js, and MongoDB skills from the previous two modules to build a backend web API.

The tool for this is Express, a lightweight web framework for Node.js. Express handles the routing layer of your server: it listens for HTTP requests, matches them to handler functions, and sends back responses. Pair that with a MongoDB connection and you have a working backend that a frontend application can talk to.

## REST APIs

A REST API is a server that communicates over HTTP using a standard set of conventions. Clients make requests to specific URLs (called endpoints) using HTTP methods like `GET` and `POST`. The server processes each request and responds with JSON.

Your COMP 205 frontend will call your API using `fetch`. This is the backend it connects to.

## What We Will Cover

By the end of this module, you should be able to build an Express server with multiple endpoints, use middleware to parse request bodies, connect Express to a MongoDB database, and handle common error cases with appropriate HTTP status codes.
