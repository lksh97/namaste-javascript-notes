A Promise is an object which represents eventual completion or failure of an asynchronous operation. Promises are immutable once resolved and they give us a lot of control over our code. 

They have three state 
1) pending
2) fulfilled or rejected. 

The promise object contains two parts
 1) PromiseState(stores the sate of the prmise)
 2) PromiseResult(stores data)

---

## What are Asynchronous Operations?

### Definition
**Asynchronous operations** allow your program to handle long-running tasks (like fetching data from a server) without blocking the main thread. This means your program can continue executing other tasks while waiting for the asynchronous operation to complete.

### Real-World Analogy
Imagine you're ordering food at a restaurant. After placing your order, you don't just stand by the kitchen waiting for the food to be ready. Instead, you might chat with friends, check your phone, or do other things while the kitchen prepares your meal. Once the meal is ready, the waiter serves it to you.

Similarly, in JavaScript, while an asynchronous operation (like fetching data) is happening in the background, your program can continue to perform other tasks.

### Synchronous vs. Asynchronous Operations

- **Synchronous**: Operations are executed one after the other. Each operation waits for the previous one to complete.
  - Example: Cooking one dish at a time, finishing each before starting the next.
  
- **Asynchronous**: Operations can be initiated and then left to complete while other operations are carried out.
  - Example: Starting to cook several dishes simultaneously, and each is finished when it's ready.

### Diagram: Synchronous vs. Asynchronous

```plaintext
Synchronous:           Asynchronous:
Task 1                 Task 1 ----> (initiated)
   |                     |         
Task 2                 Task 2 (while Task 1 is still running)
   |                     |
Task 3                 Task 3 (while Task 1 is still running)
   |                     |
...                   Task 1 completes (eventually)
```

### Example: Synchronous Code

```javascript
console.log("Start cooking");
console.log("Cooking dish 1...");
console.log("Dish 1 done!");
console.log("Cooking dish 2...");
console.log("Dish 2 done!");
console.log("Cooking dish 3...");
console.log("Dish 3 done!");
console.log("All dishes done!");
```

### Example: Asynchronous Code

```javascript
console.log("Start cooking");

setTimeout(() => {
  console.log("Dish 1 done! (after 2 seconds)");
}, 2000);

setTimeout(() => {
  console.log("Dish 2 done! (after 1 second)");
}, 1000);

console.log("Continue doing other things...");

setTimeout(() => {
  console.log("Dish 3 done! (after 3 seconds)");
}, 3000);

console.log("All dishes started!");
```

**Explanation**:
- `setTimeout` is an example of an asynchronous operation. It schedules a task (in this case, printing a message) to be executed after a specified time.
- The program doesn't wait for each `setTimeout` to finish. Instead, it schedules all the tasks and continues executing other code.

---

## Understanding Promises

### What is a Promise?

A **Promise** in JavaScript is an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

### Explanation: Eventual Completion or Failure
- **Eventual Completion**: The asynchronous operation completes successfully, and the Promise is said to be **fulfilled**. This means the operation has produced a result.
- **Failure**: The asynchronous operation encounters an error or fails to complete, and the Promise is **rejected**. This means the operation did not succeed, and an error is produced.

### The Promise Lifecycle

A Promise can be in one of three states:
1. **Pending**: The initial state, when the Promise is neither fulfilled nor rejected.
2. **Fulfilled**: The operation completed successfully, and the Promise has a resulting value.
3. **Rejected**: The operation failed, and the Promise has a reason for the failure (an error).

### Diagram: Promise Lifecycle

```plaintext
Start
  |
Pending  -----> Fulfilled (operation completed) -----> Final Value
  |                                               
  ------> Rejected (operation failed) -------> Error/Reason
```

### Example Code: Creating a Promise

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true; // Simulating success or failure

  if (success) {
    resolve("The operation was successful!"); // Move to "fulfilled"
  } else {
    reject("The operation failed."); // Move to "rejected"
  }
});

// Handling the Promise
promise
  .then((result) => {
    console.log(result); // "The operation was successful!"
  })
  .catch((error) => {
    console.error(error); // "The operation failed."
  });
