# Episode 7 : The Scope Chain, Scope & Lexical Environment

* **Scope** in Javascript is directly related to **Lexical Environment**.

* Let's observe the below examples:
```js
// CASE 1
function a() {

    console.log(b); // 10
    // Instead of printing undefined it prints 10, So somehow this a function could access the variable b outside the function scope.

}

var b = 10;

a();
```
- In **case 1**: function a is able to access variable b from Global scope.

```js
// CASE 2
function a() {

    c();

    function c() {
        console.log(b); // 10
    }
}

var b = 10;

a();
```
- In **case 2**: 10 is printed. It means that within nested function too, the global scope variable can be accessed.

```js
// CASE 3
function a() {

    c();

    function c() {
        var b = 100;
        console.log(b); // 100
    }
}

var b = 10;
a();
```
- In **case 3**: 100 is printed meaning local variable of the same name took precedence over a global variable.

```js
// CASE 4
function a() {

    var b = 10;
    c();

    function c() {
        console.log(b); // 10
    }
}

a();

console.log(b); // Error, Not Defined
```
- In **case 4**: A function can access a global variable, but the global execution context can't access any local variable.

```
To summarize the above points in terms of execution context:

- call_stack = [GEC, a(), c()]

- Now lets also assign the memory sections of each execution context in call_stack.

c() = [[`lexical environment pointer` pointing to a()]]

a() = [b:10, c:{}, [lexical environment pointer pointing to GEC]]

GEC =  [a:{},[lexical_environment pointer pointing to null]]
``` 

![Lexical Scope Explaination](/assets/lexical.jpg "Lexical Scope")

![Lexical Scope Explaination](/assets/lexical2.jpg "Lexical Scope")

<br>

**Question - What is Lexical Enviroment?**
* **Ans -** For a moment think of it as a local memory.
- Actually **Lexical Environment**   = local memory + lexical env of its parent.  Hence, Lexical Enviroment is the local memory along with the lexical environment of its parent

* **Lexical**: In hierarchy, sequence or order. So in CASE 4, c() function is lexicially/physically  (sitting) inside a() function - in order/hierarchy .

* Whenever an Execution Context is created, a Lexical environment(LE) is also created and is referenced in the local Execution Context(in memory space).

* The process of going one by one to parent and checking for values (until it does not find the variable )is called scope chain or Lexcial environment chain.
    - If scope chain is exhausted we still not find the variable we call it that mean  is not in scope  

* ```js
  function a() {
      function c() {
          // logic here
      }
      c(); // c is lexically inside a
  } // a is lexically inside global execution
  ```

* Lexical or Static scope refers to the accessibility of variables, functions and object based on physical location in source code.
    ```js
    Global {
        Outer {
            Inner
        }
    }
    // Inner is surrounded by lexical scope of Outer
    ```


* **TLDR**; An inner function can access variables which are in outer functions even if inner function is nested deep. In any other case, a function can't access variables not in its scope.


<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=uH-tVP8MUs8&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/uH-tVP8MUs8/0.jpg" width="750"
alt="The Scope Chain, Scope & Lexical Environment Youtube Link"/></a>

---

# 🚀 **Episode 7: Scope, Scope Chain & Lexical Environment in JavaScript (With TypeScript Differences)**

## **🔹 Introduction**
Scope **define karta hai ki ek variable kahaan tak accessible hai**. JavaScript ka execution **Lexical Environment** ke basis pe hota hai.

---

## **🔹 Scope in JavaScript**
| Scope Type | Explanation |
|------------|-------------|
| **Global Scope** | Jo variable ya function **pure program me accessible** ho. |
| **Function Scope** | Jo sirf ek function ke andar accessible ho. |
| **Block Scope** | `{}` curly braces ke andar jo variables declare hote hain (ES6 ke `let` aur `const`). |
| **Lexical Scope** | Ek function apne parent function ke variables access kar sakta hai. |

---

## **🔹 Example 1: Global Scope**
```js
var a = 10; // ✅ Global scope variable

function printA() {
    console.log(a); // ✅ Accessible
}

printA(); // Output: 10
console.log(a); // Output: 10
```
✔️ **Explanation**:  
- `a` global scope me declare hai, isliye **function ke andar bhi accessible** hai.

---

## **🔹 Example 2: Function Scope**
```js
function myFunc() {
    var b = 20; // ✅ Function scope
    console.log(b); // ✅ Accessible
}

myFunc();
console.log(b); // ❌ ReferenceError: b is not defined
```
✔️ **Explanation**:  
- `b` function ke andar declare hai, **toh function ke bahar accessible nahi hoga**.

