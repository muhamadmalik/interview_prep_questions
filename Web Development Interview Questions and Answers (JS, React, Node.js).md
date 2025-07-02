### **Web Development Interview Questions and Answers (JS, React, Node.js)**
### **Part 1: Core JavaScript Concepts**

#### **1. Is JavaScript single-threaded or multi-threaded? Explain the Event Loop.**
JavaScript is a **single-threaded** language, meaning it has only one call stack and can execute only one piece of code at a time.

However, it handles asynchronous operations (like API calls or timers) without blocking the main thread through a concurrency model called the **Event Loop**.

**The Event Loop mechanism consists of:**
1.  **Call Stack:** Where JavaScript keeps track of function calls. When a function is called, it's pushed to the stack. When it returns, it's popped off.
2.  **Web APIs / Node.js APIs:** Asynchronous functions (like `setTimeout`, `fetch`) are handed off from the call stack to these browser/Node.js APIs to be handled in the background.
3.  **Callback Queue (or Task Queue):** When an asynchronous operation completes, its callback function is placed in the Callback Queue.
4.  **Event Loop:** The Event Loop's job is to constantly monitor the Call Stack and the Callback Queue. **If the Call Stack is empty**, it takes the first task from the queue and pushes it onto the Call Stack to be executed.

This model allows JavaScript to be **non-blocking**, ensuring that long-running operations don't freeze the user interface.

```javascript
console.log("First"); // 1. Pushed to stack, executed, popped.

setTimeout(() => {
  console.log("Third"); // 3. After 0ms, this callback is placed in the queue.
}, 0);

console.log("Second"); // 2. Pushed to stack, executed, popped.

// Event Loop waits for stack to be empty, then pushes the callback.
// Output: First, Second, Third
```

#### **2. What is the difference between `var`, `let`, and `const`? Explain Scope.**
*   **Scope:** Determines the accessibility of variables. In JavaScript, there are Global Scope, Function Scope, and Block Scope.
*   **`var`:**
    *   **Scope:** Function-scoped. It is only confined to the function it's declared in, not to blocks like `if` or `for`.
    *   **Hoisting:** Hoisted to the top of its scope and initialized with `undefined`.
*   **`let`:**
    *   **Scope:** Block-scoped (`{}`). It is confined to the nearest block.
    *   **Hoisting:** Hoisted, but not initialized (this is called the "Temporal Dead Zone"). Accessing it before declaration throws a `ReferenceError`.
*   **`const`:**
    *   **Scope:** Block-scoped, just like `let`.
    *   **Hoisting:** Same as `let`.
    *   **Reassignment:** Cannot be reassigned. For objects and arrays, this means the reference cannot change, but the contents of the object/array can be modified.

```
function scopeTest() {
  var x = 1;
  if (true) {
    var x = 2;  // This re-declares and overwrites the original x
    let y = 3;  // y is only available inside this block
    const z = 4;// z is only available inside this block
  }
  console.log(x); // Output: 2
  // console.log(y); // ReferenceError: y is not defined
}
```
**Rule of Thumb:** Use `const` by default, `let` if you need to reassign the variable, and avoid `var`.

#### **3. What is a Closure?**
A closure is a function that has access to the variables in its outer (enclosing) function's scope, even after the outer function has finished executing. It "remembers" its lexical scope.

**Use Cases:**
*   Creating private variables and methods.
*   Implementing function factories (functions that create other functions).
*   In callbacks where the callback function needs access to variables from the context where it was created.

```javascript
function createCounter() {
  let count = 0; // This variable is "closed over" by the inner function

  return function() {
    count++;
    console.log(count);
  };
}

const counter1 = createCounter(); // count for counter1 is 0
const counter2 = createCounter(); // count for counter2 is 0

counter1(); // Output: 1
counter1(); // Output: 2
counter2(); // Output: 1 (has its own separate scope)
```

#### **4. Difference between `==` and `===`?**
*   **`==` (Loose Equality):** Compares two values for equality *after* performing type coercion if the types are different.
*   **`===` (Strict Equality):** Compares two values for equality *without* performing type coercion. It checks for both same value and same type.