```

### Breakdown and Explanation:

- **`new Promise((resolve, reject) => { ... })`**: This creates a new Promise object. Inside the Promise, you define the asynchronous operation.
  - **`resolve`**: A function that you call when the operation succeeds, changing the Promise's state to "fulfilled."
  - **`reject`**: A function that you call when the operation fails, changing the Promise's state to "rejected."

- **`.then(result => { ... })`**: This method is called when the Promise is fulfilled. It receives the value passed to `resolve`.
  
- **`.catch(error => { ... })`**: This method is called when the Promise is rejected. It receives the error passed to `reject`.

### Example: Asynchronous Operation with Promises

Let's revisit the e-commerce example using Promises:

```javascript
const createOrder = (cart) => {
  return new Promise((resolve, reject) => {
    console.log("Processing order...");
    setTimeout(() => {
      const orderId = "12345"; // Assume this is the generated order ID
      console.log("Order created with ID:", orderId);
      resolve(orderId); // Fulfill the promise with the order ID
    }, 2000); // Simulate 2 seconds delay
  });
};

const proceedToPayment = (orderId) => {
  return new Promise((resolve, reject) => {
    console.log("Processing payment...");
    setTimeout(() => {
      const paymentInfo = "Payment successful"; // Payment process completed
      console.log("Payment processed for order ID:", orderId);
      resolve(paymentInfo); // Fulfill the promise with payment information
    }, 1000); // Simulate 1 second delay
  });
};

const showOrderSummary = (paymentInfo) => {
  return new Promise((resolve, reject) => {
    console.log("Showing order summary...");
    setTimeout(() => {
      console.log("Order summary shown:", paymentInfo);
      resolve(); // Resolve without value, just to indicate completion
    }, 500); // Simulate 0.5 seconds delay
  });
};

// Using Promises
createOrder(["shoes", "pants", "kurta"])
  .then((orderId) => proceedToPayment(orderId))
  .then((paymentInfo) => showOrderSummary(paymentInfo))
  .then(() => console.log("Order process completed successfully!"))
  .catch((error) => console.error("Order process failed:", error));
```

### Explanation and State Changes:

1. **`createOrder(["shoes", "pants", "kurta"])`**:
   - The Promise is created and initially in the **Pending** state.
   - After 2 seconds, the operation is completed, and the Promise moves to the **Fulfilled** state with the `orderId`.

2. **`.then((orderId) => proceedToPayment(orderId))`**:
   - The `orderId` from the first Promise is passed to `proceedToPayment`.
   - This creates another Promise, initially in the **Pending** state.
   - After 1 second, the payment is processed, and the Promise moves to the **Fulfilled** state with `paymentInfo`.

3. **`.then((paymentInfo) => showOrderSummary(paymentInfo))`**:
   - The `paymentInfo` is passed to `showOrderSummary`.
   - This creates a final Promise, initially in the **Pending** state.
   - After 0.5 seconds, the order summary is shown, and the Promise moves to the **Fulfilled** state.

4. **`.catch((error) => console.error("Order process failed:", error))`**:
   - If any of the Promises are rejected at any point, the error handling block catches it and logs the error.

### Diagram: Promise Chaining

```plaintext
createOrder -----> (fulfilled) -----> proceedToPayment -----> (fulfilled) -----> showOrderSummary -----> (fulfilled)
  |                      |                      |                        |                        |
  |                      +--> (rejected)        |                        +--> (rejected)          +--> (rejected)
  +--> (rejected)                                                        
```

### Detailed Code Explanation:

- **Promises** help in managing the flow of asynchronous operations by ensuring each step is completed before moving to the next.
- **State Transitions**: The Promise moves from `Pending` to either `Fulfilled` or `Rejected` based on the outcome of the asynchronous operation.
- **Promise Chaining**: You can chain `.then()` methods to execute asynchronous operations in sequence. Each `.then()` receives the result of the previous operation.
- **Error Handling**: The `.catch()` method provides a way to handle errors that may occur during any of the asynchronous operations in the chain.

### Summary

- **Asynchronous operations** allow JavaScript to perform tasks like network requests without blocking the main thread

-----
-----

# Episode 21 : Promises

> Promises are used to handle async operations in JavaScript.

We will discuss with code example that how things used to work before `Promises` and then how it works after `Promises`

Suppose, taking an example of E-Commerce

```js
const cart = ["shoes", "pants", "kurta"];

