Of course. Here are the answers to the questions you provided, organized in the same categorized sequence as the list.

***

### **JavaScript (Core Concepts)**

#### **1. What are the differences between `var`, `let`, and `const`?**

`var`, `let`, and `const` are used to declare variables, but they differ in scope, hoisting, and re-assignability.

| Feature           | `var`                                              | `let`                                            | `const`                                          |
| :---------------- | :------------------------------------------------- | :----------------------------------------------- | :----------------------------------------------- |
| **Scope**         | Function-scoped                                    | Block-scoped (`{}`)                              | Block-scoped (`{}`)                              |
| **Hoisting**      | Hoisted and initialized with `undefined`           | Hoisted but not initialized (Temporal Dead Zone) | Hoisted but not initialized (Temporal Dead Zone) |
| **Re-assignment** | Can be re-declared and re-assigned                 | Cannot be re-declared, can be re-assigned        | Cannot be re-declared or re-assigned             |
| **Global Object** | Creates a property on the global object (`window`) | Does not create a property on the global object  | Does not create a property on the global object  |

**Example:**
```javascript
// Scope example
if (true) {
  var x = 10; // Accessible outside the if block
  let y = 20; // Only accessible inside this block
  const z = 30; // Only accessible inside this block
}
console.log(x); // 10
// console.log(y); // ReferenceError: y is not defined
// console.log(z); // ReferenceError: z is not defined

// Re-assignment example
let myLet = 'hello';
myLet = 'world'; // OK

const myConst = 'hello';
// myConst = 'world'; // TypeError: Assignment to constant variable.
```

#### **2. Explain scoping in JavaScript (Global, Function, Block).**

*   **Global Scope:** A variable declared outside any function or block (`{}`) is in the global scope. It is accessible from anywhere in your code.
*   **Function Scope:** A variable declared with `var` inside a function is only accessible within that function.
*   **Block Scope:** A variable declared with `let` or `const` inside a block (e.g., an `if` statement or a `for` loop) is only accessible within that block. This was introduced in ES6.

#### **3. What is lexical scoping?**

Lexical Scoping (also called Static Scoping) means that the scope of a variable is determined by its position in the source code at the time of writing, not by where the function is called. A nested function has access to variables declared in its outer (parent) function's scope. This is the mechanism that makes closures possible.

#### **4. What is hoisting in JavaScript?**

Hoisting is JavaScript's default behavior of moving all declarations (for variables and functions) to the top of their current scope before code execution.
*   **`var`:** Declarations are hoisted and initialized with `undefined`. You can access them before the line they are declared, and their value will be `undefined`.
*   **`let` and `const`:** Declarations are hoisted but not initialized. Accessing them before the declaration results in a `ReferenceError`. This is known as the "Temporal Dead Zone" (TDZ).
*   **Functions:** Function declarations are fully hoisted (both name and body), so you can call a function before it's defined in the code.

#### **5. What is the difference between a normal function and an ES6 arrow function?**

*   **`this` Binding:** This is the biggest difference.
    *   **Normal Functions:** Have their own `this` context, which is determined by how the function is called (e.g., `window`, the object, `undefined` in strict mode).
    *   **Arrow Functions:** Do not have their own `this`. They lexically inherit `this` from their parent scope.
*   **`arguments` Object:** Normal functions have a built-in `arguments` object (an array-like list of arguments). Arrow functions do not; you must use rest parameters (`...args`) instead.
*   **Constructor:** Normal functions can be used as constructors with the `new` keyword. Arrow functions cannot.
*   **Syntax:** Arrow functions offer a more concise syntax.

#### **6. What are closures in JavaScript?**

A closure is a function that remembers the environment in which it was created. It has access to its own scope, the outer function's scope, and the global scope, even after the outer function has finished executing.

**Use Case:** Data privacy and creating private variables.
```javascript
function createCounter() {
  let count = 0; // This variable is private to the closure
  return function() {
    count++;
    return count;
  };
}

const counter1 = createCounter();
console.log(counter1()); // 1
console.log(counter1()); // 2
```

#### **7. What is the difference between `==` and `===`?**

*   **`==` (Loose Equality):** Compares two values for equality *after* performing type coercion (converting values to a common type).
*   **`===` (Strict Equality):** Compares two values for equality *without* performing type coercion. Both the value and the type must be the same.

**Example:**
```javascript
5 == '5';   // true (string '5' is coerced to number 5)
5 === '5';  // false (number is not the same type as string)
```

#### **8. What is the difference between `null` and `undefined`?**

*   **`undefined`:** Means a variable has been declared but has not yet been assigned a value. It's the default value.
*   **`null`:** Is an assignment value. It represents the intentional absence of any object value. It must be explicitly assigned by a developer.

#### **9. Explain primitive vs. non-primitive (reference) data types.**

