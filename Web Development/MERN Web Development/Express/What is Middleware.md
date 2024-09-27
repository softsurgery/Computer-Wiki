[Medium](https://medium.com/@arorashivansh2661992/middleware-in-node-js-ed4eee917ff0)

Middleware in Node.js refers to a concept where functions can be used to process incoming requests before they reach their final destination and handle outgoing responses before they are sent back to the client. These functions sit in between the initial request and the final response, hence the term “middleware.”
# **How Does Node.js Middleware Pattern Work?**

In Node.js, middleware functions are essentially functions that have access to the request object (`req`), the response object (`res`), and the `next` function in the application's request-response cycle.

When a request is made to the server, it passes through a series of middleware functions before reaching the final route handler or endpoint.

Each middleware function can perform its task and either pass the request to the next middleware function using the `next` function or terminate the request-response cycle by sending a response.

# **What happens when the request reaches the last middleware in the chain ?**

1. The request passes through all middleware functions in the chain.
2. If none of the middleware functions send a response back to the client (i.e., they all call the `next()` function to pass control to the next middleware or route handler):

- Express looks for a matching route handler based on the request URL and HTTP method.
- If a matching route handler is found, it is executed.
- If no matching route handler is found, Express sends a default “Not Found” response back to the client with a status code of 404.

# What is next() ?

In Express.js, `next()` is a function that is used within middleware functions to pass control to the next middleware function in the chain. When `next()` is called within a middleware function, Express moves to the next middleware function defined in the application.

When a middleware function is defined, it typically receives three arguments: `req` (the request object), `res` (the response object), and `next` (the next middleware function in the chain). By calling `next()` within a middleware function, the control is passed to the subsequent middleware function.

# **Some common used Middleware**

* **cors :** Enable cross-origin resource sharing (CORS) with various options.
* **cookie-parser :** Parse cookie header and populate req.cookies.
* **morgan :** HTTP requests logger.
* **multer :** Handle multi-part form data.

# **Middleware Chaining**

Generally, a set of middlewares are chained to form a set of functions that execute one after the other in **order**.

The next() function is called at the end of every middleware to pass the control to the next middleware. The last middleware function sends back the response to the client. Hence, different middleware process the request before the response is sent back.

```javascript
(req, res, next) => {  
    // body of middleware  
    next();  
}
```
