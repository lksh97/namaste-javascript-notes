### Episode 22: Creating a Promise, Chaining & Error Handling

In this episode, we dive deep into how to create, chain, and handle errors with promises in JavaScript. This topic is crucial for managing asynchronous operations in a clean and maintainable way.

---

### **Understanding Promises**

Imagine you're at a restaurant, and you place an order with a waiter. The waiter takes your order (this is like a function call), but you don't get your food immediately. Instead, you get a receipt with a promise that your food will be delivered. This receipt can either be fulfilled (food arrives), or if something goes wrong (like the chef burns the food), you get an error.

In JavaScript, a **promise** is like that receipt. It’s an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

### **Example: Creating a Promise**

Let’s break down a simple example where we create an order and process it using promises:

```javascript
const cart = ["shoes", "pants", "kurta"];

// Consumer part of promise
const promise = createOrder(cart); // Order processing begins, and we get a promise in return.

promise.then(function (orderId) {
  proceedToPayment(orderId);
});
```

Here, `createOrder` is a function that initiates the order process and returns a promise. The `.then()` method is used to handle what happens after the promise is fulfilled.

But what happens inside `createOrder` that makes it return a promise? Let's look at the implementation:

### **Creating the Promise (Producer Part)**

```javascript
function createOrder(cart) {
  // JS provides a Promise constructor to create a promise.
  // It accepts a callback function with two parameters: `resolve` & `reject`.
  const promise = new Promise(function (resolve, reject) {
    // `resolve` and `reject` are functions provided by JavaScript to handle the success and failure of the operation.

    // Mock logic steps:
    // 1. Validate the cart.
    // 2. Insert the order in the database and get an orderId.

    if (!validateCart(cart)) {
      // If the cart is not valid, reject the promise.
      const err = new Error("Cart is not valid");
      reject(err);
    }

    const orderId = "12345"; // Assume this ID is generated from the database.
    if (orderId) {
      // Success scenario: resolve the promise with the orderId.
      resolve(orderId);
    }
  });

  return promise;
}
```

In this function, `createOrder`, we use the `Promise` constructor to create a promise. The constructor takes a function that has two parameters: `resolve` and `reject`.

- **`resolve(value)`**: This is called when the operation is successful, and it resolves the promise with a value.
- **`reject(error)`**: This is called when something goes wrong, and it rejects the promise with an error.

### **State of Promises**

Initially, when you call `createOrder`, the promise is in a **pending** state. This means it's neither resolved nor rejected yet. When the operation completes:

- If successful, the promise moves to the **fulfilled** state.
- If it fails, it moves to the **rejected** state.

### **Example: Handling Promise Resolution**

```javascript
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart);

// What will be printed here?
// It prints Promise {<pending>}, because `createOrder` takes some time to resolve.

console.log(promise);

promise.then(function (orderId) {
  proceedToPayment(orderId);
});
```

Here, the promise is still pending when we log it because `createOrder` hasn't completed its operation yet.

### **Error Handling with `.catch()`**

What if something goes wrong? We can handle errors in promises using `.catch()`.

```javascript
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart);

promise
  .then(function (orderId) {
    proceedToPayment(orderId);
  })
  .catch(function (err) {
    console.log("Error:", err.message);
  });
```

In this example, if `validateCart` fails, the promise is rejected, and the `.catch()` block will handle the error.

### **Promise Chaining**

Promise chaining allows you to execute multiple asynchronous operations in sequence, where each operation depends on the result of the previous one.

```javascript
createOrder(cart)
  .then(function (orderId) {
    console.log("Order ID:", orderId);
    return orderId;
  })
  .then(function (orderId) {
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    console.log("Payment Info:", paymentInfo);
  })
  .catch(function (err) {
    console.log("Error:", err.message);
  });
```

In this chain:
- The first `.then()` gets the `orderId` and returns it.
- The second `.then()` uses that `orderId` to proceed to payment and returns the payment info.
- The third `.then()` logs the payment info.

If any promise in the chain is rejected, the chain is short-circuited, and the `.catch()` block handles the error.

### **Multiple `.catch()` Blocks**

Sometimes, you may want to handle errors at different stages in the chain. You can place multiple `.catch()` blocks:

```javascript
createOrder(cart)
  .then(function (orderId) {
    console.log("Order ID:", orderId);
    return orderId;
  })
  .catch(function (err) {
    console.log("Order failed:", err.message);
    // If order creation fails, stop further processing.
  })
  .then(function (orderId) {
    return proceedToPayment(orderId);
  })
  .catch(function (err) {
    console.log("Payment failed:", err.message);
  });
```

In this example:
- The first `.catch()` handles errors related to order creation.
- The second `.catch()` handles errors related to payment processing.

### **Key Takeaways for Beginners**
1. **Promise Basics**: Promises are a way to manage asynchronous operations in JavaScript. They can be either fulfilled (resolved) or rejected based on the outcome of the operation.
  
2. **Creating Promises**: You can create promises using the `Promise` constructor. It takes a function with `resolve` and `reject` to handle success and failure.

3. **Consuming Promises**: Use `.then()` to handle the success and `.catch()` to handle errors. These methods allow you to manage what happens after the promise is fulfilled or rejected.

4. **Chaining Promises**: Promises can be chained together to perform a series of operations where each step depends on the previous one. If any step fails, the chain is broken, and the error is caught by the `.catch()` block.

5. **Error Handling**: Errors in promises can be handled at different stages using multiple `.catch()` blocks, giving you fine-grained control over error management.

6. **Pending State**: Initially, a promise is in a pending state, meaning it is waiting to be either fulfilled or rejected.

7. **Text Diagrams**: Imagine a flowchart where each step is a promise that leads to the next. If any step fails, an arrow points to an error handler that catches the mistake.

### **Visualizing the Flow**

Here’s a simple diagram to help you visualize how promises work:

```
+-------------+       +-----------+       +-------------+
| Create Order| ----> | Process   | ----> | Payment      |
| (Pending)   |       | (Fulfilled|       | (Fulfilled)  |
+-------------+       +-----------+       +-------------+
       |                    |
       |                    v
       |             +-------------+
       |             |  .catch()   |
       |             | (Rejected)  |
       v             +-------------+
+-------------+
|  .catch()   |
| (Rejected)  |
+-------------+
```

---

### **Handling Multiple Promises with `Promise.all`**

Often in real-world applications, you might need to perform multiple asynchronous operations simultaneously and proceed only when all of them are completed. JavaScript provides a method called `Promise.all` for this purpose.

#### **Example:**

Let's say you need to fetch data from three different APIs before proceeding with some logic.

```javascript
const promise1 = fetchDataFromAPI1(); // Returns a promise
const promise2 = fetchDataFromAPI2(); // Returns a promise
const promise3 = fetchDataFromAPI3(); // Returns a promise

Promise.all([promise1, promise2, promise3])
  .then(function (results) {
    // All promises are resolved
    const [result1, result2, result3] = results;
    console.log("All data fetched successfully:", result1, result2, result3);
  })
  .catch(function (error) {
    // If any of the promises fail, this block will be executed
    console.log("Failed to fetch data from one or more APIs:", error);
  });
```

#### **Explanation:**

- **`Promise.all()`** takes an array of promises as an argument and returns a new promise.
- If all the promises are resolved, it returns an array of results, corresponding to each promise in the order they were passed in.
- If any of the promises are rejected, `Promise.all()` immediately rejects, and the `.catch()` block is executed.

**Visualizing `Promise.all`:**

```
+---------------+ +---------------+ +---------------+
| Fetch Data 1  | | Fetch Data 2  | | Fetch Data 3  |
| (Pending)     | | (Pending)     | | (Pending)     |
+-------+-------+ +-------+-------+ +-------+-------+
        |                 |                 |
        v                 v                 v
+----------------------------+     +----------------------------+
|   All Promises Resolved     |     |    Any Promise Rejected    |
| (Proceed with Logic)        |     | (Handle Error)             |
+----------------------------+     +----------------------------+
```

### **Combining `Promise.all` with Chaining**

You can also combine `Promise.all` with promise chaining for more complex scenarios.

#### **Example:**

Imagine you first need to validate a cart, and then fetch user details and shipping details simultaneously before proceeding to place the order.

```javascript
validateCart(cart)
  .then(function (cartValidated) {
    if (cartValidated) {
      return Promise.all([fetchUserDetails(), fetchShippingDetails()]);
    } else {
      throw new Error("Cart validation failed");
    }
  })
  .then(function ([userDetails, shippingDetails]) {
    // Both user details and shipping details are fetched
    console.log("User:", userDetails);
    console.log("Shipping:", shippingDetails);
    return placeOrder(userDetails, shippingDetails);
  })
  .then(function (orderConfirmation) {
    console.log("Order placed successfully:", orderConfirmation);
  })
  .catch(function (error) {
    console.log("Error occurred:", error.message);
  });
```