*   **Primitive Types:** Are immutable and passed by **value**. When you assign a primitive to another variable, a copy of the value is made. Types include `string`, `number`, `bigint`, `boolean`, `undefined`, `symbol`, and `null`.
*   **Non-Primitive (Reference) Types:** Are mutable and passed by **reference**. When you assign a non-primitive to another variable, you are copying a reference (the memory address) to the original object. Any changes to the new variable will affect the original. Types include `Object`, `Array`, and `Function`.

#### **10. Explain the `window` object vs. the `document` object.**

*   **`window`:** The top-level global object in a browser environment. It represents the browser window or tab and contains all global JavaScript variables, functions, and other objects like `location`, `history`, and `document`.
*   **`document`:** A property of the `window` object (`window.document`). It represents the Document Object Model (DOM) of the current web page. It's the entry point to the page's content, allowing you to manipulate the HTML and CSS.

#### **11. How do you perform a deep copy of an object in JavaScript?**

A deep copy creates a completely new object, duplicating all nested objects and arrays, so there are no shared references.
*   **Simple (but limited) Method:** `JSON.parse(JSON.stringify(obj))`
    *   **Pros:** Easy to use for objects containing JSON-safe data (strings, numbers, booleans, arrays, nested objects).
    *   **Cons:** Fails with `undefined`, functions, `Symbol`, etc., which are either lost or converted incorrectly.
*   **Robust Method:** Use a library like **Lodash** and its `_.cloneDeep()` method, which handles all data types correctly.

#### **12. Explain the spread and rest operators.**

Both use the `...` syntax but for opposite purposes.
*   **Spread Operator:** "Spreads out" the elements of an iterable (like an array or string) or the properties of an object. It's used for creating copies or combining arrays/objects.
    ```javascript
    const arr1 = [1, 2];
    const arr2 = [...arr1, 3, 4]; // [1, 2, 3, 4]
    ```
*   **Rest Operator:** "Gathers" the rest of the elements into a single array. It's used in function parameters to collect an indefinite number of arguments.
    ```javascript
    function sum(...numbers) { // Gathers all arguments into the 'numbers' array
      return numbers.reduce((total, num) => total + num, 0);
    }
    sum(1, 2, 3); // 6
    ```

#### **13. Explain the `map`, `filter`, and `forEach` array methods.**

All three iterate over an array, but they have different purposes and return values. All have a time complexity of O(n).
*   **`forEach()`:** Executes a provided function once for each array element. It does **not** return a new array; it returns `undefined`. Use it when you need to perform an action for each element without creating a new array.
*   **`map()`:** Creates a **new array** populated with the results of calling a provided function on every element in the calling array. Use it when you want to transform each element into something new.
*   **`filter()`:** Creates a **new array** with all elements that pass the test implemented by the provided function. Use it when you want to select a subset of elements.

#### **14. Is it a valid operation to initialize a variable as a string, then assign it an integer, and then a boolean value? Why?**

Yes, this is a valid operation. JavaScript is a **dynamically-typed** (or loosely-typed) language. This means you do not have to declare the data type of a variable, and the type can change at runtime as you re-assign new values to it.

***

### **Asynchronous JavaScript**

#### **1. Is JavaScript synchronous or asynchronous by nature? Justify your answer.**

JavaScript is **synchronous, single-threaded, and blocking** by nature. This means it can only execute one command at a time. However, the runtime environment (like a browser or Node.js) provides Web APIs/C++ APIs that allow JavaScript to handle asynchronous operations (like `setTimeout`, API calls, file I/O) in a non-blocking way, using the Event Loop.

#### **2. How does JavaScript handle asynchronous operations?**

It uses the **Event Loop** model provided by the runtime environment.
1.  When an async operation (e.g., `fetch()`) is called, it's handed off to a Web API/C++ API.
2.  JavaScript continues executing the rest of the synchronous code without waiting.
3.  Once the async operation is complete, its callback function is placed in the **Callback Queue** (for `setTimeout`) or **Microtask Queue** (for Promises).
4.  The **Event Loop** constantly checks if the **Call Stack** is empty. If it is, it pushes the first function from the queue (Microtasks have priority) onto the Call Stack to be executed.

#### **3. What is the Event Loop?**

The Event Loop is a mechanism that allows Node.js or the Browser to perform non-blocking I/O operations despite JavaScript being single-threaded. Its job is to monitor the Call Stack and the Task Queues (Microtask and Macrotask/Callback). If the Call Stack is empty, it takes the first event from a queue and pushes it onto the stack for execution.

#### **4. What are callbacks? What is "callback hell"?**

*   **Callback:** A function passed as an argument to another function, which is then executed after the outer function has completed its task. They are a fundamental way to handle asynchronous results.
*   **Callback Hell (Pyramid of Doom):** When you have multiple nested callbacks to handle sequential asynchronous operations. This leads to code that is hard to read, debug, and maintain due to its deeply nested structure.

#### **5. What are Promises? Why were they introduced if we already had callbacks?**

A **Promise** is an object representing the eventual completion (or failure) of an asynchronous operation. A promise can be in one of three states:
1.  **Pending:** The initial state; neither fulfilled nor rejected.
2.  **Fulfilled:** The operation completed successfully.
3.  **Rejected:** The operation failed.