---

## **🔹 Example 3: Block Scope (let & const)**
```js
if (true) {
    let c = 30; // ✅ Block scope
    console.log(c); // ✅ Accessible
}

console.log(c); // ❌ ReferenceError: c is not defined
```
✔️ **Explanation**:  
- `let` aur `const` **block scope maintain karte hain**, toh curly braces `{}` ke bahar access nahi hoga.

---

## **🔹 Example 4: Lexical Scope (Nested Function Access)**
```js
function outerFunction() {
    let d = 40; // ✅ Outer function scope

    function innerFunction() {
        console.log(d); // ✅ Accessible (Lexical Scope)
    }

    innerFunction();
}

outerFunction(); // Output: 40
console.log(d); // ❌ ReferenceError: d is not defined
```
✔️ **Explanation**:  
- Inner function `innerFunction()` **parent function ke variables access kar sakta hai**.
- **But parent function ke variables global scope me available nahi honge**.

---

## **🔹 Lexical Environment & Scope Chain**
JavaScript **Lexical Environment** ka use karta hai **variables ko access karne ke liye**.  

📌 **Lexical Environment kya hai?**  
👉 **Har function ka ek memory hota hai jisme uske local variables aur parent ka reference store hota hai**.

📌 **Scope Chain kya hai?**  
👉 **Agar ek function me koi variable nahi milta, toh JS uske parent ke Lexical Environment me dhundhta hai**.

### **📍 Example: Scope Chain**
```js
var globalVar = "I am global";

function first() {
    var firstVar = "I am inside first";

    function second() {
        var secondVar = "I am inside second";
        console.log(globalVar); // ✅ Accessible
        console.log(firstVar); // ✅ Accessible
        console.log(secondVar); // ✅ Accessible
    }

    second();
}

first();
console.log(secondVar); // ❌ ReferenceError: secondVar is not defined
```
✔️ **Explanation**:  
- `second()` function **global variable aur parent function `first()` ke variables access kar sakta hai**.
- **But `secondVar` global scope me nahi milega**.

---

## **🔹 Execution Context & Call Stack**
Jab bhi **function execute hota hai, ek naya execution context create hota hai** jo Call Stack me push hota hai.

### **📍 Execution Order**
1️⃣ **Global Execution Context (GEC)** create hota hai.  
2️⃣ `first()` function call hota hai, toh **naya execution context** banta hai.  
3️⃣ `second()` function call hota hai, **uska execution context bhi create hota hai**.  
4️⃣ Jab `second()` function complete ho jata hai, **uska execution context remove hota hai**.  
5️⃣ Jab `first()` complete hota hai, **uska execution context bhi remove hota hai**.  

📌 **Call Stack Representation**
```
Call Stack:
1. GEC (Global Execution Context)
2. first() Execution Context
3. second() Execution Context
```

---

## **🔹 TypeScript Variations**
| Feature | JavaScript | TypeScript |
|---------|-----------|------------|
| **Scope Handling** | Function & Global Scope | Function, Block & Global Scope |
| **Strict Typing** | Loosely Typed | Strongly Typed |
| **Lexical Environment** | Parent function se variables access | Same concept, but type safety ensure karta hai |
| **Error Handling** | `undefined` aur `ReferenceError` | Compile-time error agar scope violate hota hai |

📌 **Example in TypeScript**
```typescript
function outer() {
    let message: string = "Hello from outer function";

    function inner() {
        console.log(message); // ✅ Accessible
    }

    inner();
}

outer();
console.log(message); // ❌ Compile-time error: 'message' is not defined
```
✔️ **TypeScript me agar variable accessible nahi hai, toh Compile-time error aayega.**

---

## **📝 Summary**
✔️ **Scope decide karta hai ki ek variable kahaan tak accessible hai.**  
✔️ **Global Scope me declare variables har jagah accessible hote hain.**  
✔️ **Function Scope ke variables sirf function ke andar access hote hain.**  
✔️ **Block Scope (let, const) sirf `{}` ke andar hi accessible hote hain.**  
✔️ **Lexical Scope ka matlab hota hai ki ek function apne parent function ke variables access kar sakta hai.**  
✔️ **Scope Chain ka matlab hai ki agar variable nahi mila, toh JavaScript uske parent environment me dhundhega.**  

---

# 🎯 **Final Learning**
🚀 **Ab aapko JS ka Scope aur Lexical Environment ka concept clear ho gaya!** 🎉  
💡 **Agar aap TypeScript use kar rahe ho, toh aapko extra type safety milti hai.**  
🔥 **Ab confidently coding karo aur errors ko avoid karo!** 😃