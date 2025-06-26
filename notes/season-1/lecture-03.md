# Episode 3 : Hoisting in JavaScript (variables & functions)

* Let's observe the below code and it's explaination:

```js
getName(); // Namaste Javascript

console.log(x); // undefined
var x = 7;

function getName() {
    console.log("Namaste Javascript");
}
```

- <ins>It should have been an outright error in many other languages</ins>, as it is not possible to even access something which is not even created (defined) yet.
    - But in JS, We know that in memory creation phase it assigns undefined and puts the content of function to function's memory. And in execution, it then executes whatever is asked.
    - Here, as execution goes line by line and not after compiling, it could only print undefined and nothing else. This phenomenon, is not an error. However, `if we remove var x = 7`; <ins>then it gives error</ins>.
        - **Uncaught ReferenceError :** x is not defined

- <ins>**Hoisting**</ins> is a concept which `enables us to extract values of variables and functions even before initialising/assigning value without getting error` and this is **happening due to the 1st phase** (memory creation phase) of the Execution Context.
    - So in previous lecture, we learnt that execution context gets created in two phase, so even before code execution, memory is created so in case of variable, **it will be initialized as undefined while in case of function the whole function code is placed in the memory**. Example:

```js
getName(); // Namaste JavaScript

console.log(x); // Uncaught Reference: x is not defined.

console.log(getName); // f getName(){ console.log("Namaste JavaScript); }

function getName(){
    console.log("Namaste JavaScript");
}
```

* Now let's observe a different example and try to understand the output.
```js
getName(); // Uncaught TypeError: getName is not a function

console.log(getName);

var getName = function () {
    console.log("Namaste JavaScript");
}

// The code won't execute as the first line itself throws an TypeError.
```

- **Vaibhav** know the reason for above, maybe <ins>the way function is defined</ins>.

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=Fnlnw8uY6jo&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/Fnlnw8uY6jo/0.jpg" width="750"
alt="Hoisting Youtube Link"/></a>

---

# 🚀 **Hoisting in JavaScript & TypeScript - Hinglish Guide for Beginners**  

JavaScript mein ek **unique behavior** hai jo execution ke time dekha jata hai, jise **Hoisting** kehte hain. Yeh ek mechanism hai jo allow karta hai ki **variables aur functions ko unke declare hone se pehle use kiya jaye!** 🧐

> **But How? 🤔**  
> Yeh **Execution Context ke "Memory Creation Phase"** ki wajah se hota hai.

---

## **🔷 JavaScript Execution Context & Hoisting**
Jab bhi JavaScript code execute hota hai, do phases hote hain:

1️⃣ **Memory Creation Phase:**  
   - Sabhi variables ko **undefined** assign kiya jata hai.  
   - Functions ka **poora code memory mein store hota hai**.  
2️⃣ **Code Execution Phase:**  
   - Line-by-line JavaScript code execute hota hai.  

💡 **Hoisting tabhi kaam karta hai jab Memory Creation Phase hota hai.**  

---

## **🔵 Example 1: Function Hoisting**
Pehle ek **function declaration** wala example dekhte hain:

```js
getName(); // ✅ Output: Namaste JavaScript

console.log(x); // ✅ Output: undefined
var x = 7;

function getName() {
    console.log("Namaste JavaScript");
}
```

### **🔸 JavaScript Kaise Execute Karega?**
- **Memory Creation Phase**:
  - `var x` ko `undefined` assign kiya jayega.
  - `getName` function ka **poora function memory mein store hoga**.

- **Code Execution Phase**:
  - `getName()` call hone par function execute ho jayega.
  - `console.log(x)` execute hoga aur `undefined` print karega.
  - `x = 7` assign ho jayega.

---

### **🔴 Example 2: Variable Hoisting (ReferenceError)**
```js
console.log(y); // ❌ ReferenceError: y is not defined
```
🔹 Agar **variable declare hi nahi hai**, to JavaScript error throw karega.  
🔹 Sirf declared **var variables ko undefined milta hai**, lekin agar **var likha hi nahi hai**, to **ReferenceError** aayega.  