They were introduced to solve the problems of **Callback Hell**. Promises allow for cleaner, more readable code by enabling method chaining (`.then()`, `.catch()`, `.finally()`) and better error handling.

#### **6. What is the difference between `async/await` and Promises?**

`async/await` is syntactic sugar built on top of Promises. It makes asynchronous code look and behave more like synchronous code, which significantly improves readability and maintainability.
*   **Promises (`.then()`):** Use chaining to handle results.
    ```javascript
    fetch(url)
      .then(response => response.json())
      .then(data => console.log(data))
      .catch(error => console.error(error));
    ```
*   **`async/await`:** Uses the `async` keyword to define the function and `await` to pause execution until a Promise resolves. Error handling is done with standard `try...catch` blocks.
    ```javascript
    async function fetchData() {
      try {
        const response = await fetch(url);
        const data = await response.json();
        console.log(data);
      } catch (error) {
        console.error(error);
      }
    }
    ```

#### **7. Can we create a synchronous function using Promises?**

No. This is a common misconception. `await` only pauses the execution *within the `async` function itself*. It does not block the main JavaScript thread. The overall program remains non-blocking. A Promise is, by definition, an asynchronous construct.

#### **8. What is `defer` in the context of script tags?**

`defer` is a boolean attribute for the `<script>` tag. It tells the browser to download the script file in parallel with parsing the HTML, but to **defer** the execution of the script until after the HTML document has been fully parsed. This ensures the script doesn't block page rendering and that the DOM is available when the script runs.

#### **9. How do you make an HTTP request using AJAX?**

While AJAX originally used `XMLHttpRequest`, modern JavaScript almost always uses the **`fetch()` API**.
```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json(); // Parses the JSON body text
  })
  .then(data => {
    console.log(data); // Here's your data
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });
```

#### **10. Explain error handling in asynchronous JavaScript.**

*   **Callbacks:** Use the "error-first" pattern, where the first argument to the callback is an error object (`(err, data) => {}`).
*   **Promises:** Use the `.catch()` method at the end of a promise chain to handle any rejection that occurs.
*   **`async/await`:** Use a standard `try...catch` block, which is often considered the most readable and intuitive method.

***

### **React.js**

#### **1. Why choose React? What are its main advantages?**

*   **Component-Based Architecture:** Encourages building reusable UI components, making code modular and easier to manage.
*   **Virtual DOM:** Provides high performance by minimizing direct manipulation of the slow real DOM. It calculates the most efficient way to update the UI.
*   **Declarative UI:** You describe *what* the UI should look like for a given state, and React handles the *how* of updating the DOM. This makes code more predictable.
*   **Large Ecosystem & Community:** A vast collection of libraries, tools (like Redux, React Router), and strong community support.
*   **Flexibility:** React is a library, not a framework, so it can be integrated into existing projects or used with various other libraries for routing, state management, etc.

#### **2. What is the difference between a library (like React) and a framework (like Angular)?**

*   **Library (React):** A collection of tools you can use to solve a specific problem (in this case, building UIs). You are in control and decide how to structure your application. You call the library's code.
*   **Framework (Angular):** Provides a complete, opinionated structure for your application. It dictates the architecture and calls your code (Inversion of Control). It's a "blueprint" for your app.

#### **3. What are the main features of React?**

JSX, Virtual DOM, Component-Based Architecture, One-way Data Flow (from parent to child), and Hooks.

#### **4. What is JSX?**

JSX (JavaScript XML) is a syntax extension for JavaScript. It allows you to write HTML-like code inside your JavaScript files. It is not valid JavaScript and must be transpiled (e.g., by Babel) into regular `React.createElement()` function calls.

#### **5. What is the Virtual DOM and how does it work?**

The Virtual DOM (VDOM) is a programming concept where a virtual representation of a UI is kept in memory and synced with the "real" DOM.
1.  When a component's state changes, React creates a new VDOM tree.
2.  React then compares this new tree with the previous VDOM tree (a process called "diffing").
3.  It calculates the minimal set of changes needed to bring the real DOM to the new state.
4.  These changes are then "batched" and applied to the real DOM in one go, which is much faster than re-rendering the entire DOM.

#### **6. Explain React components (functional and class-based).**

*   **Class Components:** ES6 classes that extend `React.Component`. They manage state with `this.state` and use lifecycle methods like `componentDidMount`.
*   **Functional Components:** Simple JavaScript functions that accept `props` and return JSX. With the introduction of Hooks, they can now manage state (`useState`) and handle side effects (`useEffect`), making them the modern standard for writing React components.

#### **7. What are props and state? What is the difference between them?**

*   **State:**
    *   Internal data managed **within** a component.
    *   It is mutable (can be changed using a setter function like `setState`).
    *   A change in state causes the component to re-render.