// Below two functions are asynchronous and dependent on each other
const orderId = createOrder(cart);
proceedToPayment(orderId);

// with Callback (Before Promise)
// Below here, it is the responsibility of createOrder function to first create the order then call the callback function
createOrder(cart, function () {
  proceedToPayment(orderId);
});
// Above there is the issue of `Inversion of Control`
```

Q: How to fix the above issue?  
_A: Using Promise._

Now, we will make `createOrder` function return a promise and we will capture that `promise` into a `variable`

Promise is nothing but we can assume it to be empty object with some data value in it, and this data value will hold whatever this `createOrder` function will return.

Since `createOrder` function is an async function and we don't know how much time will it take to finish execution.

So the moment `createOrder` will get executed, it will return you a `undefined` value. Let's say after 5 secs execution finished so now `orderId` is ready so, it will fill the `undefined` value with the `orderId`.

In short, When `createOrder` get executed, it immediately returns a `promise object` with `undefined` value. then javascript will continue to execute with other lines of code. After sometime when `createOrder` has finished execution and `orderId` is ready then that will `automatically` be assigned to our returned `promise` which was earlier `undefined`.

Q: Question is how we will get to know `response` is ready?  
_A: So, we will attach a `callback` function to the `promise object` using `then` to get triggered automatically when `result` is ready._

```js
const cart = ["shoes", "pants", "kurta"];

const promiseRef = createOrder(cart);
// this promiseRef has access to `then`

// {data: undefined}
// Initially it will be undefined so below code won't trigger
// After some time, when execution has finished and promiseRef has the data then automatically the below line will get triggered.

promiseRef.then(function () {
  proceedToPayment(orderId);
});
```

Q: How it is better than callback approach?

In Earlier solution we used to pass the function and then used to trust the function to execute the callback.

But with promise, we are attaching a callback function to a promiseObject.

There is difference between these words, passing a function and attaching a function.

Promise guarantee, it will callback the attached function once it has the fulfilled data. And it will call it only once. Just once.

Earlier we talked about promise are object with empty data but that's not entirely true, `Promise` are much more than that.

Now let's understand and see a real promise object.

fetch is a web-api which is utilized to make api call and it returns a promise.

We will be calling public github api to fetch data
https://api.github.com/users/alok722

```js
// We will be calling public github api to fetch data
const URL = "https://api.github.com/users/alok722";
const user = fetch(URL);
// User above will be a promise.
console.log(user); // Promise {<Pending>}

/** OBSERVATIONS:
 * If we will deep dive and see, this `promise` object has 3 things
 * `prototype`, `promiseState` & `promiseResult`
 * & this `promiseResult` is the same data which we talked earlier as data
 * & initially `promiseResult` is `undefined`
 *
 * `promiseResult` will store data returned from API call
 * `promiseState` will tell in which state the promise is currently, initially it will be in `pending` state and later it will become `fulfilled`
 */

/**
 * When above line is executed, `fetch` makes API call and return a `promise` instantly which is in `Pending` state and Javascript doesn't wait to get it `fulfilled`
 * And in next line it console out the `pending promise`.
 * NOTE: chrome browser has some in-consistency, the moment console happens it shows in pending state but if you will expand that it will show fulfilled because chrome updated the log when promise get fulfilled.
 * Once fulfilled data is there in promiseResult and it is inside body in ReadableStream format and there is a way to extract data.
 */
```

Now we can attach callback to above response?

Using `.then`

```js
const URL = "https://api.github.com/users/alok722";
const user = fetch(URL);

