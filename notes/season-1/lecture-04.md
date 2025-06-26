# Episode 4 : Functions and Variable Environments

```js
var x = 1;

a();

b(); // we are calling the functions before defining them. This will work properly, as seen in Hoisting.

console.log(x); // Print order :- 3

function a() {
  var x = 10; // localscope because of separate execution context
  
  console.log(x); // Print order :- 1
}

function b() {
  var x = 100;
  console.log(x); // Print order :- 2
}
```

Outputs:

> 10

> 100

> 1

## Code Flow in terms of Execution Context

 The Global Execution Context (GEC) is created (the big box with Memory and Code subparts). Also GEC is pushed into Call Stack

> Call Stack : GEC

* In first phase of GEC (memory phase), variable `x:undefined` and `a and b have their entire function code` as **value initialized**

* In second phase of GEC (execution phase), when the function is called, a new local Execution Context is created.
  * After x = 1 assigned to GEC `x`, a() is called. So `local EC for a` is made <ins>inside code part of GEC</ins>.

> Call Stack: [GEC, a()]

* For local EC, a totally different x variable assigned undefined(x inside a()) in phase 1
  * And in phase 2 it is assigned 10 and printed in console log.
    * After printing, no more commands to run, so a() local EC is removed from `both GEC and from Call stack`.

> Call Stack: GEC

* Cursor goes back to b() function call. Same steps repeat.

> Call Stack :[GEC, b()] -> GEC (after printing yet another totally different x value as 100 in console log)

* Finally GEC is deleted and also removed from call stack. 
  * **`Program Ends`**


* reference:

![Execution Context Phase 1](/assets/function.jpg "Execution Context")

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=gSDncyuGw0s&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/gSDncyuGw0s/0.jpg" width="750"
alt="Functions and Variable Environments Youtube Link"/></a>

---

# 🚀 **Functions and Variable Environment in JavaScript & TypeScript - Hinglish Guide for Beginners**  

JavaScript mein **function execution aur variable environment** ek **important topic** hai jo **call stack aur execution context** ke saath milke kaam karta hai. Yeh samajhna zaroori hai kyunki yeh **scope, hoisting, aur closures** ke liye bhi base provide karta hai. 🧐  

> **But How? 🤔**  
> Yeh **Execution Context ke "Memory Creation Phase" aur "Code Execution Phase"** ki wajah se hota hai.

---

## **🔷 JavaScript Execution Context & Functions**
Jab bhi JavaScript code execute hota hai, do phases hote hain:

1️⃣ **Memory Creation Phase:**  
   - Sabhi variables ko **undefined** assign kiya jata hai.  
   - Functions ka **poora code memory mein store hota hai**.  
2️⃣ **Code Execution Phase:**  
   - Line-by-line JavaScript code execute hota hai.  

💡 **Function Execution ke time ek naya "Execution Context" create hota hai, jo Call Stack me add hota hai!**  

---

## **🔵 Example 1: Function Execution and Variable Environment**
```js
var x = 1;

a(); // ✅ Function call before declaration (Hoisting ke wajah se chalega)
b();

console.log(x); // ✅ Output: 1

function a() {
  var x = 10; // Function scope ke andar naya x create hoga
  console.log(x); // ✅ Output: 10
}

function b() {
  var x = 100; // Function scope ke andar naya x create hoga
  console.log(x); // ✅ Output: 100
}
```

### **🔸 JavaScript Kaise Execute Karega?**
- **Step 1:** **Global Execution Context (GEC) create hoga.**
- **Step 2:** **Memory Creation Phase**
  - `var x = undefined`
  - `function a` aur `function b` ka **poora function code memory mein store hota hai**.

- **Step 3:** **Code Execution Phase**
  - `x = 1` assign hoga.
  - `a()` function call hoga → **Naya Execution Context (EC) create hoga**.
  - `var x = 10` local scope mein assign hoga.
  - `console.log(x)` ka output **10** aayega.
  - `a()` execution khatam hone ke baad, **execution context delete hoga** aur control wapas `GEC` pe aayega.
  - `b()` function call hoga → **Naya Execution Context (EC) create hoga**.
  - `var x = 100` local scope mein assign hoga.
  - `console.log(x)` ka output **100** aayega.
  - `b()` execution khatam hone ke baad, **execution context delete hoga**.
  - `console.log(x)` ka output **1** aayega jo **Global x** hai.

