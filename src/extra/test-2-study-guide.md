# Test 2 Study Guide

The second test of the course covers Module 2: Working with Databases. You can review the material on the following pages:

- [Working with Databases (Overview)](../module_2/overview.md)
- [Introduction to NoSQL](../module_2/introduction_to_nosql.md)
- [Introduction to MongoDB](../module_2/introduction_to_mongodb.md)
- [CRUD Operations](../module_2/crud_operations.md)
- [MongoDB with Node.js](../module_2/mongodb_with_nodejs.md)
- [Data Modelling and Queries](../module_2/data_modelling_and_queries.md)

## Key Concepts

In addition to reviewing the content above, you should feel comfortable answering the following questions:

- What is the difference between a SQL database and a NoSQL database?
- What are the three levels of organization in MongoDB (from largest to smallest)?
- What is an `ObjectId`, and what is the `_id` field used for?
- What is the difference between `find` and `findOne`? What does `findOne` return if nothing matches?
- What does a query filter do, and what happens when you pass multiple fields in a single filter?
- What does each of the following update operators do: `$set`, `$unset`, `$inc`, `$push`, `$pull`?
- What happens if you pass a plain object to `updateOne` without using an update operator like `$set`?
- Why do database operations in Node.js require `async` and `await`?
- What is the purpose of `try/catch/finally` when working with database code? Why is `finally` a good place to close the connection?
- How does `find()` behave differently in Node.js compared to the Mongo shell? What method do you call to get the results as an array?
- What is the difference between embedding and referencing when designing a document schema? Give an example of when each is appropriate.
- Write a query that finds all documents where a numeric field is greater than a given value.
- What is the difference between `$and` and `$or`? When do you need explicit `$and` versus relying on implicit `$and`?
- What do `sort`, `limit`, and `skip` do? How can they be combined for pagination?
- What is projection, and why would you use it?
