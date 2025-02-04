Sure! Let's improve these notes by expanding on key concepts, adding detailed explanations, beginner-friendly examples, and visual diagrams to enhance understanding.

---

# Episode 23: Understanding `async` and `await` in JavaScript

### Topics Covered
- What is `async`?
- What is `await`?
- How `async`/`await` works behind the scenes
- Examples of using `async`/`await`
- Error Handling with `async`/`await`
- Comparing `async`/`await` with Promises (`.then` and `.catch`)
- Practical examples and visualizations

---

### **What is `async`?**

**`async`** is a keyword in JavaScript that is used before a function to create an asynchronous function. An asynchronous function is one that allows operations to be performed without blocking the main thread. When you use the `async` keyword before a function, it always returns a Promise, even if you return a simple value.

#### **Example:**

```javascript
async function greet() {
  return "Hello, JavaScript!";
}

const greetingPromise = greet();
console.log(greetingPromise); // Promise {<fulfilled>: "Hello, JavaScript!"}
```

**Explanation:**

- When we call the `greet()` function, it returns a Promise.
- Even though we return a simple string from the function, JavaScript automatically wraps it in a Promise.
- To extract the value from this Promise, we can use `.then()`:

```javascript
greetingPromise.then((message) => console.log(message)); // "Hello, JavaScript!"
```

### **What is `await`?**

**`await`** is a keyword that can be used inside an `async` function to pause the execution of the function until the Promise is resolved or rejected. It makes asynchronous code look and behave more like synchronous code, which can be easier to understand.

#### **Example:**

```javascript
async function getData() {
  const response = await fetch("https://api.github.com/users/octocat");
  const data = await response.json();
  console.log(data);
}

getData();
```

**Explanation:**

- The `await` keyword pauses the execution of the `getData` function until the `fetch` Promise is resolved.
- Once resolved, `await` retrieves the resolved value, and the function continues to the next line.
- The code is easier to read because it flows top-down, like synchronous code.

### **How `async`/`await` Works Behind the Scenes**

When an `async` function is called, it executes synchronously until the first `await` is encountered. The function execution is then paused, and the JavaScript engine moves on to execute other code. Once the awaited Promise is resolved or rejected, the function execution resumes.

#### **Diagram:**

```
+-----------------------+
| async function called  |
| Execution starts...    |
+-----------------------+
         |
         v
+-----------------------+
| Encounter `await`      |
| Execution pauses       |
+-----------------------+
         |
         v
+-----------------------+
| JavaScript moves to    |
| other tasks...         |
+-----------------------+
         |
         v
+-----------------------+
| Promise resolved       |
| Resume function        |
+-----------------------+
```

### **Using `await` with `async` Functions**

The combination of `async` and `await` allows us to write code that handles Promises in a more readable way. Let's compare the traditional `.then()` method with `async`/`await` using a delay example:

#### **Example:**

```javascript
// Using .then() method
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("Data fetched!");
    }, 2000);
  });
}

fetchData().then((message) => {
  console.log(message); // After 2 seconds: "Data fetched!"
});
```

```javascript
// Using async/await
async function fetchData() {
  const message = await new Promise((resolve) => {
    setTimeout(() => {
      resolve("Data fetched!");
    }, 2000);
  });
  console.log(message); // After 2 seconds: "Data fetched!"
}

fetchData();
```

**Explanation:**

- In the `.then()` approach, the callback function is passed to `then()`.
- With `async`/`await`, the code looks more like synchronous code, which is easier to follow.

### **Comparing `async/await` with Promises**

**Why use `async/await` instead of `.then()` and `.catch()`?**

- **Readability:** `async/await` allows code to be written in a more sequential manner, making it easier to understand, especially for beginners.
- **Error Handling:** With `async/await`, you can use `try/catch` blocks for error handling, which is a more familiar pattern compared to using `.catch()`.

#### **Example:**

**Using `.then()` and `.catch()`:**

```javascript
fetch("https://api.github.com/users/octocat")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.log("Error:", error));
```

