# Episode 5 : Shortest JS Program, window & this keyword

* The shortest JS program is `empty file`. 
    * Because even then, JS engine does a lot of things. As always, <ins>even in this case, it creates the GEC</ins> which has memory space and the execution context.

* JS engine creates something known as `window`. 
    * It is an object, which is created in the global space.
        * It contains lots of functions and variables.
        * These functions and variables can be accessed from anywhere in the program.
* JS engine also creates a `this` keyword, which points to the `window object` at the <ins>global level</ins>. 

* In **summary**
    * <ins>`Along with GEC, a global object (window) and a this variable are created`</ins>.

* In different engines, the name of global object changes. `Window in browsers`, but <ins>in nodeJS it is called something else. At global level, this === window</ins>

* If we create any <ins><i>variable in the global scope</i></ins>, then the <ins><i>variables get attached to the global object</i></ins>.

eg:

```js
var x = 10;
console.log(x); // 10
console.log(this.x); // 10
console.log(window.x); // 10
```

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=QCRpVw2KXf8&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/QCRpVw2KXf8/0.jpg" width="750"
alt="Shortest JS Program, window & this keyword Youtube Link"/></a>

---

## **📌 Shortest JavaScript Program, `window` Object & `this` Keyword**

### **🔹 Shortest JavaScript Program**
- Agar aap **khaali file bhi run karein**, toh bhi JavaScript **kuch kaam karti hai**!
- Jaise hi JS file execute hoti hai, ek **Global Execution Context (GEC)** create hota hai.
- Global Execution Context **do cheezein create karta hai**:
  1. **`window` Object** - Browser environment mein global object hota hai.
  2. **`this` Keyword** - Global scope mein `this` window object ko refer karta hai.

---

## **🔹 `window` Object in JavaScript**
- JavaScript **browsers ke andar** run karta hai, aur browser **ek global object provide karta hai** jise hum `window` object kehte hain.
- `window` object **global scope ke saare variables aur functions ko store karta hai**.

📌 **Example:**
```js
console.log(window); // ✅ Output: Window object with many properties
```

📌 **Variables `window` object ka part hote hain**:
```js
var myVar = "Hello JS!";
console.log(window.myVar); // ✅ Output: Hello JS!
```
🔹 `window.myVar` ka matlab hai ki global variable **window object ka ek property hai**.

---

## **🔹 `this` Keyword in Global Scope**
- **Global scope mein**, `this` keyword **`window` object ko point karta hai**.

📌 **Example:**
```js
console.log(this === window); // ✅ Output: true
```
🔹 Yaani **global scope mein `this` ka reference `window` object hota hai**.

📌 **Function ke andar `this` ka behavior**:
```js
function showThis() {
    console.log(this);
}
showThis(); // ✅ Output: window object (in non-strict mode)
```
🔹 **Normal function mein `this` global object ko refer karega** (window).

📌 **Strict mode mein `this` undefined ho jata hai**:
```js
"use strict";
function showThis() {
    console.log(this);
}
showThis(); // ❌ Output: undefined
```
🔹 **Strict mode mein function ke andar `this` undefined ho jata hai**.

---

## **🔹 Global Object in Different Environments**
💡 **JavaScript har environment mein ek different global object use karta hai**:

| Environment | Global Object |
|-------------|--------------|
| **Browser** | `window` |
| **Node.js** | `global` |
| **Web Workers** | `self` |

📌 **Node.js mein `this` ka behavior alag hota hai**:
```js
console.log(global); // ✅ Output: Global object in Node.js
console.log(this === global); // ❌ Output: false (Node.js mein `this` alag hota hai)
```

---

## **💡 `var` vs `let` & `const` in Global Scope**
| Feature | `var` | `let` & `const` |
|---------|------|--------------|
| Hoisting | Hoist hota hai (`undefined` assign hota hai) | Hoist hota hai, par **TDZ (Temporal Dead Zone) mein rehta hai** |
| Global `window` object ka part | Haan ✅ | Nahi ❌ |
| Redeclaration | Allowed ✅ | Allowed ❌ |
| Scope | Function Scope | Block Scope |

📌 **Example**:
```js
var a = 10;
let b = 20;
const c = 30;

console.log(window.a); // ✅ Output: 10
console.log(window.b); // ❌ Output: undefined
console.log(window.c); // ❌ Output: undefined
```
🔹 **`let` aur `const` variables global object ka part nahi bante**, sirf **`var` banta hai**.

---

## **🔹 Temporal Dead Zone (TDZ) - Full Explanation**
💡 **Temporal Dead Zone (TDZ) ek time period hota hai jisme `let` aur `const` variables hoist toh hote hain, par unko access karna error deta hai.**

📌 **Example:**
```js
console.log(myVar); // ✅ Output: undefined
var myVar = 10;

console.log(myLet); // ❌ ReferenceError: Cannot access 'myLet' before initialization
let myLet = 20;
```
🚀 **TDZ Concept**:
1. **Memory phase mein** `let` aur `const` hoist toh hote hain, **but unko access nahi kar sakte**.
2. **Jab tak unko initialize nahi karte, tab tak unka access error dega**.
3. **Ye `var` ke behavior se alag hai, jo `undefined` assign karta hai.**

📌 **TDZ Visualization:**
```js
console.log(x); // ❌ ReferenceError (TDZ)
let x = 10;
console.log(x); // ✅ Output: 10
```
🔹 **Yahan `x` hoist toh ho gaya hai, but jab tak usko initialize nahi karenge tab tak access nahi kar sakte.**

---

## **📝 Summary**
✔️ JavaScript **execution ke time Global Execution Context (GEC) create karta hai**.  
✔️ **Browser mein `window` object hota hai**, jo **global object ka kaam karta hai**.  
✔️ **Global scope mein `this` ka reference `window` object hota hai**.  
✔️ **`var` hoist hota hai aur `undefined` assign hota hai**, lekin **`let` aur `const` sirf hoist hote hain par TDZ (Temporal Dead Zone) mein rehte hain**.  
✔️ **TDZ ek time period hota hai jisme `let` aur `const` variables access nahi kiye ja sakte** jab tak wo initialize nahi hote.  

🚀 **Ab aap `window`, `this` aur `TDZ` ko confidently use kar sakte ho!** 😃