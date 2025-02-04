
# Episode 20: Callback Functions in JavaScript

## Understanding Callback Functions

### What is a Callback Function?

A callback function is simply a function that you pass as an argument to another function, and it gets executed after the completion of that function. 

Imagine you have a task that you need someone else to finish before you can continue with your work. You give them instructions (the callback function), and once they’re done, they follow your instructions to finish the job.

### Simple Example

Let's start with a simple example to demonstrate how a callback works:

```javascript
function greet(name) {
    console.log("Hello " + name);
}

function sayGoodbye(name) {
    console.log("Goodbye " + name);
}

function performAction(action, name) {
    action(name); // Here, the `action` is the callback function that gets executed.
}

performAction(greet, "Alice");    // Output: Hello Alice
performAction(sayGoodbye, "Bob"); // Output: Goodbye Bob
```

In the above code:
- `greet` and `sayGoodbye` are two simple functions.
- `performAction` is a higher-order function that accepts another function (`action`) as an argument and executes it.
- We pass `greet` and `sayGoodbye` as callbacks to `performAction`, which then executes them.

### The Good Part of Callbacks

Callbacks are super important when writing asynchronous code in JavaScript. 

**Asynchronous** means that something happens in the background and doesn't block the rest of your code from running. For example, if you’re waiting for a response from a server, you can use a callback function to execute some code only after you get the response.

### The Bad Part of Callbacks

However, using callbacks can lead to issues like:

1. **Callback Hell**: When you have multiple nested callbacks, making the code hard to read and maintain.
2. **Inversion of Control**: When you give up control of your code to another function, which can be risky.

### JavaScript: A Synchronous Language

JavaScript is a **synchronous** and **single-threaded** language. This means it can only do one thing at a time, in the order that you write it. Think of it like a single-person assembly line: they pick up one item, work on it, finish it, and then move on to the next one.

```javascript
console.log("Hello");
console.log("World");
console.log("!");

// Output:
// Hello
// World
// !
```

JavaScript quickly prints each line because it doesn’t wait for anything—it just moves to the next line immediately.

### Introducing Asynchronous Behavior with Callbacks

But what if you want to delay something? You can use a callback function to do that.

```javascript
console.log("Hello");

setTimeout(function () {
  console.log("World");
}, 2000); // 2000 milliseconds = 2 seconds

console.log("!");

// Output:
// Hello
// !
// World (after 2 seconds)
```

Here, `setTimeout` is a function that waits for 2 seconds before running the callback function inside it. So, "World" is printed last, even though it’s written second in the code.

---

## A Real-World Example: e-Commerce Website

Imagine you’re buying something online, like shoes and a shirt. When you place an order, the following steps might happen:

1. **Create the Order**: The order is created in the system.
2. **Proceed to Payment**: You’re charged for the order.

Here’s how you might write this in JavaScript:

```javascript
const cart = ["shoes", "shirt"];

// Step 1: Create Order
api.createOrder(cart);

// Step 2: Proceed to Payment
api.proceedToPayment();
```

But what if creating the order takes some time (e.g., it needs to talk to a server)? You want to make sure the order is created before moving on to payment.

This is where callbacks can help:

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment();
});
```

Here, the `createOrder` function is passed a callback function—`api.proceedToPayment()`—which will only run after the order is successfully created.

### Complicating the Scenario

Now, let’s make it more complex. After payment, you want to show the user their order summary:

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary();
  });
});
```

And let’s say you also want to update the user’s wallet balance after showing the order summary:

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary(function () {
      api.updateWallet();
    });
  });
});
```

This pattern, where callbacks are nested within other callbacks, is called **Callback Hell**. The code becomes difficult to read and maintain, and it’s easy to make mistakes. It’s also known as the **Pyramid of Doom** because of how the code structure looks.

---

## Inversion of Control: The Risk with Callbacks

When using callbacks, you’re handing control of your code to another function, which can be risky. 

### What is Inversion of Control?

**Inversion of Control** happens when you give control of your function (e.g., when and how it runs) to another function, which can lead to unexpected behavior.

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment();
});
```

In this example, you’re trusting that `api.createOrder` will correctly call `api.proceedToPayment()` when it’s done. But what if it doesn’t? What if it calls it twice, or not at all? This can lead to bugs that are hard to find and fix.