**Using `async/await` with `try/catch`:**

```javascript
async function fetchData() {
  try {
    const response = await fetch("https://api.github.com/users/octocat");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("Error:", error);
  }
}

fetchData();
```

### **Error Handling with `async/await`**

Error handling in `async/await` can be done using `try/catch` blocks. This pattern is similar to synchronous error handling and is easier to understand for beginners.

#### **Example:**

```javascript
async function fetchUserData() {
  try {
    const response = await fetch("https://api.github.com/users/nonexistentuser");
    if (!response.ok) {
      throw new Error("User not found");
    }
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("Error:", error.message);
  }
}

fetchUserData();
```

**Explanation:**

- If the fetch request fails (e.g., the user doesn't exist), the code throws an error.
- The error is caught in the `catch` block, and you can handle it accordingly.

### **Advanced Concept: Parallel Execution with `Promise.all` and `async/await`**

Sometimes, you need to execute multiple asynchronous operations simultaneously and wait for all of them to finish. This can be achieved using `Promise.all` with `async/await`.

#### **Example:**

```javascript
async function fetchMultipleData() {
  const [user, repos] = await Promise.all([
    fetch("https://api.github.com/users/octocat").then((res) => res.json()),
    fetch("https://api.github.com/users/octocat/repos").then((res) => res.json())
  ]);

  console.log("User:", user);
  console.log("Repositories:", repos);
}

fetchMultipleData();
```

**Explanation:**

- `Promise.all` takes an array of Promises and returns a single Promise that resolves when all the Promises have resolved.
- The result is an array containing the resolved values in the same order as the Promises were passed.

**Visualizing Parallel Execution:**

```
+----------------------+  +----------------------+
| Fetch User Data      |  | Fetch User Repos     |
| (Promise 1)          |  | (Promise 2)          |
+----------+-----------+  +-----------+----------+
           |                          |
           v                          v
  +------------------+      +------------------+
  | All Promises Done|      | Handle Results   |
  +------------------+      +------------------+
```

### **Real-World Example with `async/await`**

In real-world scenarios, you might use `async/await` to interact with APIs, handle user input, or manage complex workflows.

#### **Example:**

```javascript
async function getUserAndFollowers() {
  try {
    const user = await fetch("https://api.github.com/users/octocat").then((res) => res.json());
    console.log("User:", user);

    const followers = await fetch(user.followers_url).then((res) => res.json());
    console.log("Followers:", followers);
  } catch (error) {
    console.log("Error:", error.message);
  }
}

getUserAndFollowers();
```

**Explanation:**

- First, the user data is fetched from the GitHub API.
- Then, the user's followers are fetched using the `followers_url` from the previous response.
- The code is sequential and easy to follow, making it an excellent example of using `async/await` for API requests.

### **Conclusion**

- **`async/await`** simplifies the process of working with asynchronous code, making it more readable and easier to understand.
- **Error handling** becomes more intuitive with `try/catch` blocks.
- **Comparing `async/await` with Promises** shows that while both are powerful, `async/await` often provides a cleaner and more maintainable approach.
- **Practical examples** demonstrate how `async/await` can be used in real-world scenarios, like fetching data from APIs.

---

By expanding these concepts and examples, the notes are now tailored to help a complete beginner grasp the fundamental principles of `async/await` in JavaScript, along with practical applications and visual aids for better understanding.

----
Original Code
----
# Episode 23 : async await

###
Topics Covered
- What is async?
- What is await?
- How async await works behind the scenes?
- Example of using async/await
- Error Handling
- Interviews
- Async await vs Promise.then/.catch

Q: What is async?  
A: Async is a keyword that is used before a function to create a async function.

Q: What is async function and how it is different from normal function?  
```js
// 💡 async function always returns a promise, even if I return a simple string from below function, async keyword will wrap it under Promise and then return.
async function getData() {
  return "Namaste JavaScript";
}
const dataPromise = getData();
console.log(dataPromise); // Promise {<fulfilled>: 'Namaste JavaScript'}

//❓How to extract data from above promise? One way is using promise .then
dataPromise.then(res => console.log(res)); // Namaste JavaScript
```
Another example where `async` function is returning a Promise
```js
const p = new Promise((resolve, reject) => {
  resolve('Promise resolved value!!');
})

async function getData() {
  return p;
}
// In above case, since we are already returning a promise async function would simply return that instead of wrapping with a new Promise.
const dataPromise = getData();
console.log(dataPromise); // Promise {<fulfilled>: 'Promise resolved value!!'}
dataPromise.then(res => console.log(res)); // Promise resolved value!!
```

Q: How we can use `await` along with async function?  
A: `async` and `await` combo is used to handle promises.

But Question is how we used to handle promises earlier and why we even need async/await?

```js
const p = new Promise((resolve, reject) => {
  resolve('Promise resolved value!!');
})

function getData() {
  p.then(res => console.log(res));
}

getData(); // Promise resolved value!!

//📌 Till now we have been using Promise.then/.catch to handle promise.
// Now let's see how async await can help us and how it is different

// The rule is we have to use keyword await in front of promise.
async function handlePromise() {
  const val = await p;
  console.log(val);
}
handlePromise(); // Promise resolved value!!
```
📌 `await` is a keyword that can only be used inside a `async` function.
```js
await function() {} // Syntax error: await is only valid under async function.
```

Q: What makes `async`-`await` special?  
A: Let's understand with one example where we will compare async-await way of resolving promise with older .then/.catch fashion. For that we will modify our promise `p`.
```js
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Promise resolved value!!');
  }, 3000);
})

// Let's now compare with some modification:

// 📌 Promise.then/.catch way
function getData() {
  // JS engine will not wait for promise to be resolved
  p.then(res => console.log(res));
  console.log('Hello There!');
}

getData(); // First `Hello There!` would be printed and then after 3 secs 'Promise resolved value!!' will be printed.
// Above happened as Javascript wait for none, so it will register this promise and take this callback function and register separately then js will move on and execute the following console and later once promise is resolved, following console will be printed.

//❓ Problem: Normally one used to get confused that JS will wait for promise to be resolved before executing following lines.

// 📌 async-wait way:
async function handlePromise() {
  // JS Engine will waiting for promise to resolve.
  const val = await p;
  console.log('Hello There!');
  console.log(val);
}
handlePromise(); // This time `Hello There!` won't be printed immediately instead after 3 secs `Hello There!` will be printed followed by 'Promise resolved value!!'
// 💡 So basically code was waiting at `await` line to get the promise resolve before moving on to next line.

// Above is the major difference between Promise.then/.catch vs async-await

//🤓 Let's brainstorm more around async-await
async function handlePromise() {
  console.log('Hi');
  const val = await p;
  console.log('Hello There!');
  console.log(val);

  const val2 = await p;
  console.log('Hello There! 2');
  console.log(val2);
}
handlePromise(); 
// In above code example, will our program wait for 2 time or will it execute parallely.
//📌 `Hi` printed instantly -> now code will wait for 3 secs -> After 3 secs both promises will be resolved so ('Hello There!' 'Promise resolved value!!' 'Hello There! 2' 'Promise resolved value!!') will get printed immediately.

// Let's create one promise and then resolve two different promise.
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Promise resolved value by p2!!');
  }, 2000);
})

async function handlePromise() {
  console.log('Hi');
  const val = await p;
  console.log('Hello There!');
  console.log(val);

  const val2 = await p2;
  console.log('Hello There! 2');
  console.log(val2);
}
handlePromise(); 
// 📌 `Hi` printed instantly -> now code will wait for 3 secs -> After 3 secs both promises will be resolved so ('Hello There!' 'Promise resolved value!!' 'Hello There! 2' 'Promise resolved value by p2!!') will get printed immediately. So even though `p2` was resolved after 2 secs it had to wait for `p` to get resolved


// Now let's reverse the order execution of promise and observe response.
async function handlePromise() {
  console.log('Hi');
  const val = await p2;
  console.log('Hello There!');
  console.log(val);

  const val2 = await p;
  console.log('Hello There! 2');
  console.log(val2);
}
handlePromise(); 
// 📌 `Hi` printed instantly -> now code will wait for 2 secs -> After 2 secs ('Hello There!' 'Promise resolved value by p2!!') will get printed and in the subsequent second i.e. after 3 secs ('Hello There! 2' 'Promise resolved value!!') will get printed
```

Q: Question is Is program actually waiting or what is happening behind the scene?  
A: As we know, Time, Tide and JS wait for none. And it's true. Over here it appears that JS engine is waiting but JS engine is not waiting over here. It has not occupied the call stack if that would have been the case our page may have got frozen. So JS engine is not waiting. So if it is not waiting then what it is doing behind the scene? Let's understand with below code snippet.
```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Promise resolved value by p1!!');
  }, 5000);
})

const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Promise resolved value by p2!!');
  }, 10000);
})

async function handlePromise() {
  console.log('Hi');
  debugger;
  const val = await p;
  console.log('Hello There!');
  debugger;
  console.log(val);

  const val2 = await p2;
  console.log('Hello There! 2');
  debugger;
  console.log(val2);
}
handlePromise(); 
// When this function is executed, it will go line by line as JS is synchronous single threaded language. Lets observe what is happening under call-stack. Above you can see we have set the break-points.

// call stack flow -> handlePromise() is pushed -> It will log `Hi` to console -> Next it sees we have await where promise is suppose to be resolved -> So will it wait for promise to resolve and block call stack? No -> thus handlePromise() execution get suspended and moved out of call stack -> So when JS sees await keyword it suspend the execution of function till promise is resolved -> So `p` will get resolved after 5 secs so handlePromise() will be pushed to call-stack again after 5 secs. -> But this time it will start executing from where it had left. -> Now it will log 'Hello There!' and 'Promise resolved value!!' -> then it will check whether `p2` is resolved or not -> It will find since `p2` will take 10 secs to resolve so the same above process will repeat -> execution will be suspended until promise is resolved.

// 📌 Thus JS is not waiting, call stack is not getting blocked.

// Moreover in above scenario what if p1 would be taking 10 secs and p2 5 secs -> even though p2 got resolved earlier but JS is synchronous single threaded language so it will first wait for p1 to be resolved and then will immediately execute all.
```

### Real World example of async/await

```js
async function handlePromise() {
  // fetch() => Response Object which as body as Readable stream => Response.json() is also a promise which when resolved => value
  const data = await fetch('https://api.github.com/users/alok722');
  const res = await data.json();
  console.log(res);
};
handlePromise()
```

### Error Handling

While we were using normal Promise we were using .catch to handle error, now in `async-await` we would be using `try-catch` block to handle error.

```js
async function handlePromise() {
  try {
    const data = await fetch('https://api.github.com/users/alok722');
    const res = await data.json();
    console.log(res);
  } catch (err) {
    console.log(err)
  }
};
handlePromise()

// In above whenever any error will occur the execution will move to catch block. One could try above with bad url which will result in error.

// Other way of handling error:
handlePromise().catch(err => console.log(err)); // this will work as handlePromise will return error promise in case of failure.
```

### Async await vs Promise.then/.catch
What one should use? `async-await` is just a syntactic sugar around promise. Behind the scene `async-await` is just promise. So both are same, it's just `async-await` is new way of writing code. `async-await` solves few of the short-coming of Promise like `Promise Chaining`. `async-await` also increases the readability. So sort of it is always advisable to use `async-await.`

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=6nv3qy3oNkc&list=PLlasXeu85E9eWOpw9jxHOQyGMRiBZ60aX&index=4&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/6nv3qy3oNkc/0.jpg" width="750"
alt="async-await in Javascript Youtube Link"/></a>
