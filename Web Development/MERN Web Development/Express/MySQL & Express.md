[Medium](https://medium.com/@kirankumal714/creating-a-powerful-restful-api-with-node-js-mysql-and-express-a-step-by-step-guide-18cdad7c6e18)

In the world of web development, two technologies stand out for their immense popularity and versatility — MySQL and Node.js. MySQL is an open-source relational database management system, while Node.js is a powerful runtime environment built on Chrome’s V8 JavaScript engine. Together, they form a dynamic duo that empowers developers to build robust and scalable web applications with ease.

MySQL, with its proven track record and widespread adoption, serves as a trusted and efficient data storage solution. Its relational nature allows developers to define and manage structured data with ease, ensuring data integrity and consistency. MySQL has been a cornerstone of the web ecosystem for decades, providing the foundation for countless applications and websites worldwide.

Node.js, on the other hand, has taken the JavaScript language, traditionally known for front-end development, to the backend. With its event-driven, non-blocking I/O model, Node.js excels at handling concurrent connections, making it ideal for building real-time applications and APIs. Node.js allows developers to use a unified language and share code between the frontend and backend, streamlining the development process and fostering collaboration within development teams.

When these two powerhouses come together, magic happens. Node.js can seamlessly interact with MySQL, enabling developers to access, retrieve, and manipulate data with ease. The combination of Node.js’s asynchronous capabilities and MySQL’s performance and scalability make it a preferred choice for modern web applications that require real-time updates, responsiveness, and reliability.

In this article, we will delve into the symbiotic relationship between MySQL and Node.js, exploring how to set up a database connection, perform CRUD (Create, Read, Update, Delete) operations, and handle data efficiently. We will also touch upon best practices, security considerations, and tips to optimize the performance of MySQL and Node.js applications.

## Creating a Node.js application

Now, we’ll initialize a new node.js project with the command below:

```bash
mkdir nodejs-mysql-crud && cd nodejs-mysql-crud  
npm init
```

Next, we’ll install the dependencies that are require for our project.

```
npm install cors dotenv express mysql2  
npm install nodemon -D
```

- `cors`: This package enables Cross-Origin Resource Sharing, which allows a server to specify who can access its resources.
- `dotenv`: This package loads environment variables from a file named ".env" into the Node.js process, making it easy to manage configurations.
- `express`: This package is a popular web framework for Node.js, simplifying the process of building web applications and APIs.
- `mysql2`: This package provides a fast and efficient way to interact with MySQL databases in Node.js.
- `nodemon`: This package is a development tool that monitors your Node.js application for changes and automatically restarts the server when code changes are detected. It's very useful during the development process as you don't have to manually restart the server after each code modification.

## Folder Structure:

Finally, let’s look at our project structure. At the end of this tutorial, our project structure will look like this:

```
nodejs-mysql-crud/  
├─ node_modules/  
├─ src/  
│ ├─ db/  
│ │ ├─ connect.js  
│ ├─ errors/  
│ │ ├─ customError.js  
│ ├─ middlewares/  
│ │ ├─ handleError.js  
│ │ ├─ tryCatchWrapper.js  
│ │ ├─ notFound.js  
│ ├─ resources/  
│ │ ├─ notes/  
│ │ │ ├─ notes.controllers.js  
│ │ │ ├─ notes.routes.js  
├─ .gitignore  
├─ package.json  
├─ .env  
├─ README.md
```

## Setting up Expres Server

Create an app.js file and add the following code:

```javascript
import express from "express";  
import dotenv from "dotenv";  
import { notFound } from "./src/middlewares/notFound.js";  
import { handleError } from "./src/middlewares/handleError.js";  
import notesRoute from "./src/resources/notes/notes.routes.js";  
import cors from "cors";  
dotenv.config();  
  
const app = express();  
const port = process.env.PORT || 3000;  
  
const corsOptions = {  
  origin: "http://localhost:5173", // for vite application  
  optionsSuccessStatus: 200,  
};  
  
//middleware  
app.use(cors(corsOptions));  
app.use(express.json());  
  
// api routes  
app.use("/notes", notesRoute);  
  
app.use(notFound);  
app.use(handleError);  
  
app.listen(port, () => {  
  console.log(`server running on port ${port}`);  
});
```

- `app`: This variable creates an instance of the Express application, which allows you to define routes and middleware for handling requests.
- `port`: This variable specifies the port number the server will listen on. If the "PORT" environment variable is provided in the ".env" file, it will use that value; otherwise, it defaults to port 3000.
- `cors`: This middleware is used to enable CORS for requests coming from "[http://localhost:5173](http://localhost:5173/)" (a Vite application in this case). CORS allows the specified origin to access the server's resources.
- `express.json()`: This middleware parses incoming JSON data from the request body and makes it available on the `req.body` object.
- `notFound`: This middleware handles 404 responses, meaning that if no route matches the incoming request, this middleware will be triggered and send a proper 404 response.
- `handleError`: This middleware is responsible for handling errors that occur during the request-response cycle. It helps provide consistent error responses for various situations.

In summary, this code sets up an Express web server with CORS enabled for a specific origin and handles API routes related to managing notes. It also includes middleware for handling JSON data, 404 responses, and errors. The server will listen on the specified port, allowing clients to interact with the defined API routes.

## Setup MySQL

Now, we have our Express server set up. Let’s go ahead and set up our MySQL Database. I’ll be using phpMyAdmin for creating databases and tables. To run the lampp server you can use following command:

sudo /opt/lampp/lampp start

or else you can create the database and table using cmd also, but you must have mysql installed in your system.

CREATE DATABASE notes_app  
  
```sql
CREATE TABLE `notes` (  
  `id` int(11) NOT NULL,  
  `title` varchar(255) NOT NULL,  
  `contents` text NOT NULL,  
  `created` timestamp NOT NULL DEFAULT current_timestamp(),  
  PRIMARY KEY (`id`)  
);
```

Next, create **_.env_** file and add mysql config.

```dotenv
MYSQL_HOST="localhost"  
MYSQL_USER="root"  
MYSQL_PASSWORD=""  
MYSQL_DATABASE="notes_app"  
PORT = 5000
```

Then, create db folder and inside it create **_connect.js_** and add the following code.

```javascript
import mysql from "mysql2";  
import dotenv from "dotenv";  
dotenv.config();  
  
export const pool = mysql  
  .createPool({  
    host: process.env.MYSQL_HOST,  
    database: process.env.MYSQL_DATABASE,  
    user: process.env.MYSQL_USER,  
    password: process.env.MYSQL_PASSWORD,  
  })  
  .promise();
```

`mysql.createPool()`: This method is used to create a connection pool for MySQL database interactions. A connection pool is a collection of reusable database connections, and it helps improve performance by reusing connections rather than creating new ones for each request.

## Creating Controller

Now, let’s create the **CRUD** for note entity. create **_notes.controllers.js_** inside notes folder on resouces.

**Create:**

```javascript
/**  
 * @description Create note  
 * @route POST /notes  
 */  
export const createNote = tryCatchWrapper(async function (req, res, next) {  
  const { title, contents } = req.body;  
  
  if (!title || !contents)  
    return next(createCustomError("All fields are required", 400));  
  
  let sql = "INSERT INTO notes (title, contents) VALUES (?, ?)";  
  await pool.query(sql, [title, contents]);  
  
  return res.status(201).json({ message: "note has been created" });  
});
```

This `createNote` function handles the creation of a new note by extracting the necessary data from the request body, validating the input, executing an SQL query to insert the note into the database, and sending a response back to the client. It implements error handling using a custom error creation function and the try-catch wrapper to handle any errors that might occur during the database operation.

The question marks are placeholders used in prepared statements.In prepared statements, question marks act as placeholders for parameterized values that will be supplied later when executing the query. The values for these placeholders are provided separately from the query, which allows for better security against SQL injection attacks and improved performance

**Read:**
```javascript
/**  
 * @returns note object  
 */  
async function getNote(id) {  
  let sql = "SELECT * FROM notes WHERE id = ?";  
  const [rows] = await pool.query(sql, [id]);  
  return rows[0];  
}  
  
/**  
 * @description Get All note  
 * @route GET /notes  
 */  
export const getAllNotes = tryCatchWrapper(async function (req, res, next) {  
  let sql = "SELECT * from notes";  
  const [rows] = await pool.query(sql);  
  if (!rows.length) return res.status(204).json({ message: "empty list" });  
  
  return res.status(200).json({ notes: rows });  
});  
  
/**  
 * @description Get Single note  
 * @route GET /notes/:id  
 */  
export const getSingleNote = tryCatchWrapper(async function (req, res, next) {  
  const { id } = req.params;  
  
  const note = await getNote(id);  
  if (!note) return next(createCustomError("note not found", 404));  
  
  return res.status(200).json(note);  
});
```

1. `getNote(id)`: Retrieves a single note by its ID from the "notes" table in the database using a prepared SQL statement with a placeholder.
2. `getAllNotes`: Handles the request to retrieve all notes from the "notes" table, sending a response with the retrieved notes as a JSON object or a 204 status code if the list is empty.
3. `getSingleNote`: Handles the request to retrieve a single note by its ID from the "notes" table, calling `getNote(id)` and sending the retrieved note as a JSON object or a 404 status code if the note is not found.

Just like above, let’s create update and delete handler.

**Update:**
```javascript
/**  
 * @description Update note  
 * @route PATCH /notes/:id  
 */  
export const updateNote = tryCatchWrapper(async function (req, res, next) {  
  const { id } = req.params;  
  const { title, contents } = req.body;  
  
  if (!id || !title || !contents)  
    return next(createCustomError("All fields are required", 400));  
  
  const note = await getNote(id);  
  if (!note) return next(createCustomError("note not found", 404));  
  
  let sql = "UPDATE notes SET title = ? , contents = ? WHERE id = ?";  
  await pool.query(sql, [title, contents, id]);  
  
  return res.status(201).json({ message: "note has been updated" });  
});
```

**Delete:**

```javascript
/**  
 * @description Delete note  
 * @route DELETE /notes/:id  
 */  
export const deleteNote = tryCatchWrapper(async function (req, res, next) {  
  const { id } = req.params;  
  
  if (!id) return next(createCustomError("Id is required", 400));  
  
  const note = await getNote(id);  
  if (!note) return next(createCustomError("note not found", 404));  
  
  let sql = "DELETE FROM notes WHERE id = ?";  
  await pool.query(sql, [id]);  
  
  return res.status(200).json({ message: "note has been deleted" });  
});
```
Above controllers, implements error handling using a custom error creation function and the try-catch wrapper to handle any errors that might occur during the database operation. Let’s create it.

create **_customError.js_** inside errors folder.

```javascript
export class CustomError extends Error {  
  constructor(message, statusCode) {  
    super(message);  
    this.statusCode = statusCode;  
  }  
}  

export const createCustomError = (message, statusCode) => {  
  return new CustomError(message, statusCode);  
};
```
  
`CustomError`, which inherits from the built-in `Error` class in JavaScript. It also includes a utility function `createCustomError` to create instances of this custom error class easily.

Also, let’s create **_tryCatchWrapper.js_** inside middlewares.

```javascript
import { createCustomError } from "../errors/customErrors.js";  
  
export function tryCatchWrapper(func) {  
  return async (req, res, next) => {  
    try {  
      await func(req, res, next);  
    } catch (error) {  
      return next(createCustomError(error, 400));  
    }  
  };  
}
```

`tryCatchWrapper` function wraps other asynchronous functions (typically Express middleware) and adds error handling to them. If an error occurs during the execution of the wrapped function, it creates a custom error and passes it to the Express error-handling middleware for further processing and appropriate response to the client. This ensures that the application can handle errors more effectively and provide meaningful error responses to the users.

## Routes:

We have to create routes for the controllers. Now, let’s create **_notes.routes.js_** next to **notes.controllers.js**

```javascript
import express from "express";  
import {  
  createNote,  
  deleteNote,  
  getAllNotes,  
  getSingleNote,  
  updateNote,  
} from "./notes.controllers.js";  
  
const router = express.Router();  
  
router.route("/").get(getAllNotes).post(createNote);  
router.route("/:id").get(getSingleNote).patch(updateNote).delete(deleteNote);  
  
export default router;
```

This express router handles different HTTP routes related to note management. It defines routes for listing all notes (`GET /notes`), creating a new note (`POST /notes`), retrieving a single note by ID (`GET /notes/:id`), updating a note by ID (`PATCH /notes/:id`), and deleting a note by ID (`DELETE /notes/:id`).

That’s it , see how simple it is to create REST api using node and express js. Now you will be able to CRUD notes using any other client application or using postman. It will create and store data in mysql as shown below.