*   **Props (Properties):**
    *   External data passed **into** a component from its parent.
    *   They are read-only (immutable) within the child component.
    *   A change in a parent's props will cause the child component to re-render.

In short: **State** is owned and managed by the component itself. **Props** are passed to the component.

#### **8. What are React Hooks? Name some hooks you have used.**

Hooks are functions that let you "hook into" React state and lifecycle features from functional components.
Commonly used hooks include:
*   **`useState`:** To add state to a component.
*   **`useEffect`:** To perform side effects (like API calls).
*   **`useContext`:** To subscribe to React context without introducing nesting.
*   **`useRef`:** To access DOM elements directly or store a mutable value that doesn't trigger a re-render.
*   **`useMemo` & `useCallback`:** To optimize performance by memoizing values and functions.

#### **9. Explain the `useState` hook.**

`useState` is a hook that allows you to add state to a functional component. It returns an array containing two elements:
1.  The current state value.
2.  A function to update that state value.

```javascript
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // Initial state is 0
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </button>
  );
}
```

#### **10. Explain the `useEffect` hook.**

`useEffect` is a hook for performing side effects in functional components. It runs after every render by default. You can control when it runs by providing a dependency array:
*   **No dependency array `useEffect(() => {})`:** Runs after every single render.
*   **Empty dependency array `useEffect(() => {}, [])`:** Runs only **once** after the initial render (like `componentDidMount`).
*   **With dependencies `useEffect(() => {}, [prop, state])`:** Runs after the initial render and any time a value in the dependency array changes.

#### **11. What are side effects in React?**

A side effect is any operation that affects something outside the scope of the current function's execution. In React, this includes:
*   Fetching data from an API.
*   Setting up subscriptions (e.g., with WebSockets).
*   Manually changing the DOM (e.g., to set a document title).

#### **12. Explain the React component lifecycle methods.**

In class components, these methods are called at different stages of a component's life.
*   **Mounting:** `constructor()`, `render()`, `componentDidMount()`
*   **Updating:** `render()`, `componentDidUpdate()`
*   **Unmounting:** `componentWillUnmount()`

In functional components, `useEffect` covers most of these use cases:
*   `componentDidMount` -> `useEffect(() => {}, [])`
*   `componentDidUpdate` -> `useEffect(() => {}, [dependency])`
*   `componentWillUnmount` -> The return function from `useEffect(() => { return () => { /* cleanup */ } }, [])`

#### **13. What is prop drilling? How can you avoid it?**

**Prop drilling** is the process of passing props down through multiple nested components that do not need the props themselves, just to get them to a deeply nested child component that does.

You can avoid it using:
1.  **React Context API:** Creates a global-like state accessible by any component within its Provider tree.
2.  **State Management Libraries (like Redux):** Provides a centralized store for your application's state.

#### **14. What is the Context API? When would you use it over props?**

The Context API provides a way to pass data through the component tree without having to pass props down manually at every level. You use it for "global" data that many components in your app need, such as:
*   User authentication status
*   Theme (e.g., light/dark mode)
*   Preferred language

#### **15. Explain controlled vs. uncontrolled components.**

This refers to how form data is handled.
*   **Controlled Component:** An input form element whose value is controlled by React state. The state is the "single source of truth."
    ```jsx
    const [name, setName] = useState('');
    <input type="text" value={name} onChange={e => setName(e.target.value)} />
    ```
*   **Uncontrolled Component:** The form data is handled by the DOM itself. You typically use a `ref` to get the value when you need it (e.g., on form submission).

#### **16. What are Higher-Order Components (HOCs)?**

A Higher-Order Component is a function that takes a component and returns a new component, usually with some additional props or logic. It's an advanced pattern for reusing component logic.

#### **17. What are synthetic events in React?**

React wraps the browser's native event object into a `SyntheticEvent`. This provides a cross-browser consistent API, meaning events work identically across all browsers.

#### **18. How do you handle forms in React?**

You typically use **controlled components**.
1.  Create a state variable for each form input using `useState`.
2.  Set the `value` of the input to the state variable.
3.  Create an `onChange` handler that calls the state setter function to update the value as the user types.
4.  Create an `onSubmit` handler on the `<form>` element to process the data (e.g., send it to an API).

#### **19. Explain routing in React.**

Since React is a single-page application (SPA) library, it doesn't have built-in routing. We use a third-party library like **React Router**. It allows you to synchronize the UI with the URL by rendering different components for different URL paths, without causing a full page refresh.

#### **20. What is the latest version of `react-router-dom`?**

As of late 2023, the current major version is **v6**. It introduced significant API changes from v5, such as a more intuitive `Routes` and `Route` system, the `useNavigate` hook, and automatic route ranking.

***

### **Redux**

#### **1. What is Redux? Why do we need it?**

Redux is a predictable state container for JavaScript apps. It provides a centralized "store" for all the state in your application, making state changes predictable and traceable. You need it for large-scale applications where state is shared among many components, or when state management becomes too complex for props and Context API alone.

#### **2. When would you choose Redux over the Context API?**