---

### **📌 Call Stack Representation**
Jab `a()` aur `b()` call hote hain, tab **Call Stack** kuch aisa dikhega:

1️⃣ **Start:**
   ```
   Call Stack: [GEC]
   ```

2️⃣ **`a()` Call Hota Hai:**
   ```
   Call Stack: [GEC, a()]
   ```

3️⃣ **`a()` Execution Complete:**
   ```
   Call Stack: [GEC]
   ```

4️⃣ **`b()` Call Hota Hai:**
   ```
   Call Stack: [GEC, b()]
   ```

5️⃣ **`b()` Execution Complete:**
   ```
   Call Stack: [GEC]
   ```

6️⃣ **Program Ends (GEC bhi remove ho jata hai):**
   ```
   Call Stack: []
   ```

📌 **Final Output:**  
```
10
100
1
```

---

## **🔵 Example 2: Function Scope and Shadowing**
```js
var x = 5;

function test() {
  var x = 10;
  console.log(x); // ✅ Output: 10
}

test();

console.log(x); // ✅ Output: 5
```
💡 **Function ke andar wala `x` sirf function ke scope tak hi limited hai. Bahar wala `x` global scope ka `x` hai.**

---

## **🚀 TypeScript Mein Function Execution Ka Kya Scene Hai?**
TypeScript bhi **function execution aur variable environment** ko **JavaScript jaise handle karta hai**, par yeh **better type safety** deta hai!  

---

## **🟢 Example 3: TypeScript Variable Environment**
```ts
let x = 1;

function a() {
  let x = 10; // ✅ Local Scope
  console.log(x);
}

function b() {
  let x = 100;
  console.log(x);
}

a(); // ✅ Output: 10
b(); // ✅ Output: 100

console.log(x); // ✅ Output: 1
```
🔹 **Difference Between JavaScript and TypeScript**
| Feature | JavaScript | TypeScript |
|---------|-----------|------------|
| **Hoisting** | `var` hoist hota hai (undefined assign hota hai) | `let` aur `const` hoist hote hain but "Temporal Dead Zone (TDZ)" mein hote hain |
| **Block Scope** | `var` global scope maintain karta hai | `let` aur `const` block scope maintain karte hain |
| **Function Execution** | Alag execution context banta hai | Same execution context, par strict typing |

---

## **🔴 Example 4: TypeScript Function Parameter Type Safety**
```ts
function addNumbers(a: number, b: number): number {
  return a + b;
}

console.log(addNumbers(2, 3)); // ✅ Output: 5
console.log(addNumbers("2", 3)); // ❌ Error: Argument of type 'string' is not assignable to parameter of type 'number'.
```
💡 **TypeScript Type Safety Ensure Karta Hai!**  
- JavaScript mein agar `"2" + 3` likhen to `23` output aayega (concatenation).  
- **TypeScript yeh error compile time pe pakad lega!**  

---

## **🔵 Example 5: TypeScript Function with Default Parameters**
```ts
function greet(name: string = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet(); // ✅ Output: Hello, Guest!
greet("Vaibhav"); // ✅ Output: Hello, Vaibhav!
```
💡 **TypeScript Default Parameters Support Karta Hai** jo **JavaScript ES6** ke baad aaya.

---

## **🔶 Summary: JavaScript vs TypeScript Variable Environment**
| Behavior | JavaScript | TypeScript |
|----------|------------|-------------|
| **var hoisting** | `undefined` assign hota hai | Compile-time error agar `TDZ` mein ho |
| **let/const hoisting** | Hoist hota hai but error deta hai (TDZ) | Compile-time error (TDZ) |
| **Function Execution Context** | Alag execution context banta hai | Alag execution context banta hai par type safety ke saath |
| **Type Safety** | No type safety | Strict type safety |

---

## **📌 Conclusion**
1️⃣ **JavaScript aur TypeScript dono mein functions execution similar hai, but TypeScript runtime pe type safety ensure karta hai.**  
2️⃣ **Function ke andar variables sirf uske scope tak hi limited hote hain (function scope).**  
3️⃣ **JavaScript mein `var` global scope maintain karta hai, jabki TypeScript `let` aur `const` ko block scope deta hai.**  
4️⃣ **TypeScript Type Safety Ensure Karta Hai jo runtime errors ko compile-time pe fix karne mein madad karta hai.**  

