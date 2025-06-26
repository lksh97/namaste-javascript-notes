
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


---

## Episode 20: Callback Functions in JavaScript

### 1. Callback Functions Ka Basic Concept

**Kya hai Callback Function?**  
- **Callback Function** ek aisa function hota hai jo doosre function ke argument ke roop mein pass kiya jata hai.  
- Jab parent function apna kaam complete kar leta hai, tab callback function ko execute kiya jata hai.

**Real-world Analogy:**  
Socho aapko ek kaam complete karna hai lekin pehle aapne kisi ko bola ke "jab tum yeh kaam complete kar do, mujhe batana" – yeh "batana" ka process callback function ki tarah hota hai.

**Simple Example:**

```javascript
// Ye function simply kisi naam ko greet karta hai
function greet(name) {
  console.log("Hello " + name);
}

// Ye function kisi naam se goodbye kehta hai
function sayGoodbye(name) {
  console.log("Goodbye " + name);
}

// Ye function ek higher-order function hai jo callback function ko argument ke roop mein leta hai
function performAction(action, name) {
  action(name); // "action" yaha callback function hai jo pass hua hai
}

// Callback functions ko call karna
performAction(greet, "Alice");    // Output: Hello Alice
performAction(sayGoodbye, "Bob");   // Output: Goodbye Bob
```

**Detailed Explanation:**
- **greet()** aur **sayGoodbye()** simple functions hain.
- **performAction()** ek higher-order function hai, jo kisi bhi function ko (jo hum callback ke roop mein dete hain) execute karta hai.
- Jab hum **greet** aur **sayGoodbye** ko performAction ke argument ke roop mein pass karte hain, tab woh function unhe call karta hai.

---

### 2. Callbacks in Asynchronous Code

JavaScript synchronous aur single-threaded language hai, matlab ek samay mein sirf ek kaam ho sakta hai. Lekin jab hume asynchronous tasks (jaise network request, timer) perform karne hote hain, tab callback functions bahut useful hote hain.

#### Example: setTimeout ke saath

```javascript
console.log("Namaste");  // Sabse pehle "Namaste" print hoga

// setTimeout asynchronous Web API hai, jo 5000ms (5 sec) ke baad callback execute karega
setTimeout(function () {
  console.log("JavaScript");
}, 5000);

console.log("Season 2");  // Turant "Season 2" print hoga

// Output:
// Namaste
// Season 2
// (5 sec ke baad) JavaScript
```

**Explanation:**
- "Namaste" aur "Season 2" synchronous code hain, jo call stack mein execute hote hain.
- **setTimeout()** function browser ke Web API se call hota hai. Jab timer expire hota hai, callback function ko Callback Queue mein bhej diya jata hai.
- **Event Loop** check karta hai ki call stack empty hai, tab callback function ko call stack mein push karke execute karta hai.

---

### 3. Event Loop, Callback Queue, & Microtask Queue

#### **Event Loop:**

- **Event Loop** ek infinite loop hai jo continuously check karta hai:  
  - Kya call stack khali hai?  
  - Agar haan, to microtask queue ya callback queue se pending functions ko call stack mein bhejta hai.

**Diagram:**

```
         +---------------------+
         |     Call Stack      |
         |  (Synchronous Code) |
         +---------------------+
                  ▲
                  │ (When empty)
         +---------------------+
         |    Microtask Queue  |   <-- Promises & MutationObservers
         +---------------------+
                  │
         +---------------------+
         |   Callback Queue    |   <-- setTimeout, DOM events, etc.
         +---------------------+
```

#### **Microtask Queue vs. Callback Queue:**

- **Microtask Queue** mein promise ke callbacks aur mutation observers ke callbacks jate hain.  
- **Callback Queue** mein setTimeout ke jaise asynchronous tasks jate hain.
- **Priority:** Microtask queue ke tasks, callback queue ke tasks se pehle execute hote hain.

**Example:**

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout Callback"); // Ye callback callback queue mein jayega
}, 0);