*   **Context API:** Best for low-frequency updates and simple "global" state like theme or user info.
*   **Redux:** Best for complex, high-frequency state updates, when you need robust developer tools for debugging (like time-travel debugging), and a powerful middleware ecosystem (for handling API calls, logging, etc.).

#### **3. Explain the core concepts of Redux: Store, Actions, Reducers, and Dispatcher.**

*   **Store:** A single JavaScript object that holds the entire state of your application. It's the "single source of truth."
*   **Action:** A plain JavaScript object that describes *what happened*. It must have a `type` property (e.g., `{ type: 'INCREMENT_COUNTER' }`).
*   **Reducer:** A pure function that takes the current `state` and an `action` as arguments and returns the **new state**. It describes *how* the state changes in response to an action.
*   **Dispatch:** The method used to send an action to the Redux store (`store.dispatch(action)`). This is the only way to trigger a state change.

#### **44. Explain the Redux data flow.**

1.  An event occurs in the UI (e.g., a button click).
2.  The UI dispatches an **action**.
3.  The **reducer** function receives the current state and the action, and calculates the new state.
4.  The Redux **store** is updated with the new state returned by the reducer.
5.  The UI, subscribed to the store, re-renders to reflect the new state.

#### **5. How is state managed in Redux?**

State in Redux is **immutable**. Reducers must not modify the existing state object directly. Instead, they must create a copy of the state, make changes to the copy, and return the new object.

#### **6. What is Redux middleware? Explain Redux Thunk or Redux Saga.**

**Middleware** provides a third-party extension point between dispatching an action and the moment it reaches the reducer. It's used for logging, crash reporting, and handling asynchronous actions.
*   **Redux Thunk:** A middleware that allows you to write action creators that return a **function** instead of an action object. This function receives `dispatch` and `getState` as arguments and can be used to perform async logic (like an API call) and dispatch actions when it's complete.
*   **Redux Saga:** A more advanced middleware that uses ES6 Generators to make async flows easier to manage, test, and handle complex scenarios like concurrent tasks.

***

### **Node.js & Express.js**

#### **1. What is Node.js?**

Node.js is a back-end JavaScript **runtime environment**. It allows you to run JavaScript code on the server, outside of a web browser. It's built on Chrome's V8 JavaScript engine.

#### **2. Is Node.js a framework or a language?**

Neither. It's a **runtime environment**. The language is JavaScript; frameworks like Express.js run on top of the Node.js environment.

#### **3. What are the main advantages of Node.js?**

*   **Asynchronous & Non-blocking:** Its event-driven architecture makes it highly efficient for I/O-intensive applications (like web servers, chat apps).
*   **JavaScript Everywhere:** Allows developers to use a single language (JavaScript) for both front-end and back-end development.
*   **Large Ecosystem:** NPM (Node Package Manager) is the largest software registry in the world.
*   **Fast Execution:** Built on Chrome's V8 engine, which compiles JavaScript into native machine code.

#### **4. For what kind of tasks is Node.js best suited? When should you not use it?**

*   **Best for:** I/O-bound applications like real-time chat apps, streaming services, microservices, and API servers.
*   **Not for:** CPU-intensive tasks like heavy image processing, video encoding, or complex scientific calculations. Because it's single-threaded, a long-running CPU task will block the entire event loop.

#### **5. Node.js is single-threaded. How does it handle concurrency and avoid being blocked?**

Node.js uses an **event-driven, non-blocking I/O model**.
*   The main **Event Loop** runs on a single thread.
*   When an I/O operation (like reading a file or a database query) is requested, Node.js delegates that task to a **worker pool** (which uses multiple threads under the hood, managed by the `libuv` library).
*   The main thread is now free to handle other requests.
*   When the I/O task is complete, it places a callback in the event queue, and the Event Loop picks it up to execute.

#### **6. What is Express.js? What are its main features?**

Express.js is a minimal and flexible Node.js web application **framework**. It provides a robust set of features to build web and mobile applications.
*   **Features:** Routing, middleware support, template engine integration, and simplified HTTP request/response handling.

#### **7. What is middleware in the context of Express?**

Middleware functions are functions that have access to the request object (`req`), the response object (`res`), and the `next` function in the application’s request-response cycle. They can:
*   Execute any code.
*   Make changes to the request and the response objects.
*   End the request-response cycle.
*   Call the next middleware in the stack (`next()`).

Examples: Logging, authentication, parsing JSON bodies (`express.json()`).

#### **8. Explain the request-response lifecycle in an Express application.**

1.  An HTTP request hits the server.
2.  Express creates `req` and `res` objects.
3.  The request is passed through a series of **middleware** functions in the order they are defined.
4.  Each middleware can process the request or pass it to the next one using `next()`.
5.  Eventually, a **route handler** processes the request and sends a response back to the client using methods like `res.send()`, `res.json()`, or `res.render()`. This ends the cycle.

#### **9. What are Express Routers?**

