# Episode 6 : undefined vs not defined in JS

* In first phase (memory allocation) JS assigns `each variable` a placeholder called **undefined**.

* **undefined** is when `memory is allocated for the variable`, <i>but no value is assigned yet</i>.

* If an object/variable is `not even declared/found in memory allocation phase`, and tried to access it then it is **Not defined**

* Not Defined !== Undefined (`not equal to`)

> When variable is declared but not assigned value, its current value is **undefined**. But when the variable itself is not declared but called in code, then it is **not defined**. 

```js
console.log(x); // undefined
var x = 25;
console.log(x); // 25
console.log(a); // Uncaught ReferenceError: a is not defined
```

* JS is a **loosely typed / weakly typed** language. `It doesn't attach variables to any datatype`. We can say <ins>var a = 5</ins>, and then `change the value to` <ins>boolean *a = true*</ins> or <ins>string *a = 'hello'*</ins>  later on.

* **Never** `assign undefined to a variable manually`. Let it happen on it's own accord.
    - Know reason - **Why??**

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=B7iF6G3EyIk&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/B7iF6G3EyIk/0.jpg" width="750"
alt="undefined vs not defined in JS Youtube Link"/></a>

---

# 🚀 **Episode 6: `undefined` vs `not defined` in JavaScript (JS & TypeScript Differences)**

## **🔹 Introduction**
JavaScript execution hoti hai **2 phases** me:
1. **Memory Allocation Phase** – JavaScript har variable aur function ke liye **memory reserve** kar leta hai.
2. **Execution Phase** – Actual value assign hoti hai aur code execute hota hai.

---

## **🔹 `undefined` vs `not defined` – Core Difference**
| Concept | `undefined` | `not defined` |
|----------|------------|----------------|
| **Kab hota hai?** | Jab variable declare kiya gaya ho but **koi value assign nahi hui** | Jab variable memory me **exist hi nahi karta** |
| **Error deta hai?** | ❌ Nahi deta (default value hoti hai) | ✅ `ReferenceError` deta hai |
| **Hoisting hota hai?** | ✅ Haan, **memory allocated hoti hai, value undefined hoti hai** | ❌ Nahi, kyunki variable memory me nahi hota |
| **Example** | `var x; console.log(x); // undefined` | `console.log(y); // ReferenceError: y is not defined` |

---

## **🔹 `undefined` – Memory Allocated But No Value Assigned**
```js
console.log(x); // ✅ Output: undefined
var x = 25;
console.log(x); // ✅ Output: 25
```
✔️ **Explanation**:  
- `x` **hoist ho gaya** memory me, **par value nahi mili** to default `"undefined"` assign ho gaya.
- Jaise hi `x = 25;` assign hua, `undefined` **replace ho gaya** `25` se.

📌 **Never assign `undefined` manually!**
```js
var x = undefined; // ❌ Bad practice! JS automatically assign karega.
```

---

## **🔹 `not defined` – Variable Memory Me Hi Nahi Hai!**
```js
console.log(a); // ❌ ReferenceError: a is not defined
```
✔️ **Explanation**:  
- Variable `a` **memory me exist hi nahi karta**, isliye **ReferenceError** deta hai.

🚀 **Solution:** Pehle `a` declare karo, phir access karo:
```js
var a;
console.log(a); // ✅ Output: undefined
```

---

## **🔹 JavaScript is Loosely Typed**
JavaScript **loosely typed** language hai, iska matlab **variables kisi bhi type ka value le sakte hain**.

```js
var myVar = 10; // ✅ Number
myVar = "Hello"; // ✅ String
myVar = true; // ✅ Boolean
```
✔️ **JavaScript automatically type change kar sakta hai**.

🚀 **Bad Practice**: Jab unnecessary type change ho:
```js
var num = "10";
console.log(num + 5); // ❌ Output: "105" (String Concatenation)
```
✔️ **Solution**: Type convert karo:
```js
var num = Number("10");
console.log(num + 5); // ✅ Output: 15 (Addition)
```

---

## **🔹 TypeScript Variations**
| Feature | JavaScript | TypeScript |
|---------|-----------|------------|
| **Hoisting** | `var` hoist hota hai | `let` & `const` bhi hoist hote hain, par **TDZ** me rehte hain |
| **`undefined` Handling** | Default value hoti hai | `strictNullChecks` enable hone par warning de sakta hai |
| **`not defined` Error** | `ReferenceError` deta hai | Compile-time error deta hai |
| **Strict Typing** | Loosely typed | Strongly typed |

📌 **Example in TypeScript:**
```typescript
let x: number;
console.log(x); // ❌ Compile-time error: Variable 'x' is used before being assigned.
```
🚀 **Solution**: Default value assign karo:
```typescript
let x: number = 0; // ✅ No error
```

---

## **📝 Summary**
✔️ **`undefined` ka matlab hai variable declare ho gaya hai, but value assign nahi hui.**  
✔️ **`not defined` ka matlab hai variable memory me exist hi nahi karta.**  
✔️ **JavaScript loosely typed hai, but TypeScript strongly typed hai.**  
✔️ **TypeScript me hoisting ke time strict rules follow hote hain.**  

---

# 🎯 **Final Learning**
🚀 Ab aapko **JavaScript me `undefined` vs `not defined` ka clear concept** ho gaya hoga! 🎉  
💡 **Agar aap TypeScript use kar rahe ho, toh aapko extra type safety milti hai.**  
🔥 **Ab confidently coding karo aur errors ko avoid karo!** 😃

---