Promise.resolve().then(() => {
  console.log("Promise Callback"); // Ye microtask queue mein jayega, jiska priority zyada hai
});

console.log("End");

// Expected Output:
// Start
// End
// Promise Callback  <-- Microtask priority
// Timeout Callback  <-- Callback Queue ke baad
```

---

### 4. Real-World Example: e-Commerce Order Processing

**Scenario:**  
User online shopping karta hai, order place karta hai, phir payment process hota hai, order summary dikhai jati hai, aur finally wallet update hota hai.

**Without Callbacks (Synchronous Flow):**

```javascript
api.createOrder(cart);
api.proceedToPayment();
api.showOrderSummary();
api.updateWallet();
```

**Using Callbacks to Handle Dependencies:**

```javascript
api.createOrder(cart, function () {
  // Order create hone ke baad, payment process karo
  api.proceedToPayment(function () {
    // Payment complete hone ke baad, order summary dikhayo
    api.showOrderSummary(function () {
      // Order summary dekhne ke baad, wallet update karo
      api.updateWallet();
    });
  });
});
```

**Issue:**  
Ye nested structure, jise **Callback Hell** ya **Pyramid of Doom** kehte hain, bahut complex ho jata hai jab callbacks kaafi nest ho jate hain.

---

### 5. Inversion of Control in Callbacks

**Concept:**  
- **Inversion of Control (IoC)** ka matlab hai ke jab aap callback function ko kisi doosre function ko pass karte hain, toh aap us function par control de dete hain.  
- Iska risk yeh hai ki agar parent function galat se implement ho jaye, toh callback kab execute hoga, yeh aapke control se bahar ho sakta hai.

**Example:**

```javascript
api.createOrder(cart, function () {
  // Yahan aap blindly trust kar rahe ho ke createOrder callback ko sahi se call karega
  api.proceedToPayment();
});
```

**Explanation:**  
- Agar `createOrder` function callback ko sahi tarah se call nahi karta, toh payment process nahi hoga.  
- Isse dependency create hoti hai jahan aap parent function par control chhod dete hain.

---

### 6. Summary

- **Callback Functions**: Ek function jo argument ke roop mein pass hota hai aur parent function ke complete hone par execute hota hai.
- **Asynchronous Behavior**: Callbacks ka use asynchronous operations (jaise setTimeout, fetch) mein hota hai.
- **Event Loop**: Call stack, microtask queue, aur callback queue ke beech ka mechanism hai jo asynchronous callbacks ko execute karta hai.
- **Callback Hell**: Jab multiple callbacks nested hote hain, code complex aur maintain karna mushkil ho jata hai.
- **Inversion of Control**: Callback functions par control lose ho jata hai jab aap dependent functions ko pass karte hain.

---

### Text Diagram (Simplified):

```
   [Global Execution Context]
           |
           v
+-----------------------------+
| Synchronous Code            |  <-- console.log("Start"), etc.
+-----------------------------+
           |
           v
+-----------------------------+
| Web APIs (setTimeout, etc.) |  <-- Timer, DOM APIs, etc.
+-----------------------------+
           |
           v
+-----------------------------+
| Callback Queue              |  <-- setTimeout callbacks, etc.
+-----------------------------+
           |
           v
+-----------------------------+
| Microtask Queue             |  <-- Promises, MutationObserver callbacks
+-----------------------------+
           |
           v
     [Event Loop] <-- Checks call stack; executes microtasks first, then callbacks