`express.Router` is a mini Express application. It's a middleware that can be used for grouping route handlers. It helps organize your application's routes into modular, mountable files.

***

### **APIs & HTTP**

#### **1. What is an API? What are the advantages of using an API?**

An **API (Application Programming Interface)** is a set of rules and protocols that allows different software applications to communicate with each other.
*   **Advantages:**
    *   **Modularity:** Allows separation of concerns (e.g., front-end and back-end).
    *   **Reusability:** A single back-end API can serve multiple clients (web, mobile, desktop).
    *   **Abstraction:** Hides complex implementation details from the client.

#### **2. What is the difference between REST and SOAP APIs?**

| Feature | REST (Representational State Transfer) | SOAP (Simple Object Access Protocol) |
| :--- | :--- | :--- |
| **Architecture** | Architectural style | Protocol with strict rules |
| **Data Format** | Flexible (JSON, XML, text) - JSON is standard | Primarily XML |
| **Transport** | Primarily HTTP/HTTPS | Any transport (HTTP, SMTP, etc.) |
| **State** | Stateless (server holds no client state) | Can be stateful |
| **Performance** | Generally lighter and faster due to JSON | More verbose and slower due to XML envelope |

#### **3. What makes an API "RESTful"?**

A RESTful API adheres to the constraints of the REST architectural style. Key constraints include:
*   **Client-Server Architecture:** Separation of concerns.
*   **Statelessness:** Each request from a client must contain all the information needed to understand and process the request. The server does not store any client context.
*   **Cacheability:** Responses must define themselves as cacheable or not.
*   **Uniform Interface:** Using standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`), resources (`/users`), and representations (JSON).

#### **4. Explain the difference between GET, POST, PUT, PATCH, and DELETE.**

*   **GET:** Retrieves data from a server. Idempotent (calling it multiple times has the same effect).
*   **POST:** Submits new data to a server to create a resource. Not idempotent.
*   **PUT:** Updates an existing resource completely. If the resource doesn't exist, it may create it. Idempotent.
*   **PATCH:** Partially updates an existing resource. Not necessarily idempotent.
*   **DELETE:** Removes a resource. Idempotent.

#### **5. For a login form, should you use GET or POST? Why?**

You must use **POST**.
*   **Security:** POST sends data in the request body, not in the URL. GET appends data to the URL, which is visible in browser history, server logs, and to anyone looking over your shoulder.
*   **Data Length:** URLs have a length limit, which can be an issue for GET requests. POST requests have no such limit on the body size.
*   **Idempotency:** GET requests should be idempotent and safe (no side effects). A login attempt is not a safe, idempotent operation.

#### **6. Why don't we use POST for all types of requests?**

Because it violates the principles of a **Uniform Interface** (a REST constraint). Using the correct HTTP verb makes the API self-descriptive and predictable.
*   **GET** for retrieval is cacheable by browsers and proxies.
*   **PUT** and **DELETE** are idempotent, which allows for safe retries.
*   Using specific verbs clearly communicates the intent of the operation.

#### **7. Can you perform GET, PUT, and DELETE operations using only POST?**

Yes, you can. This is a common workaround for older clients or proxies that only support GET and POST. You would typically use a special header like `X-HTTP-Method-Override: PUT` in your POST request to tell the server what the real intent is. However, this is considered a hack and should be avoided in modern APIs.

#### **8. What are HTTP status codes?**

HTTP status codes are standard responses given by a server to indicate the outcome of a client's request.
*   **1xx (Informational):** Request received, continuing process.
*   **2xx (Success):** The action was successfully received, understood, and accepted. (e.g., `200 OK`, `201 Created`).
*   **3xx (Redirection):** Further action must be taken to complete the request. (e.g., `301 Moved Permanently`).
*   **4xx (Client Error):** The request contains bad syntax or cannot be fulfilled. (e.g., `400 Bad Request`, `401 Unauthorized`, `404 Not Found`).
*   **5xx (Server Error):** The server failed to fulfill a valid request. (e.g., `500 Internal Server Error`).

#### **9. What are headers in an HTTP request?**

Headers are key-value pairs that pass additional information between the client and the server. Examples include:
*   `Content-Type: application/json` (tells the server the body is JSON).
*   `Authorization: Bearer <token>` (sends an authentication token).
*   `Accept: application/json` (tells the server what format the client wants back).

#### **10. What is the difference between HTTP and HTTPS?**

*   **HTTP (HyperText Transfer Protocol):** The standard protocol for transferring data over the web. Data is sent in plain text, making it vulnerable to interception.
*   **HTTPS (HTTP Secure):** The secure version of HTTP. It encrypts the data using TLS/SSL, ensuring confidentiality and integrity between the client and server.
*   **Default Ports:** HTTP uses port **80**. HTTPS uses port **443**.

#### **11. What is an API endpoint?**

An endpoint is a specific URL where an API can be accessed. It represents a specific resource or function. For example, in the API `https://api.example.com/users/123`, `/users/123` is the endpoint.

#### **12. How do you test an API?**