**Explanation:**

- First, `validateCart` is called and returns a promise. If the cart is valid, the promise is resolved.
- `Promise.all` is then used to fetch user details and shipping details simultaneously.
- Once both operations are successful, the details are passed to `placeOrder`, which also returns a promise.
- The entire chain will catch any errors that occur at any step.

### **Error Propagation in Promise Chains**

An important concept in promise chains is error propagation. If an error occurs at any point in a chain, it will propagate down to the nearest `.catch()` block.

#### **Example:**

```javascript
createOrder(cart)
  .then(function (orderId) {
    // Let's say proceedToPayment fails
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    console.log("Payment Info:", paymentInfo);
  })
  .catch(function (error) {
    // This catch will handle any error from the chain
    console.log("Error:", error.message);
  });
```

**Explanation:**

- If `proceedToPayment` fails and the promise is rejected, the error will propagate down the chain.
- The nearest `.catch()` will handle the error, stopping further execution in the chain.

### **Advanced: Handling Independent Operations with `Promise.allSettled`**

Sometimes, you want to execute multiple promises where each promise is independent of the others, and you want to know the result of all of them regardless of success or failure. For this, `Promise.allSettled` is useful.

#### **Example:**

```javascript
const promise1 = fetchDataFromAPI1(); // This might succeed or fail
const promise2 = fetchDataFromAPI2(); // This might succeed or fail
const promise3 = fetchDataFromAPI3(); // This might succeed or fail

Promise.allSettled([promise1, promise2, promise3])
  .then(function (results) {
    results.forEach((result, index) => {
      if (result.status === "fulfilled") {
        console.log(`Promise ${index + 1} fulfilled with value:`, result.value);
      } else {
        console.log(`Promise ${index + 1} rejected with reason:`, result.reason);
      }
    });
  });
```

**Explanation:**

- **`Promise.allSettled()`** returns a promise that resolves after all the given promises have either resolved or rejected.
- The result is an array of objects, each having a `status` (`"fulfilled"` or `"rejected"`) and either a `value` (if fulfilled) or `reason` (if rejected).
- This method is useful when you need the outcome of every promise, regardless of whether they succeed or fail.

**Visualizing `Promise.allSettled`:**

```
+---------------+ +---------------+ +---------------+
| Fetch Data 1  | | Fetch Data 2  | | Fetch Data 3  |
| (Pending)     | | (Pending)     | | (Pending)     |
+-------+-------+ +-------+-------+ +-------+-------+
        |                 |                 |
        v                 v                 v
+--------------------------+ +--------------------------+ +--------------------------+
| Promise 1: Fulfilled      | | Promise 2: Rejected      | | Promise 3: Fulfilled      |
| (Handle Success or Fail)  | | (Handle Success or Fail) | | (Handle Success or Fail)  |
+--------------------------+ +--------------------------+ +--------------------------+
```

### **Conclusion**

- **Promises** in JavaScript are a powerful way to handle asynchronous operations, allowing you to write cleaner and more readable code.
- **Chaining** promises lets you perform operations in sequence, ensuring that each step is completed before moving on to the next.
- **Error handling** is crucial and can be managed using `.catch()` at different levels of the promise chain.
- For handling **multiple asynchronous operations**, `Promise.all` and `Promise.allSettled` provide ways to manage and respond to several promises simultaneously.

-----
# Original Notes -
-----
# Episode 22 : Creating a Promise, Chaining & Error Handling

###

```js
const cart = ["shoes", "pants", "kurta"];

// Consumer part of promise
const promise = createOrder(cart); // orderId
// Our expectation is above function is going to return me a promise.

promise.then(function (orderId) {
  proceedToPayment(orderId);
});

// Above snippet we have observed in our previous lecture itself.
// Now we will see, how createOrder is implemented so that it is returning a promise
// In short we will see, "How we can create Promise" and then return it.

// Producer part of Promise
function createOrder(cart) {
  // JS provides a Promise constructor through which we can create promise
  // It accepts a callback function with two parameter `resolve` & `reject`
  const promise = new Promise(function (resolve, reject) {
    // What is this `resolve` and `reject`?
    // These are function which are passed by javascript to us in order to handle success and failure of function call.
    // Now we will write logic to `createOrder`
    /** Mock logic steps
     * 1. validateCart
     * 2. Insert in DB and get an orderId
     */
    // We are assuming in real world scenario, validateCart would be defined
    if (!validateCart(cart)) {
      // If cart not valid, reject the promise
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345"; // We got this id by calling to db (Assumption)
    if (orderId) {
      // Success scenario
      resolve(orderId);
    }
  });
  return promise;
}
```