```

---

### 7. Conclusion

- Callback functions JavaScript mein asynchronous tasks handle karne ke liye essential hain.  
- Inka sahi istemal karke hum apne code ko asynchronous bana sakte hain, lekin nesting se bachna zaroori hai (Callback Hell).  
- Event loop, callback queue, aur microtask queue ke concepts ko samajhna asynchronous programming ke liye bahut important hai.

---

# Episode 20: Callback Functions in JavaScript

---

## 1. Understanding Callback Functions

### What is a Callback Function?
A callback function ek aisi function hai jo aap kisi doosri function ko argument ke roop mein pass karte ho. Jab woh doosri function apna kaam complete kar leti hai, tab callback function ko execute kiya jata hai.  
**Analogy:**  
Socho aap ek kaam kar rahe ho aur aap kisi dost se kehte ho, "Mujhe bata dena jab tum apna kaam complete kar lo." Yahaan, aapka dost callback function hai jo tab chalti hai jab kaam ho jata hai.

---

### Simple Example of Callback

```javascript
// Ye function ek naam leta hai aur greet karta hai
function greet(name) {
  console.log("Hello " + name);
}

// Ye function ek naam leta hai aur goodbye kehta hai
function sayGoodbye(name) {
  console.log("Goodbye " + name);
}

// Ye function higher-order function hai jo doosri function (callback) ko execute karta hai
function performAction(action, name) {
  // Yahan 'action' parameter callback function hai, jo pass hua hai
  action(name);
}

// Ab hum callback functions ko performAction ke through execute karte hain
performAction(greet, "Alice");    // Output: Hello Alice
performAction(sayGoodbye, "Bob");   // Output: Goodbye Bob
```

**Inline Comments Explanation (Hinglish):**

- **`function greet(name) { ... }`**  
  Ye function "greet" hai jo kisi naam ko greet karta hai. Isme `name` parameter diya gaya hai aur console.log se "Hello" message print hota hai.

- **`function sayGoodbye(name) { ... }`**  
  Ye function "sayGoodbye" hai jo kisi naam ko bye kehta hai.

- **`function performAction(action, name) { ... }`**  
  Ye ek higher-order function hai jo do parameters leta hai:  
  - `action`: Ye ek function (callback) hai jo execute hoga.  
  - `name`: Ye parameter callback function ke liye pass hoga.
  
- **`action(name);`**  
  Yahan par `action` function ko call kiya jata hai, jo ki callback hai, aur usme `name` diya jata hai.

- **`performAction(greet, "Alice");`**  
  Yahan hum greet function ko callback ke roop mein pass kar rahe hain aur "Alice" naam de rahe hain. Output: "Hello Alice".

- **`performAction(sayGoodbye, "Bob");`**  
  Yahan hum sayGoodbye function ko callback ke roop mein pass kar rahe hain aur "Bob" naam de rahe hain. Output: "Goodbye Bob".

---

## 2. Asynchronous Behavior with Callbacks

JavaScript by default synchronous (ek ke baad ek execute hota hai) aur single-threaded hai. Lekin asynchronous operations ki wajah se hum delay ya background tasks execute kar sakte hain bina main code ko block kiye.

### Example: Using setTimeout()

```javascript
console.log("Namaste"); // Sabse pehle ye execute hoga

// setTimeout asynchronous function hai jo 5000ms (5 seconds) ke baad callback function ko execute karega
setTimeout(function () {
  console.log("JavaScript"); // Ye 5 seconds ke baad execute hoga
}, 5000);

console.log("Season 2"); // Ye turant execute hoga