API testing can be done manually or with automated tools.
*   **Manual Tools:** Postman, Insomnia. They allow you to construct and send HTTP requests and inspect the responses.
*   **Automated Testing:** Using testing frameworks like Jest or Mocha (with libraries like Supertest for Node.js) to write test scripts that make API calls and assert that the responses (status code, body, headers) are correct.

***

### **Web Concepts & Security**

#### **1. What is the difference between a web application and a desktop application?**

*   **Web Application:** Runs on a web server and is accessed through a web browser. It does not require installation on the user's machine. It's platform-independent.
*   **Desktop Application:** Installed and runs directly on a specific operating system (Windows, macOS). It has direct access to the machine's hardware and file system.

#### **2. What is the MVC (Model-View-Controller) architecture?**

MVC is a design pattern that separates an application into three interconnected components:
*   **Model:** Manages the data and business logic of the application. It's responsible for interacting with the database.
*   **View:** The user interface (UI). It displays the data from the Model.
*   **Controller:** Handles user input. It takes input from the View, processes it (by interacting with the Model), and updates the View.

#### **3. Explain Sessions and Cookies. What are the differences?**

*   **Cookie:** A small piece of data stored **on the client's browser**. It's sent with every request to the server. Used for remembering user preferences, tracking, and session management.
*   **Session:** A mechanism to store information about a user **on the server**. The server creates a unique Session ID, sends it to the client to be stored in a cookie, and the client sends this ID back with each request. The server then uses this ID to retrieve the user's session data.

**Difference:** The main difference is **where the data is stored**. Cookies store data on the client; sessions store data on the server.

#### **4. Where are cookies stored on the client-side?**

They are stored in a small text file managed by the web browser on the user's computer.

#### **5. Explain the difference between Cookies, Session Storage, and Local Storage.**

| Feature | Cookies | Local Storage | Session Storage |
| :--- | :--- | :--- | :--- |
| **Capacity** | ~4KB | ~5-10MB | ~5MB |
| **Expiration** | Manually set expiration date | Never expires | Expires when tab/window is closed |
| **Accessibility**| Accessible from any window/tab | Accessible from any window/tab (from the same origin) | Accessible only in the same tab/window |
| **With Server**| Sent with every HTTP request | Not sent with requests | Not sent with requests |

#### **6. What is JWT (JSON Web Token)? Explain its basic structure.**

JWT is an open standard for securely transmitting information between parties as a JSON object. It is often used for authentication and authorization. A JWT consists of three parts separated by dots (`.`):
1.  **Header:** Contains the token type (JWT) and the signing algorithm (e.g., HMAC SHA256).
2.  **Payload:** Contains the claims (the data), such as user ID, roles, and expiration time.
3.  **Signature:** A cryptographic signature created using the header, the payload, and a secret key. It's used to verify that the token has not been tampered with.

#### **7. Explain the difference between Authentication and Authorization.**

*   **Authentication (Who are you?):** The process of verifying a user's identity. This is typically done with a username and password, biometrics, or an access token. It answers the question, "Are you who you say you are?"
*   **Authorization (What can you do?):** The process of determining if an authenticated user has permission to access a specific resource or perform a specific action. It answers the question, "Are you allowed to do this?"

#### **8. What are web injection attacks?**

Injection attacks are a broad class of attacks where an attacker supplies untrusted input to a program, which is then executed or interpreted as part of a command or query. A common example is **SQL Injection**, where an attacker injects malicious SQL code into a form field to manipulate a database.

#### **9. What is CSRF (Cross-Site Request Forgery)?**

CSRF is an attack that tricks a user into submitting a malicious request to a web application where they are currently authenticated. For example, an attacker could embed a hidden form on a malicious website that, when visited by the victim, automatically sends a request to the victim's bank to transfer money. Protection involves using anti-CSRF tokens.

#### **10. What is Cross-Origin Resource Sharing (CORS)?**

CORS is a browser security mechanism that allows a web application running at one origin (domain) to request resources from another origin. By default, browsers block these requests for security reasons (Same-Origin Policy). A server can enable CORS by sending specific HTTP headers (like `Access-Control-Allow-Origin: *`).

#### **11. What is server-side caching? What are caching policies?**

**Server-side caching** involves storing frequently accessed data or pre-computed results in a fast-access layer (like Redis or Memcached) on the server. This avoids expensive operations like database queries or complex calculations on every request, improving performance and reducing server load.

**Caching Policies** are rules that define what to cache, for how long (TTL - Time to Live), and when to invalidate the cache (e.g., on data update).

***

### **HTML & CSS**

#### **1. What is the difference between `display: none;` and `visibility: hidden;` in CSS?**

*   **`display: none;`:** The element is completely removed from the document flow. It takes up no space, and other elements will fill its place.
*   **`visibility: hidden;`:** The element is hidden, but it still occupies its space in the layout. It's invisible, but the space it would have taken is preserved.

#### **2. What are the `br`, `tb`, `td` tags in HTML?**