**Best Practice:** Always use `===` to avoid unexpected behavior from type coercion.

```javascript
console.log(5 == "5");   // true (string "5" is coerced to number 5)
console.log(5 === "5");  // false (number vs string)

console.log(0 == false); // true (boolean false is coerced to number 0)
console.log(0 === false);// false (number vs boolean)
```

#### **5. Pass by Value vs Pass by Reference?**
JavaScript always uses **pass by value**. However, the "value" for objects (including arrays and functions) is a **reference** to that object in memory.

*   **Primitive Types (String, Number, Boolean, etc.):** A copy of the value is passed. Modifying the parameter inside the function does not affect the original variable.
    ```javascript
    let a = 10;
    function change(val) { val = 20; }
    change(a);
    console.log(a); // Output: 10
    ```
*   **Objects (and Arrays):** A copy of the *reference* (the memory address) is passed. Modifying the properties of the object inside the function *will* affect the original object because both variables point to the same object. However, reassigning the parameter to a new object will not affect the original.
    ```javascript
    let obj = { name: "Alice" };
    function update(o) {
      o.name = "Bob"; // This modifies the original object
    }
    update(obj);
    console.log(obj.name); // Output: "Bob"
    ```

---

### **Part 2: Web Fundamentals (HTTP, APIs, HTML, CSS)**

#### **6. What are the main HTTP methods? (GET, POST, PUT, PATCH, DELETE)**

| Method  | Description                                        | Idempotent? | Safe? | Use Case                                |
| :------ | :------------------------------------------------- | :---------- | :---- | :-------------------------------------- |
| **GET** | Retrieves data from a server.                      | Yes         | Yes   | Fetch a user profile, read a blog post. |
| **POST**| Submits new data to the server, often creating a new resource. | No          | No    | Create a new user, post a comment.      |
| **PUT** | Replaces an existing resource entirely with new data. If the resource doesn't exist, it may be created. | Yes         | No    | Update a user's entire profile.         |
| **PATCH** | Applies partial modifications to an existing resource. | No          | No    | Update only a user's email address.     |
| **DELETE**| Deletes a specified resource.                    | Yes         | No    | Delete a user account.                  |
*   **Safe:** The method does not change the state of the server.
*   **Idempotent:** Making the same request multiple times produces the same result as making it once.

#### **7. What are RESTful APIs?**
**REST (Representational State Transfer)** is an architectural style for designing networked applications. A **RESTful API** is an API that adheres to the constraints of REST.

**Key Principles:**
1.  **Client-Server Architecture:** Separation of concerns between the client (UI) and server (data storage).
2.  **Statelessness:** Each request from a client to a server must contain all the information needed to understand and complete the request. The server does not store any client context between requests.
3.  **Cacheability:** Responses must define themselves as cacheable or not, to prevent clients from reusing stale data.
4.  **Uniform Interface:** This is the most important constraint, involving:
    *   **Resource-Based:** Resources are identified by URIs (e.g., `/users/123`).
    *   **Manipulation Through Representations:** Clients manipulate resources via their representations (commonly JSON).
    *   **Standard HTTP Methods:** Use standard methods (GET, POST, PUT, DELETE) for actions.

#### **8. What is CORS (Cross-Origin Resource Sharing)?**
CORS is a browser security mechanism that allows or restricts web applications running at one origin (domain) from requesting resources from another origin. By default, browsers enforce the **Same-Origin Policy**, which blocks these cross-origin requests.

CORS works by having the server send back specific HTTP headers (like `Access-Control-Allow-Origin: *`) that tell the browser it's okay for the web page from the requesting origin to access the response.

#### **9. What is JSON? Why is it used?**
**JSON (JavaScript Object Notation)** is a lightweight, text-based data-interchange format. It is easy for humans to read and write and easy for machines to parse and generate.

**Why it's used:**
*   **Language Independent:** Although derived from JavaScript, it's a common format used by virtually all programming languages.
*   **Human-Readable:** Its structure is simple and clear.
*   **Standard API Format:** It has become the de-facto standard for data transfer in RESTful APIs.