// Expected Output:
// Namaste
// Season 2
// (5 seconds baad) JavaScript
```

**Inline Comments Explanation:**

- **`console.log("Namaste");`**  
  Ye line synchronously print karegi "Namaste" to the console.

- **`setTimeout(function () { ... }, 5000);`**  
  Ye function browser ke Web API se aata hai. Ye callback function ko 5 seconds ke baad call karega.  
  - **Callback function:** `function () { console.log("JavaScript"); }`  
    Ye callback setTimeout ke baad call hoga.
  
- **`console.log("Season 2");`**  
  Ye line immediately execute ho jati hai, kyunki setTimeout asynchronous hai aur call stack pe delay hota hai.

---

## 3. Real-World Example: e-Commerce Order Process

### Scenario:
User online order place karta hai (shoes, pants, kurta). Order creation aur payment ke beech dependency hai.

**Without Callback (Synchronous – Incorrect):**
```javascript
const cart = ["shoes", "pants", "kurta"];
api.createOrder(cart);         // Order create hota hai, lekin yahan delay ho sakta hai
api.proceedToPayment();        // Ye immediately call ho jata hai, order ready nahi hua ho sakta
```

**With Callback (Proper Dependency Handling):**
```javascript
// Order create hone ke baad hi payment process ho, isliye callback use karte hain
api.createOrder(cart, function () {
  // Ye function tab call hoga jab order successfully create ho jayega
  api.proceedToPayment();
});
```

**Nested Callbacks (Callback Hell):**
```javascript
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary(function () {
      api.updateWallet(function () {
        console.log("All steps completed!");
      });
    });
  });
});
```

**Explanation:**
- **Callback Hell:** Jab callbacks nested hote hain, code deeply indented ho jata hai (Pyramid of Doom), jo padhna aur maintain karna mushkil ho jata hai.
- **Solution:** Promises ya async/await ka use karo, jo cleaner aur linear code structure dete hain.

---

## 4. Solving Callback Hell: Promises & async/await

### Using Promises:

```javascript
api.createOrder(cart)
  .then(() => {
    return api.proceedToPayment();
  })
  .then(() => {
    return api.showOrderSummary();
  })
  .then(() => {
    return api.updateWallet();
  })
  .then(() => {
    console.log("All steps completed!");
  })
  .catch((error) => {
    console.error("An error occurred:", error);
  });
```

**Inline Comments:**
- **`.then()`**: Har then block mein, previous promise resolve hone ke baad next function call hota hai.
- **`.catch()`**: Agar koi error aaye to ye block error handle karta hai.

### Using async/await:

```javascript
async function processOrder(cart) {
  try {
    await api.createOrder(cart);         // Order create hone tak wait karega
    await api.proceedToPayment();          // Payment process hone tak wait karega
    await api.showOrderSummary();          // Order summary dikhane tak wait karega
    await api.updateWallet();              // Wallet update hone tak wait karega
    console.log("All steps completed!");
  } catch (error) {
    console.error("An error occurred:", error);
  }
}

processOrder(cart);
```

**Inline Comments:**
- **async function:** Ye function asynchronous hai aur promises ko await karta hai.
- **await:** Ye keyword promise ko resolve hone tak wait karta hai, aur code ko linear banata hai.
- **try/catch:** Sari error handling try/catch block mein hoti hai, jo code ko clean banata hai.

---

## 5. Event Loop, Callback Queue & Microtask Queue (Priority Order)

**Event Loop ka kaam:**  
JavaScript mein call stack hota hai jahan synchronous code execute hota hai. Jab call stack khali hota hai, event loop microtask queue check karta hai (promises, async callbacks) aur phir callback queue (setTimeout, DOM events) se functions execute karta hai.

### Priority Order:

1. **Microtask Queue:**
   - Promises ke callbacks (then/catch/finally)
   - async/await ke callbacks
   - MutationObserver callbacks
2. **Callback (Task) Queue:**
   - setTimeout, setInterval callbacks
   - DOM event handlers

**Diagram:**

```
   [Call Stack]  <-- Synchronous Code
         │
         ▼
  [Microtask Queue]  <-- Promises, async/await callbacks, MutationObserver
         │
         ▼
  [Callback Queue]   <-- setTimeout, setInterval, DOM events
         │
         ▼
    [Event Loop]     <-- Checks queues and pushes tasks to call stack when empty