Agar yeh samajh aaya toh **Next Step: Closures & Scope Chain** padho! 🚀

## **📺 Watch Live on YouTube**
[![Functions and Variable Environments](https://img.youtube.com/vi/gSDncyuGw0s/0.jpg)](https://www.youtube.com/watch?v=gSDncyuGw0s&ab_channel=AkshaySaini)

---

## **🔍 Hoisting Kya Hota Hai? Aur TDZ Mein `let` aur `const` Kyun Error Dete Hain?**  

💡 **Hoisting ek JavaScript ka concept hai jisme variables aur functions ko execution ke pehle memory mein allocate kar diya jata hai.** Matlab, chahe aap variables ya functions code ke kisi bhi line pe likho, unka memory allocation pehle ho jata hai.  

---
### **📌 Hoisting Ka Matlab Kya Hota Hai?**
✔️ **JavaScript ke execution context ke `memory creation phase` mein,** sabhi **variables aur functions ka declaration** memory mein store ho jata hai, pehle se hi.  

👀 **Ek Example Dekho:**  
```js
console.log(myVar); // Output: undefined (kyunki hoisting hui)
var myVar = 10;
console.log(myVar); // Output: 10
```
💡 **Yahaan `myVar` hoist ho chuka hai aur `undefined` assign ho gaya hai.**

---
### **🛑 `let` aur `const` TDZ Mein Kyun Hain?**
🚀 **TDZ ka matlab hai Temporal Dead Zone**, jo ek aisa time period hai jab `let` aur `const` variables **hoist toh ho jaate hain, but access nahi kiye ja sakte jab tak initialize na ho jaye!**  

👀 **Example Samjho:**  
```js
console.log(myLet); // ❌ ReferenceError: Cannot access 'myLet' before initialization
let myLet = 10;
console.log(myLet); // ✅ Output: 10
```
---
### **🔵 `var` vs `let` vs `const` Hoisting Behaviour**
| Feature | `var` | `let` | `const` |
|---------|-----------|-----------|-----------|
| **Hoisting** | Hoist hota hai ✅ | Hoist hota hai ✅ | Hoist hota hai ✅ |
| **Default Value** | `undefined` assign hota hai | TDZ mein rehta hai ❌ | TDZ mein rehta hai ❌ |
| **Access Before Initialization** | Allowed, output `undefined` ✅ | ❌ ReferenceError | ❌ ReferenceError |
| **Scope** | Function Scope | Block Scope | Block Scope |


### **🤔 TDZ Kyun Hota Hai?**
👉 **TDZ ka reason yeh hai ki `let` aur `const` ko block-scope ke sath design kiya gaya hai.**  
👉 JavaScript chahta hai ki `let` aur `const` ka istemal sirf tabhi ho jab wo **explicitly initialize ho jayein.**  
👉 `var` se errors avoid karne ke liye, TDZ introduce kiya gaya taaki developer mistakenly `undefined` values ko na use kare.

---
### **🔵 TDZ ka Execution Flow Samjho**
```js
console.log(x); // ❌ ReferenceError: Cannot access 'x' before initialization
let x = 5;
console.log(x); // ✅ Output: 5
```
📌 **Memory Phase (Before Execution)**
```
Memory:
    var x -> undefined ✅
    let x -> TDZ ❌
```
📌 **Execution Phase (After Initialization)**
```
Memory:
    var x -> "Hello World!" ✅
    let x -> 5 ✅
```

---
## **🔍 TypeScript Mein TDZ Strict Ban Jata Hai**
👉 **JavaScript mein runtime pe error aata hai, lekin TypeScript isko compile-time pe pakad leta hai!**  
```ts
console.log(myName); // ❌ Compile-Time Error: Block-scoped variable 'myName' used before its declaration.
let myName = "Vaibhav";
console.log(myName);
```

---
## **📌 Conclusion**
✔️ **Hoisting ka matlab hai variable/function ka declaration memory mein store ho jaana execution se pehle.**  
✔️ **`var` ko `undefined` assign hota hai, but `let` aur `const` TDZ mein rehte hain jab tak initialize na ho.**  
✔️ **TDZ ka reason safe variable usage aur accidental `undefined` errors ko avoid karna hai.**  
✔️ **TypeScript TDZ errors ko compile-time pe detect kar leta hai.**  

🚀 **Ab TDZ clear ho gaya, toh next concepts: Closures aur Scope Chain pe move karein?** 😃

---
## **🔍 Block Scope vs Function Scope in JavaScript**
JavaScript mein variables ka **scope** define karta hai ki ek variable **kahaan tak accessible hai** aur kahaan nahi. **Scope ka do major type hote hain:**
1. **Function Scope (var)**
2. **Block Scope (let, const)**

---

## **📌 1. Function Scope (var)**
💡 **Jo bhi variable `var` keyword se declare hota hai, wo sirf us function ke andar accessible hota hai jisme declare kiya gaya hai.**

📌 **Example:**
```js
function myFunction() {
    var myVar = "Hello";
    console.log(myVar); // ✅ Output: Hello
}
console.log(myVar); // ❌ ReferenceError: myVar is not defined
```
🔹 **Yahaan `myVar` sirf function ke andar accessible hai, uske bahar access karne par error aayega.**  
🔹 **Iska scope pura function hota hai, chahe kisi bhi block `{}` ke andar ho.**

---

### **🚨 `var` ka problem - Hoisting + Scope Issue**
```js
if (true) {
    var a = 10;
}
console.log(a); // ✅ Output: 10 (Function scope hone ki wajah se accessible hai)
```
**⚠️ Problem:**  
🔹 `var` **block ke bahar bhi accessible hota hai**, jo kabhi kabhi unexpected behavior create kar sakta hai.

---

## **📌 2. Block Scope (`let` and `const`)**
💡 **`let` aur `const` block-scope wale hote hain, iska matlab wo sirf us `{}` ke andar accessible honge jisme declare kiya gaya hai.**

📌 **Example:**
```js
if (true) {
    let x = 5;
    console.log(x); // ✅ Output: 5
}
console.log(x); // ❌ ReferenceError: x is not defined
```
**🔹 Yahaan `x` ka scope sirf `{}` ke andar hai. Uske bahar access karne par error aayega.**  
**🔹 `let` aur `const` ko TDZ (Temporal Dead Zone) ka support milta hai, isliye hoisting ke baad bhi error aata hai.**  

---

### **🚀 Block Scope ka Advantage**
```js
function test() {
    var a = 1; // Function Scoped
    let b = 2; // Block Scoped
    const c = 3; // Block Scoped

    if (true) {
        var a = 100; // Same function ke andar reassign ho gaya!
        let b = 200; // Alag block scope ka b variable
        const c = 300; // Alag block scope ka c variable
        console.log(a, b, c); // ✅ Output: 100, 200, 300
    }
    console.log(a, b, c); // ✅ Output: 100, 2, 3
}
test();
```
**📌 Explanation:**  
✔️ `var a` **function scope** ka hai, toh andar se modify hone ke baad **bahar bhi update ho gaya**.  
✔️ `let b` aur `const c` **block scope** ke hai, toh **andar wale block ka alag variable** create ho gaya.  

---

## **🔵 Function Scope vs Block Scope - Quick Comparison**
| Feature | Function Scope (`var`) | Block Scope (`let` & `const`) |
|---------|----------------|----------------|
| **Scope** | Function ke andar hota hai ✅ | `{}` block ke andar hota hai ✅ |
| **Hoisting** | Hoist hota hai, `undefined` assign hota hai ✅ | Hoist hota hai but TDZ mein rehta hai ❌ |
| **Re-declaration** | Allowed ✅ | Not Allowed ❌ |
| **Can be accessed outside block?** | Yes ✅ | No ❌ |
| **Preferred for modern JS?** | No ❌ | Yes ✅ |

---

## **📌 Conclusion**
✔️ **Function Scope** (`var`) puri function body tak accessible hota hai, **chahe kisi block `{}` ke andar ho ya na ho.**  
✔️ **Block Scope** (`let` aur `const`) sirf us `{}` ke andar accessible hote hain.  
✔️ **Modern JavaScript mein `var` avoid karna chahiye, aur `let` aur `const` ka use karna chahiye.**  

🚀 **Ab aapko `Scope` ka concept clear ho gaya! Aage `Closures` aur `Lexical Scope` explore karein?** 😃