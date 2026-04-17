# Test 3 Study Guide

The third test of the course covers Module 3: Full-Stack Application Development. You can review the material on the following pages:

- [Full-Stack Application Development (Overview)](../module_3/overview.md)
- [Introduction to Express](../module_3/introduction_to_express.md)
- [Middleware](../module_3/middleware.md)
- [Express with MongoDB](../module_3/express_with_mongodb.md)
- [Putting It Together](../module_3/putting_it_together.md)

## Key Concepts

In addition to reviewing the content above, you should feel comfortable answering the following questions:

- What is Express, and what role does it play in a web application?
- What is a REST API? What does a client send, and what does the server respond with?
- What is the difference between `app.get()` and `app.post()`?
- What is the difference between `res.json()` and `res.send()`? Which do you use for APIs, and why?
- What is a route parameter? How do you define one, and how do you access its value?
- Why does route order matter in Express? What happens if a less specific route is defined before a more specific one?
- What is middleware? What does `express.json()` do, and what happens to `req.body` if you forget to register it?
- What is CORS? Why does the browser block requests from a frontend on one port to an API on a different port, and how does the `cors` middleware fix it?
- Why do you connect to MongoDB before calling `app.listen()` in an Express server?
- Why do route handler callbacks need to be `async` when they make database calls?
- Why should database calls in route handlers be wrapped in `try/catch`? What happens if you omit it?
- What is `ObjectId`, and why can't you query MongoDB by `_id` using a plain string? What should you check before constructing one from a URL parameter?
- Why is it important to `return` after sending an early error response in a route handler?
- What is the difference between a success response, a client error response, and a server error response? Given a scenario (resource created, invalid input, resource not found, unexpected server failure), which category does it fall into?