Over above, if your validateCart is returning true, so the above promise will be resolved (success),

```js
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart); // orderId
// ❓ What will be printed in below line?
// It prints Promise {<pending>}, but why?
// Because above createOrder is going to take sometime to get resolved, so pending state. But once the promise is resolved, `.then` would be executed for callback.
console.log(promise);

promise.then(function (orderId) {
  proceedToPayment(orderId);
});

function createOrder(cart) {
  const promise = new Promise(function (resolve, reject) {
    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345";
    if (orderId) {
      resolve(orderId);
    }
  });
  return promise;
}
```

Now let's see if there was some error and we are rejecting the promise, how we could catch that?  
-> Using `.catch`

```js
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart); // orderId

// Here we are consuming Promise and will try to catch promise error
promise
  .then(function (orderId) {
    // ✅ success aka resolved promise handling
    proceedToPayment(orderId);
  })
  .catch(function (err) {
    // ⚠️ failure aka reject handling
    console.log(err);
  });

// Here we are creating Promise
function createOrder(cart) {
  const promise = new Promise(function (resolve, reject) {
    // Assume below `validateCart` return false then the promise will be rejected
    // And then our browser is going to throw the error.
    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345";
    if (orderId) {
      resolve(orderId);
    }
  });
  return promise;
}
```

Now, Let's understand the concept of Promise Chaining  
-> for this we will assume after `createOrder` we have to invoke `proceedToPayment`  
-> In promise chaining, whatever is returned from first `.then` become data for next `.then` and so on...  
-> At any point of promise chaining, if promise is rejected, the execution will fallback to `.catch` and others promise won't run.

```js
const cart = ["shoes", "pants", "kurta"];

createOrder(cart)
  .then(function (orderId) {
    // ✅ success aka resolved promise handling
    // 💡 we have return data or promise so that we can keep chaining the promises, here we are returning data
    console.log(orderId);
    return orderId;
  })
  .then(function (orderId) {
    // Promise chaining
    // 💡 we will make sure that `proceedToPayment` returns a promise too
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    // from above, `proceedToPayment` is returning a promise so we can consume using `.then`
    console.log(paymentInfo);
  })
  .catch(function (err) {
    // ⚠️ failure aka reject handling
    console.log(err);
  });

// Here we are creating Promise
function createOrder(cart) {
  const promise = new Promise(function (resolve, reject) {
    // Assume below `validateCart` return false then the promise will be rejected
    // And then our browser is going to throw the error.
    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }
    const orderId = "12345";
    if (orderId) {
      resolve(orderId);
    }
  });
  return promise;
}

function proceedToPayment(cart) {
  return new Promise(function (resolve, reject) {
    // For time being, we are simply `resolving` promise
    resolve("Payment Successful");
  });
}
```

Q: What if we want to continue execution even if any of my promise is failing, how to achieve this?  
-> By placing the `.catch` block at some level after which we are not concerned with failure.  
-> There could be multiple `.catch` too.
Eg:

```js
createOrder(cart)
  .then(function (orderId) {
    // ✅ success aka resolved promise handling
    // 💡 we have return data or promise so that we can keep chaining the promises, here we are returning data
    console.log(orderId);
    return orderId;
  })
    .catch(function (err) {
    // ⚠️ Whatever fails below it, catch wont care
    // this block is responsible for code block above it.
    console.log(err);
  });
  .then(function (orderId) {
    // Promise chaining
    // 💡 we will make sure that `proceedToPayment` returns a promise too
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    // from above, `proceedToPayment` is returning a promise so we can consume using `.then`
    console.log(paymentInfo);
  })
```

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=U74BJcr8NeQ&list=PLlasXeu85E9eWOpw9jxHOQyGMRiBZ60aX&index=4&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/U74BJcr8NeQ/0.jpg" width="750"
alt="promise in Javascript Youtube Link"/></a>