*   **`<br>`:** A line break tag. It creates a new line.
*   **`<td>`:** A table data cell tag. It defines a standard cell in an HTML table.
*   **`tb`:** This is not a standard HTML tag. The user likely meant **`<table>`** (which defines the table), **`<tbody>`** (which groups the body content in a table), or **`<th>`** (which defines a header cell).

#### **3. How would you format a table in HTML?**

You structure a table using the following tags:
*   **`<table>`:** The wrapper for the entire table.
*   **`<thead>`:** (Optional) The group of header content.
*   **`<tbody>`:** The group of body content.
*   **`<tr>`:** A table row.
*   **`<th>`:** A table header cell (usually bold and centered).
*   **`<td>`:** A table data cell.

```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Role</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John Doe</td>
      <td>Developer</td>
    </tr>
  </tbody>
</table>
```

***

### **Data Structures & Algorithms / Coding Challenges**

#### **1. Explain Stacks and Queues.**

*   **Stack:** A data structure that follows the **LIFO (Last-In, First-Out)** principle. The last element added is the first one to be removed. Operations are `push` (add to top) and `pop` (remove from top). Think of a stack of plates.
*   **Queue:** A data structure that follows the **FIFO (First-In, First-Out)** principle. The first element added is the first one to be removed. Operations are `enqueue` (add to back) and `dequeue` (remove from front). Think of a line at a ticket counter.

#### **2. Write a function that finds a subpattern in a pattern array.**

This is essentially implementing a basic search algorithm.
```javascript
function findSubpattern(pattern, subpattern) {
  if (subpattern.length === 0) return 0;
  if (pattern.length < subpattern.length) return -1;

  for (let i = 0; i <= pattern.length - subpattern.length; i++) {
    let match = true;
    for (let j = 0; j < subpattern.length; j++) {
      if (pattern[i + j] !== subpattern[j]) {
        match = false;
        break;
      }
    }
    if (match) {
      return i; // Return the starting index of the match
    }
  }

  return -1; // Return -1 if no match is found
}

// Example:
const pattern = [1, 2, 3, 4, 5, 6, 7];
const subpattern = [3, 4, 5];
console.log(findSubpattern(pattern, subpattern)); // Output: 2
```

#### **3. How would you remove options from a `<select>` and append new ones using jQuery?**
```javascript
// Assume you have HTML like this: <select id="mySelect"></select>
const newOptions = [
  { value: 'volvo', text: 'Volvo' },
  { value: 'saab', text: 'Saab' }
];

// Get a reference to the select element
const $select = $('#mySelect');

// 1. Remove all existing options
$select.empty();

// 2. Append new options
$.each(newOptions, function(index, option) {
  $select.append($('<option>', {
    value: option.value,
    text: option.text
  }));
});
```

***

### **General/System Concepts**

#### **1. Is JavaScript a multi-threading language?**

No, JavaScript is fundamentally a **single-threaded** language. It has one call stack and one memory heap, meaning it executes code sequentially.

#### **2. What are threads? Why do we use them?**

A **thread** is the smallest sequence of programmed instructions that can be managed independently by a scheduler. A process can contain one or more threads. We use **multi-threading** to perform parallel execution of tasks, which can significantly improve the performance of CPU-bound applications by utilizing multiple CPU cores.

#### **3. What is a process? How many stack frames are created for a process?**

A **process** is an instance of a computer program that is being executed. It contains the program code and its current activity. A process has its own memory space. A process itself doesn't have "stack frames"; its **threads** have stacks, and each function call within a thread creates a new **stack frame** on that thread's call stack.

#### **4. Is a stack frame allocated for a single function or the entire program?**

A stack frame is allocated for a **single function call**. When a function is called, a new frame is pushed onto the call stack containing its local variables and arguments. When the function returns, its stack frame is popped off the stack.

#### **5. What is a kernel and what are its functionalities in an Operating System?**

The **kernel** is the core component of an Operating System. It has complete control over everything in the system. Its main functionalities are:
*   **Process Management:** Managing the creation, execution, and termination of processes.
*   **Memory Management:** Allocating and de-allocating memory space for processes.
*   **Device Management:** Acting as an intermediary between hardware (CPU, disk drives, printers) and software.
*   **System Call and I/O Management:** Handling requests from programs for services like file I/O.

#### **6. How does RAM process information?**

RAM (Random Access Memory) is volatile memory that stores data the CPU needs to access quickly. The CPU can read from and write to any location in RAM directly using memory addresses. When a program is run, its instructions and data are loaded from a slower storage device (like an SSD) into RAM so the CPU can execute them rapidly.

#### **7. Have you ever encountered a segmentation fault error? What is it?**

A **segmentation fault** is an error that occurs when a program tries to access a memory location that it is not allowed to access (e.g., memory outside its allocated space, or a read-only location it's trying to write to). In the context of web development, you are unlikely to see this error directly from your JavaScript code, as the JS engine and OS manage memory for you. It's more common in lower-level languages like C/C++.