---

## Summary

- **Callbacks** are essential for handling asynchronous tasks in JavaScript, but they come with challenges like **Callback Hell** and **Inversion of Control**.
- Understanding these challenges is crucial for learning about **Promises**, which we’ll cover in the next session as a solution to these problems.

---

By understanding these concepts and examples, you’ll be better equipped to handle asynchronous code in JavaScript, especially as you move on to more advanced topics like Promises and async/await.

----

# Episode 20 : Callback

- There are 2 Parts of Callback:

  1. Good Part of callback - Callback are super important while writing asynchronous code in JS
  2. Bad Part of Callback - Using callback we can face issue:
     - Callback Hell
     - Inversion of control

- Understanding of Bad part of callback is super important to learn Promise in next lecture.

> 💡 JavaScript is synchronous, single threaded language. It can Just do one thing at a time, it has just one call-stack and it can execute one thing at a time. Whatever code we give to Javascript will be quickly executed by Javascript engine, it does not wait.

```js
console.log("Namaste");
console.log("JavaScript");
console.log("Season 2");
// Namaste
// JavaScript
// Season 2

// 💡 It is quickly printing because `Time, tide & Javascript waits for none.`
```

_But what if we have to delay execution of any line, we could utilize callback, How?_

```js
console.log("Namaste");
setTimeout(function () {
  console.log("JavaScript");
}, 5000);
console.log("Season 2");
// Namaste
// Season 2
// JavaScript

// 💡 Here we are delaying the execution using callback approach of setTimeout.
```

### 🛒 e-Commerce web app situation

Assume a scenario of e-Commerce web, where one user is placing order, he has added items like, shoes, pants and kurta in cart and now he is placing order. So in backend the situation could look something like this.

```js
const cart = ["shoes", "pants", "kurta"];
// Two steps to place a order
// 1. Create a Order
// 2. Proceed to Payment

// It could look something like this:
api.createOrder();
api.proceedToPayment();
```

Assumption, once order is created then only we can proceed to payment, so there is a dependency. So How to manage this dependency.
Callback can come as rescue, How?

```js
api.createOrder(cart, function () {
  api.proceedToPayment();
});
// 💡 Over here `createOrder` api is first creating a order then it is responsible to call `api.proceedToPayment()` as part of callback approach.
```

To make it a bit complicated, what if after payment is done, you have to show Order summary by calling `api.showOrderSummary()` and now it has dependency on `api.proceedToPayment()`
Now my code should look something like this:

```js
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary();
  });
});
```

Now what if we have to update the wallet, now this will have a dependency over `showOrderSummary`

```js
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary(function () {
      api.updateWallet();
    });
  });
});
// 💡 Callback Hell
```

When we have a large codebase and multiple apis and have dependency on each other, then we fall into callback hell.
These codes are tough to maintain.
These callback hell structure is also known as **Pyramid of Doom**.

Till this point we are comfortable with concept of callback hell but now lets discuss about `Inversion of Control`. It is very important to understand in order to get comfortable around the concept of promise.

> 💡 Inversion of control is like that you lose the control of code when we are using callback.

Let's understand with the help of example code and comments:

```js
api.createOrder(cart, function () {
  api.proceedToPayment();
});

// 💡 So over here, we are creating a order and then we are blindly trusting `createOrder` to call `proceedToPayment`.

// 💡 It is risky, as `proceedToPayment` is important part of code and we are blindly trusting `createOrder` to call it and handle it.

// 💡 When we pass a function as a callback, basically we are dependant on our parent function that it is his responsibility to run that function. This is called `inversion of control` because we are dependant on that function. What if parent function stopped working, what if it was developed by another programmer or callback runs two times or never run at all.

// 💡 In next session, we will see how we can fix such problems.
```

> 💡 Async programming in JavaScript exists because callback exits.

more at `http://callbackhell.com/`

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=yEKtJGha3yM&list=PLlasXeu85E9eWOpw9jxHOQyGMRiBZ60aX" target="_blank"><img src="https://img.youtube.com/vi/yEKtJGha3yM/0.jpg" width="750"
alt="callback Youtube Link"/></a>