---

## **🟡 Example 3: Function Expression Hoisting**
```js
getName(); // ❌ TypeError: getName is not a function

console.log(getName);

var getName = function () {
    console.log("Namaste JavaScript");
};
```

💡 **Error Kyun Aaya?**  
- Yeh **function declaration nahi, function expression hai**.  
- Function expression mein `var getName` ko **undefined** assign hota hai **pehle phase mein**.  
- Jab `getName()` call hota hai, tab yeh **undefined()** ban jata hai, jo **TypeError** deta hai.

---

## **🚀 TypeScript Mein Hoisting Ka Kya Scene Hai?**
TypeScript mein bhi **Hoisting hoti hai**, **lekin TypeScript runtime errors ko compile-time pe pakad leta hai!**  

---

## **🟢 Example 4: TypeScript Hoisting (Compile-Time Safety)**
```ts
console.log(x); // ❌ Error: Variable 'x' is used before being assigned.
let x = 10;
```
💡 **TypeScript pehle hi error de dega**, kyunki `let` aur `const` hoist hote hain **but they are in "Temporal Dead Zone" (TDZ) until assigned**.

🔹 **Difference between `var`, `let`, `const` hoisting in TypeScript:**
| Variable Type | Hoisted? | Default Value | Scope |
|--------------|----------|--------------|--------|
| `var`        | ✅ Yes   | `undefined`   | Function Scope |
| `let`        | ✅ Yes   | ❌ No (TDZ)   | Block Scope |
| `const`      | ✅ Yes   | ❌ No (TDZ)   | Block Scope |

🧐 **Temporal Dead Zone (TDZ)** means **variable accessible nahi hoga jab tak usko value assign na ho jaye!**

---

## **🔵 Example 5: TypeScript Function Hoisting**
```ts
getName(); // ✅ Output: Namaste TypeScript

function getName() {
    console.log("Namaste TypeScript");
}
```
TypeScript mein bhi function declaration **hoist hota hai** aur **poora function memory mein store hota hai**, bilkul JavaScript ki tarah.

---

## **🔴 Example 6: Function Expression in TypeScript**
```ts
console.log(getName); // ❌ Error: Cannot access 'getName' before initialization

const getName = () => {
    console.log("Namaste TypeScript");
};
```
💡 **TypeScript yeh error pehle hi detect kar lega**, kyunki **Arrow Functions** aur **Function Expressions** "Temporal Dead Zone (TDZ)" mein hote hain.

---

## **🔶 Summary: JavaScript vs TypeScript Hoisting**
| Behavior | JavaScript | TypeScript |
|----------|------------|-------------|
| **var hoisting** | `undefined` assign hota hai | Compile-time error agar `TDZ` mein ho |
| **let/const hoisting** | Hoist hota hai but error deta hai (TDZ) | Compile-time error (TDZ) |
| **Function Declaration hoisting** | Full function hoist hota hai | Full function hoist hota hai |
| **Function Expression hoisting** | `undefined` assign hota hai, `TypeError` deta hai | `TDZ` ki wajah se compile-time error |

---

## **📌 Conclusion**
1️⃣ **JavaScript aur TypeScript dono mein Hoisting hoti hai, par TypeScript compile-time safety provide karta hai.**  
2️⃣ **Function declarations hoist hote hain, function expressions nahi.**  
3️⃣ **var hoist hota hai, lekin let aur const "Temporal Dead Zone (TDZ)" mein hote hain.**  
4️⃣ **TypeScript runtime pe JavaScript jaise behave karta hai, par compile-time errors detect kar leta hai.**  

Agar yeh samajh aaya toh **Next Step: Closures & Scope Chain** padho! 🚀

---

## **📺 Watch Live on YouTube**
[![Hoisting in JavaScript](https://img.youtube.com/vi/Fnlnw8uY6jo/0.jpg)](https://www.youtube.com/watch?v=Fnlnw8uY6jo&ab_channel=AkshaySaini)