user.then(function (data) {
  console.log(data);
});
// And this is how Promise is used.
// It guarantees that it could be resolved only once, either it could be `success` or `failure`
/**
    A Promise is in one of these states:

    pending: initial state, neither fulfilled nor rejected.
    fulfilled: meaning that the operation was completed successfully.
    rejected: meaning that the operation failed.
 */
```

💡Promise Object are immutable.  
-> Once promise is fulfilled and we have data we can pass here and there and we don't have to worry that someone can mutate that data. So over above we can't directly mutate `user` promise object, we will have to use `.then`

### Interview Guide

💡What is Promise?  
-> Promise object is a placeholder for certain period of time until we receive value from asynchronous operation.

-> A container for a future value.

-> **A Promise is an object representing the eventual completion or failure of an asynchronous operation.**

We are now done solving one issue of callback i.e. Inversion of Control

But there is one more issue, callback hell...

```js
// Callback Hell Example
createOrder(cart, function (orderId) {
  proceedToPayment(orderId, function (paymentInf) {
    showOrderSummary(paymentInf, function (balance) {
      updateWalletBalance(balance);
    });
  });
});
// And now above code is expanding horizontally and this is called pyramid of doom.
// Callback hell is ugly and hard to maintain.

// 💡 Promise fixes this issue too using `Promise Chaining`
// Example Below is a Promise Chaining
createOrder(cart)
  .then(function (orderId) {
    proceedToPayment(orderId);
  })
  .then(function (paymentInf) {
    showOrderSummary(paymentInf);
  })
  .then(function (balance) {
    updateWalletBalance(balance);
  });

// ⚠️ Common PitFall
// We forget to return promise in Promise Chaining
// The idea is promise/data returned from one .then become data for next .then
// So,
createOrder(cart)
  .then(function (orderId) {
    return proceedToPayment(orderId);
  })
  .then(function (paymentInf) {
    return showOrderSummary(paymentInf);
  })
  .then(function (balance) {
    return updateWalletBalance(balance);
  });