#### **10. Difference between `id`, `class`, and `inline` styles in CSS?**
This relates to **CSS Specificity**, which determines which style rule is applied by the browser.

*   **`id`:**
    *   **Selector:** `#my-id`
    *   **Uniqueness:** An `id` must be unique per page.
    *   **Specificity:** Highest specificity among the three.
*   **`class`:**
    *   **Selector:** `.my-class`
    *   **Uniqueness:** A `class` can be used on multiple elements.
    *   **Specificity:** Medium specificity.
*   **`inline` styles:**
    *   **Syntax:** `<div style="color: blue;">`
    *   **Specificity:** Highest of all. It will override styles from `id`s and `class`es.
    *   **Usage:** Generally discouraged because it mixes presentation with content and is hard to maintain.

**Specificity Hierarchy (Highest to Lowest):** Inline Styles > ID > Class > Element Tag.

---

### **Part 3: React**

#### **11. What are the main features of React?**
1.  **Component-Based Architecture:** UI is broken down into reusable, independent pieces called components.
2.  **JSX (JavaScript XML):** A syntax extension that allows you to write HTML-like code in your JavaScript, making component rendering more intuitive.
3.  **Virtual DOM:** React keeps a lightweight representation of the actual DOM in memory. When state changes, it updates the Virtual DOM, compares it with a snapshot of the previous Virtual DOM, and then efficiently updates only the changed parts of the real DOM. This minimizes direct DOM manipulation, which is slow.
4.  **Unidirectional Data Flow:** Data flows in one direction, from parent components to child components (via props), making the application state more predictable and easier to debug.

#### **12. What is the difference between State and Props?**

| Feature      | **Props (Properties)**                                         | **State**                                                      |
| :----------- | :------------------------------------------------------------- | :------------------------------------------------------------- |
| **Purpose**  | To pass data from a parent component to a child component.     | To manage data that is internal to a component and changes over time. |
| **Mutability**| **Immutable**. Props are read-only and cannot be changed by the child component. | **Mutable**. It can be changed by the component itself using `useState`. |
| **Ownership**| Owned and controlled by the parent component.                | Owned and controlled by the component itself.                  |
| **Data Flow**| Part of the unidirectional data flow (Parent -> Child).        | Internal to the component. Changes to state trigger a re-render. |

#### **13. What are React Hooks? Explain `useState` and `useEffect`.**
**Hooks** are functions that let you "hook into" React state and lifecycle features from function components. They allow you to use state and other React features without writing a class.

*   **`useState`:**
    *   **Purpose:** To add state to a function component.
    *   **How it works:** It returns an array with two elements: the current state value and a function to update it. Calling the update function will trigger a re-render of the component.
    ```jsx
    import React, { useState } from 'react';

    function Counter() {
      const [count, setCount] = useState(0); // Initial state is 0
      return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
    }
    ```

*   **`useEffect`:**
    *   **Purpose:** To perform **side effects** in function components. Side effects are operations that affect something outside the component, like fetching data from an API, setting up a subscription, or manually changing the DOM.
    *   **How it works:** It takes a function that contains the side-effect code. It also takes an optional **dependency array**. The effect will run after every render if the dependency array is omitted, only on the first render if it's an empty array `[]`, and only when a value in the dependency array changes.
    ```jsx
    import React, { useState, useEffect } from 'react';

    function UserData({ userId }) {
      const [user, setUser] = useState(null);

      useEffect(() => {
        // This effect runs whenever 'userId' changes
        fetch(`https://api.example.com/users/${userId}`)
          .then(res => res.json())
          .then(data => setUser(data));
      }, [userId]); // Dependency array

      return user ? <div>{user.name}</div> : <div>Loading...</div>;
    }
    ```

#### **14. What is the Context API? When would you use it over props?**
The **Context API** provides a way to pass data through the component tree without having to pass props down manually at every level.

**Problem it solves:** "Prop drilling," which is the cumbersome process of passing props through many intermediate components that don't actually need the data themselves, just to get it to a deeply nested child.

**When to use it:**
*   For managing global state that many components need, such as theme (dark/light mode), user authentication status, or language preference.
*   When you find yourself passing the same prop through more than 2-3 levels of components.

**Avoid using it for all state management**, as it can make component reuse more difficult. For complex state, a dedicated library like Redux might be a better fit.

#### **15. How do you submit a form in React (and send data to a DB)?**
This is a multi-step process involving the client (React) and the server (Node.js/Express).

**Step 1: Create the Form in React**
*   Manage the form's input values using the `useState` hook.
*   Create an `onSubmit` handler for the form.

```jsx
import React, { useState } from 'react';

function MyForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = async (event) => {
    event.preventDefault(); // Prevent the default form submission behavior

    // Step 2: Send the data to the server API
    try {
      const response = await fetch('/api/users', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ name, email }),
      });
      const data = await response.json();
      console.log('Success:', data);
    } catch (error) {
      console.error('Error:', error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={name} onChange={e => setName(e.target.value)} />
      <input type="email" value={email} onChange={e => setEmail(e.target.value)} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Step 3: Create the Server-Side API Endpoint (e.g., in Express)**
*   The server listens for the `POST` request, parses the JSON body, and interacts with the database.

```javascript
// In your server.js (using Express)
const express = require('express');
const app = express();
app.use(express.json()); // Middleware to parse JSON bodies

// Assume 'db' is your configured database connection object
app.post('/api/users', async (req, res) => {
  const { name, email } = req.body;
  try {
    // Step 4: Interact with the database
    const newUser = await db.query('INSERT INTO users (name, email) VALUES ($1, $2) RETURNING *', [name, email]);
    res.status(201).json(newUser.rows[0]);
  } catch (err) {
    res.status(500).json({ error: 'Database error' });
  }
});
```

---

### **Part 4: Node.js and Express**

#### **16. Why is Node.js single-threaded? How does it handle multiple requests?**
Node.js uses a **single-threaded event loop model**. The main reason is to simplify programming and avoid the complexities of concurrent programming (like deadlocks).

It handles multiple requests efficiently through **non-blocking I/O (Input/Output)**.
1.  When a request arrives that involves an I/O operation (like reading a file or querying a database), Node.js does **not** wait for it to complete.
2.  Instead, it delegates the operation to the underlying system's kernel (managed by a C++ library called `libuv`, which uses a thread pool).
3.  Node.js immediately starts processing the next request.
4.  When the I/O operation is finished, the kernel informs Node.js, which then places the corresponding callback function into the event queue to be executed by the event loop when the call stack is free.

This makes Node.js highly scalable and efficient for I/O-intensive applications (like web servers, APIs), but less suitable for CPU-intensive tasks (like video encoding), which would block the single main thread.

#### **17. What is Middleware in Express?**
**Middleware** functions are functions that have access to the `request` object (`req`), the `response` object (`res`), and the `next` function in the application's request-response cycle.

They can:
*   Execute any code.
*   Make changes to the request and the response objects.
*   End the request-response cycle.
*   Call the next middleware in the stack using `next()`. If `next()` is not called, the request will be left hanging.

**Common Middleware Use Cases:**
*   `express.json()`: To parse incoming JSON request bodies.
*   `cors`: To enable Cross-Origin Resource Sharing.
*   **Authentication:** To check if a user is logged in before allowing access to a route.
*   **Logging:** To log details of every incoming request.

```javascript
const express = require('express');
const app = express();

// A simple logger middleware
const loggerMiddleware = (req, res, next) => {
  console.log(`${req.method} ${req.path} - ${new Date().toISOString()}`);
  next(); // Pass control to the next middleware/route handler
};

app.use(loggerMiddleware); // Apply middleware to all routes

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(3000);
```

---

### **Part 5: Other Technologies**

#### **18. What are Docker images and containers?**
*   **Docker Image:** A lightweight, standalone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files. It is an immutable **blueprint**.
*   **Docker Container:** A running **instance** of a Docker image. You can create, start, stop, move, and delete containers. They are isolated from each other and from the host machine, providing a consistent environment from development to production.

**Analogy:** An image is like a class in OOP, and a container is like an object (an instance of that class).