```

**Why this order?**  
- Microtasks ko priority isliye di jati hai taki promise resolution jaldi ho aur state consistent rahe.
- Callbacks (setTimeout) lower priority hain kyunki ye intentionally delay ke liye use hote hain.

---

## 6. Summary

- **Callback Functions:**  
  Functions jo doosre functions ko argument ke roop mein diye jate hain aur specific events ke complete hone par execute hote hain.
  
- **Asynchronous Operations:**  
  JavaScript mein asynchronous tasks, jaise network requests ya timers, Web APIs ke through execute hote hain. In callbacks ko event loop ke through manage kiya jata hai.
  
- **Event Loop Priority:**  
  Microtask Queue (promises, async/await) pehle execute hota hai, uske baad Callback Queue (setTimeout, DOM events). Yeh order ensure karta hai ki critical asynchronous tasks jaldi execute ho jayein.
  
- **Callback Hell aur Inversion of Control:**  
  Nested callbacks (Callback Hell) se code mushkil ho jata hai. Iska solution hai Promises aur async/await, jo linear aur maintainable code structure dete hain.
  
- **Real-World Example:**  
  e-Commerce order processing example se humne dependency samjha aur bataya kaise callbacks ka istemal karte hain.

---

## Example Code with Detailed Hinglish Comments

Below is a consolidated code example with detailed inline Hinglish comments explaining each part:

```javascript
// Simple Callback Example
function greet(name) {
  // Ye function greet karta hai aur console pe "Hello" message print karta hai.
  console.log("Hello " + name);
}

function sayGoodbye(name) {
  // Ye function bye kehta hai aur console pe "Goodbye" message print karta hai.
  console.log("Goodbye " + name);
}

function performAction(action, name) {
  // Ye higher-order function hai jo dusri function (callback) ko call karega.
  // "action" parameter mein function pass kiya jata hai.
  action(name); // Callback function ko call kar rahe hain with parameter "name".
}

// Ab hum performAction ko call karte hain aur greet aur sayGoodbye ko callbacks ke roop mein pass karte hain.
performAction(greet, "Alice");    // Expected Output: Hello Alice
performAction(sayGoodbye, "Bob");   // Expected Output: Goodbye Bob
```

```javascript
// Asynchronous Callback Example using setTimeout
console.log("Namaste"); // Synchronous code: sabse pehle "Namaste" print hoga

// setTimeout asynchronous function hai. Ye callback function ko 5000ms (5 seconds) ke baad execute karega.
setTimeout(function () {
  console.log("JavaScript"); // Ye callback 5 seconds ke baad execute hoga.
}, 5000);

console.log("Season 2"); // Ye immediately execute hoga, kyunki setTimeout asynchronous hai

// Expected Console Output:
// Namaste
// Season 2
// (5 seconds baad) JavaScript
```

```javascript
// Real-World Example: e-Commerce Order Process with Callback Hell
api.createOrder(cart, function () {
  // Pehle order create ho raha hai.
  api.proceedToPayment(function () {
    // Jab order create ho jaye, payment process shuru hota hai.
    api.showOrderSummary(function () {
      // Payment ke baad order summary dikhai jati hai.
      api.updateWallet(function () {
        // Finally, wallet update hota hai.
        console.log("All steps completed!");
      });
    });
  });
});

// Note: Ye nested callbacks (Callback Hell) code ko samajhna mushkil bana dete hain.
```

```javascript
// Improved Approach using async/await (Solution to Callback Hell)
async function processOrder(cart) {
  try {
    // Await ka matlab hai ke ye line complete hone tak ruk jayega.
    await api.createOrder(cart);
    await api.proceedToPayment();
    await api.showOrderSummary();
    await api.updateWallet();
    console.log("All steps completed!");
  } catch (error) {
    // Agar koi error aata hai, to catch block use handle karega.
    console.error("An error occurred:", error);
  }
}

// Call the async function to process order
processOrder(cart);
```

---

## Conclusion

- **Callback Functions** help in executing code after a task is complete, but deeply nested callbacks (callback hell) make code hard to read.
- **Promises and async/await** offer cleaner, linear alternatives to manage asynchronous code.
- **Event Loop Priority:** Microtask queue (promises) is executed first, then callback queue (setTimeout, etc.).
- Understanding these concepts is essential for writing efficient asynchronous JavaScript code.

Is detailed explanation se aap asynchronous JavaScript, callback functions, aur event loop ka concept ache se samajh paayenge. Agar aur koi doubt ya question ho, toh puch sakte hain!