// To improve readability you can use arrow function instead of regular function
```

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=ap-6PPAuK1Y&list=PLlasXeu85E9eWOpw9jxHOQyGMRiBZ60aX&index=3&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/ap-6PPAuK1Y/0.jpg" width="750"
alt="promise in Javascript Youtube Link"/></a>


-----

Yes, Java also supports asynchronous programming, though the mechanisms are different from JavaScript. Asynchronous operations in Java allow you to perform tasks like file I/O, network requests, or computations in the background without blocking the main thread. This is especially useful in applications that need to remain responsive, like GUI applications, servers, or any application where tasks might take an unpredictable amount of time to complete.

### Asynchronous Programming in Java

#### Traditional Approach: Using Threads

In Java, the traditional way to perform asynchronous tasks is by using threads. A thread is a lightweight process that can run concurrently with other threads.

**Example:**

```java
public class AsyncExample {
    public static void main(String[] args) {
        System.out.println("Main thread starts");

        // Creating a new thread to run an asynchronous task
        Thread thread = new Thread(() -> {
            try {
                // Simulate a long-running task
                Thread.sleep(2000);
                System.out.println("Asynchronous task completed");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        thread.start(); // Start the asynchronous task

        System.out.println("Main thread continues");

        try {
            thread.join(); // Wait for the asynchronous task to complete
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Main thread ends");
    }
}
```

**Explanation:**
- **`Thread`**: We create a new thread and pass a `Runnable` (a task to be executed) to its constructor.
- **`thread.start()`**: This starts the new thread, which runs the task asynchronously.
- **`thread.join()`**: This makes the main thread wait for the asynchronous task to finish.

### Modern Approach: Using `CompletableFuture`

Java 8 introduced `CompletableFuture`, which is a powerful tool for asynchronous programming. It allows you to write non-blocking code and handle asynchronous tasks more elegantly than using threads directly.

**Example:**

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExample {
    public static void main(String[] args) {
        System.out.println("Main thread starts");

        // Asynchronous task using CompletableFuture
        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            try {
                // Simulate a long-running task
                Thread.sleep(2000);
                System.out.println("Asynchronous task completed");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        // Continue with other tasks
        System.out.println("Main thread continues");

        // Block and wait for the future to complete
        future.join();

        System.out.println("Main thread ends");
    }
}
```

**Explanation:**
- **`CompletableFuture.runAsync()`**: Executes a task asynchronously without blocking the main thread.
- **`future.join()`**: Blocks the main thread until the asynchronous task is completed.

### More Complex Example: Asynchronous Computation

Let's say you want to perform two independent asynchronous computations and then combine their results.

**Example:**

```java
import java.util.concurrent.CompletableFuture;

public class AsyncComputationExample {
    public static void main(String[] args) {
        System.out.println("Starting asynchronous computations...");

        // First asynchronous computation
        CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return 10;
        });

        // Second asynchronous computation
        CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return 20;
        });

        // Combine results of both computations
        CompletableFuture<Integer> combinedFuture = future1.thenCombine(future2, (result1, result2) -> result1 + result2);

        // Block and get the result of the combined computation
        int combinedResult = combinedFuture.join();

        System.out.println("Combined Result: " + combinedResult);
    }
}
```

**Explanation:**
- **`CompletableFuture.supplyAsync()`**: Used for tasks that return a result. Each computation runs asynchronously.
- **`thenCombine()`**: Combines the results of two independent asynchronous tasks once both are completed.
- **`join()`**: Blocks the main thread until the combined result is ready.

### Asynchronous Operations in Java

- **I/O Operations**: Java's `java.nio` package provides non-blocking I/O operations, allowing you to perform file or network I/O asynchronously.
- **Asynchronous Servlets**: In Java EE, you can use asynchronous servlets to handle long-running tasks without blocking the servlet's thread pool.
- **Executors**: The `ExecutorService` in Java provides a higher-level API for managing threads and tasks, supporting both synchronous and asynchronous task execution.

### Asynchronous vs Synchronous in Java

- **Synchronous**: Methods execute in a sequence, and each method must complete before the next one starts.
- **Asynchronous**: Methods start tasks that run in parallel to other tasks, allowing the program to continue running other tasks without waiting for the current task to complete.

### Lifecycle of a CompletableFuture (Similar to Promise in JavaScript)

- **Pending**: The `CompletableFuture` is created but not yet completed.
- **Completed**: The task has finished, and the result (or exception) is available.

**States**:
1. **Pending**: When the `CompletableFuture` is still running or waiting for the task to complete.
2. **Completed**: When the task is done, either successfully (fulfilled) or with an exception (rejected).

### Example: Full Lifecycle of CompletableFuture

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureLifecycleExample {
    public static void main(String[] args) {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            System.out.println("Processing task...");
            try {
                Thread.sleep(2000); // Simulate a long-running task
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "Task result";
        });

        // Attach a callback to be called upon completion
        future.thenAccept(result -> {
            System.out.println("Received result: " + result);
        });

        // Attach a callback to handle any errors
        future.exceptionally(ex -> {
            System.out.println("Error occurred: " + ex.getMessage());
            return null;
        });

        System.out.println("Main thread continues...");

        // Block until the future completes
        future.join();

        System.out.println("Main thread ends.");
    }
}
```

**Explanation:**
- **`supplyAsync()`**: Asynchronously runs the provided task.
- **`thenAccept()`**: Attaches a callback that executes when the task is completed successfully.
- **`exceptionally()`**: Attaches a callback to handle any exceptions that may occur during the task's execution.
- **`join()`**: Blocks the main thread until the asynchronous task is completed.

### Visual Representation: CompletableFuture Lifecycle

```plaintext
Start
  |
Pending -----> (Success) -----> Fulfilled (Result: "Task result")
  |                       
  -----> (Error) -----> Rejected (Error: "Some error occurred")
```

### Conclusion

Java provides robust mechanisms for asynchronous programming, from traditional thread management to modern, flexible approaches like `CompletableFuture`. Asynchronous operations in Java allow programs to be more responsive and efficient by not blocking the main thread while waiting for tasks to complete.