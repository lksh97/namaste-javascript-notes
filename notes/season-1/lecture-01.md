# Episode 1 : Execution Context

- Everything in JS happens inside the `execution context`. 
- Imagine a <ins>sealed-off container inside which JS runs</ins>.
  -  It is an abstract concept that `hold information about the enviroment`, within the current code is being executed.

  ![Execution Context](/assets/execution-context.jpg "Execution Context")

- In the container the first component is `memory component` and the 2nd one is `code component`

- Memory component has all the variables and functions in <ins>key value pairs</ins>. It is also called `Variable environment`.

- Code component is the place where code is executed **one line at a time**. It is also called the `Thread of Execution`.

- JS is a `synchronous`, `single-threaded` language
  - Synchronous :- In a specific synchronous order.
    - Will move to next line ones current line is done.
  - Single-threaded :- One command at a time.

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=ZvbzSrg0afE&list=PLlasXeu85E9cQ32gLCvAvr9vNaUccPVNP" target="_blank"><img src="https://img.youtube.com/vi/ZvbzSrg0afE/0.jpg" width="750"
alt="Execution Context Youtube Link"/></a>

----

# 🚀 **Episode 1: Execution Context (Hinglish Mein Samjho)**
  
JavaScript mein **har ek code execution ek execution context ke andar hoti hai**. Samajhne ke liye aise socho jaise ek **sealed-off container** jisme **JS ka pura code run hota hai**.

---

## 🔍 **Execution Context Kya Hota Hai?**
JavaScript ka ek **abstract concept** hai jo yeh decide karta hai ki **kaunsa code execute ho raha hai aur kis environment mein**.

### **📝 Text Diagram (Execution Context ka Structure)**

           +---------------------------------------+
           |       Execution Context (EC)         |
           +---------------------------------------+
           |       Memory Component (Heap)        |  <-- Stores Variables & Functions
           +---------------------------------------+
           |       Code Component (Call Stack)    |  <-- Executes Code Line-by-Line
           +---------------------------------------+

---

## 🔥 **Execution Context Ke 2 Main Parts**
### 1️⃣ **Memory Component (Variable Environment)**
   - **Yeh ek storage area hota hai jisme sare variables aur functions store hote hain** **(key-value pairs format)**.
   - Jab bhi koi function ya variable declare hota hai, wo yaha memory allocate karta hai.
   - Example:
     ```js
     var name = "Vaibhav";  // Memory mein 'name' variable create hoga
     function greet() { 
         console.log("Hello");
     }  // 'greet' function ka reference memory component mein store hoga
     ```

### 2️⃣ **Code Component (Thread of Execution)**
   - Yeh **JS ka execution area hota hai**, jisme code **one-line-at-a-time** execute hota hai.
   - Isko **Call Stack** bhi bolte hain kyunki function calls stack format mein process hote hain.
   - Example:
     ```js
     function one() {
         console.log("Function One");
     }
     function two() {
         one();
         console.log("Function Two");
     }
     two();
     ```
     **Execution Flow (Call Stack)**
     ```
     1️⃣ two()  <-- Call Stack pe push hota hai
     2️⃣ one()  <-- one() call hota hai, stack pe push hota hai
     3️⃣ "Function One" print hota hai, then one() stack se pop hota hai
     4️⃣ "Function Two" print hota hai, then two() stack se pop hota hai
     ```
   - **Call Stack empty ho jata hai jab saara code execute ho jata hai**.

---

## 🚀 **JavaScript Ki Execution Kaise Hoti Hai?**
### ⚡ JavaScript Ek **Synchronous, Single-Threaded** Language Hai
1. **Synchronous Execution**  
   - JS ek **line-by-line execute hoti hai**, jab tak ek line execute nahi hoti, next line execute nahi hogi.
   - Example:
     ```js
     console.log("Hello");
     console.log("World");
     ```
     Output:
     ```
     Hello
     World
     ```

2. **Single-Threaded**  
   - JS **ek time pe ek hi kaam karti hai**.
   - Ek hi execution stack hota hai jo sequentially instructions execute karta hai.

---

## 📌 **JavaScript Execution Context Ke Stages**
1️⃣ **Creation Phase (Memory Allocation)**
   - **Sabhi variables ko "undefined" assign kiya jata hai aur functions ka reference store hota hai**.
   - Example:
     ```js
     console.log(a);  // undefined
     var a = 10;
     console.log(a);  // 10
     ```
   - Execution se pehle `a` ko memory mein allocate kiya gaya lekin iska value `undefined` rakha gaya.

2️⃣ **Execution Phase (Code Execution)**
   - Ab **actual code execute hota hai** aur variables apna assigned value le lete hain.

---

## 🎯 **Example: Execution Context Ka Step-by-Step Breakdown**
```js
console.log(x);
var x = 10;
function sayHello() {
    console.log("Hello");
}
sayHello();
```

Execution Context Breakdown

Step	Action
🔹 1	Memory Creation Phase: x = undefined, sayHello ka function reference store hota hai
🔹 2	Execution Phase: console.log(x); –> undefined print hota hai
🔹 3	x = 10; assign hota hai
🔹 4	sayHello() call hota hai, Call Stack pe push hota hai
🔹 5	console.log("Hello"); execute hota hai, “Hello” print hota hai
🔹 6	sayHello() stack se pop hota hai, execution context destroy hota hai

🎥 Watch Live on YouTube Below:

💡 Conclusion
	•	Execution Context ek container hai jisme JavaScript ka code run hota hai.
	•	Isme Memory Component (Heap) aur Code Component (Call Stack) hota hai.
	•	JavaScript synchronous aur single-threaded hai, yani ek time pe ek hi task execute hota hai.
	•	Har function call ke liye naya execution context create hota hai jo Call Stack me push aur pop hota hai.

Agar aapko yeh samajh aaya toh agla step Hoisting aur Scope Chain ko samajhna hoga! 🚀

---

🚀 TypeScript Mein Execution Context (Hinglish Mein Samjho)

JavaScript ki tarah hi TypeScript ka code bhi Execution Context ke andar hi run hota hai. Bas ek major difference yeh hai ki TypeScript static typing support karta hai, jo execution phase ke pehle errors detect kar leta hai.

🔍 Execution Context in TypeScript

JavaScript ki tarah hi, TypeScript ka bhi ek execution environment hota hai, jo Memory Component aur Code Component ka combination hota hai.

📝 Text Diagram (Execution Context in TypeScript)

               +---------------------------------------+
               |       TypeScript Execution Context   |
               +---------------------------------------+
               |       Memory Component (Heap)        |  <-- Stores Variables & Functions
               +---------------------------------------+
               |       Code Component (Call Stack)    |  <-- Executes Code Line-by-Line
               +---------------------------------------+

TypeScript me Execution Context ka structure JavaScript jaisa hi hota hai, bus ek extra feature hota hai: “Type Safety”.

🔥 Execution Context Ke 2 Main Parts (TypeScript Mein)

1️⃣ Memory Component (Variable Environment)
	•	Yeh ek storage area hota hai jisme sare variables aur functions store hote hain (key-value pairs format).
	•	TypeScript extra validation karta hai, taaki koi invalid value assign na ho.
	•	Example:

let name: string = "Vaibhav";  // Memory mein 'name' variable create hoga
function greet(): void { 
    console.log("Hello");
}  // 'greet' function ka reference memory component mein store hoga

	•	Yahaan name variable ko TypeScript check karega ki string ke alawa koi aur value na aaye.

2️⃣ Code Component (Thread of Execution)
	•	Yeh JS ka execution area hota hai, jisme code one-line-at-a-time execute hota hai.
	•	TypeScript mein Compilation Phase hota hai jo JavaScript mein nahi hota!
	•	Compilation Phase ka fayda:
	•	Errors execution ke pehle hi detect ho jate hain!
	•	Example:

function one(): void {
    console.log("Function One");
}
function two(): void {
    one();
    console.log("Function Two");
}
two();

Execution Flow (Call Stack)

1️⃣ two()  <-- Call Stack pe push hota hai
2️⃣ one()  <-- one() call hota hai, stack pe push hota hai
3️⃣ "Function One" print hota hai, then one() stack se pop hota hai
4️⃣ "Function Two" print hota hai, then two() stack se pop hota hai


	•	Call Stack empty ho jata hai jab saara code execute ho jata hai.

🚀 TypeScript Ki Execution Kaise Hoti Hai?

⚡ TypeScript JavaScript Se Kaise Different Hai?

Feature	JavaScript	TypeScript
Execution Context	JavaScript Engine ke andar execute hota hai	Same as JavaScript
Synchronous Execution	Code ek line-by-line execute hota hai	Same as JavaScript
Single-Threaded	Ek time pe ek hi task execute hota hai	Same as JavaScript
Compilation Phase	Directly run hota hai (Just-in-time execution)	Pehle TypeScript compiler (tsc) code check karta hai
Type Checking	Nahi hota (Dynamically Typed)	Hota hai (Statically Typed)

Example

let age: number = 25;
age = "Twenty-Five";  // ❌ Error: Type 'string' is not assignable to type 'number'

Isme TypeScript Compilation Phase mein hi error de dega, jabki JavaScript mein runtime error aata.

📌 TypeScript Execution Context Ke Stages

1️⃣ Creation Phase (Memory Allocation)
	•	Sabhi variables ko “undefined” assign kiya jata hai aur functions ka reference store hota hai.
	•	TypeScript Extra Step: Static Type Checking
	•	Example:

console.log(a);  // Error: 'a' is used before being assigned
let a: number = 10;
console.log(a);  // 10


	•	TypeScript runtime error ke pehle hi error detect kar leta hai!

2️⃣ Execution Phase (Code Execution)
	•	Actual code execute hota hai aur variables apna assigned value le lete hain.

🎯 Example: Execution Context Ka Step-by-Step Breakdown (TypeScript)

console.log(x);  // ❌ ERROR: Cannot access 'x' before initialization
let x: number = 10;
function sayHello(): void {
    console.log("Hello");
}
sayHello();

Execution Context Breakdown

Step	Action
🔹 1	Compilation Phase: x ka type number assign hota hai
🔹 2	Memory Creation Phase: x = undefined, sayHello ka function reference store hota hai
🔹 3	Execution Phase: console.log(x); –> ERROR: Cannot access ‘x’ before initialization
🔹 4	Agar error fix hota toh x = 10; assign hota
🔹 5	sayHello() call hota, Call Stack pe push hota
🔹 6	console.log("Hello"); execute hota, “Hello” print hota
🔹 7	sayHello() stack se pop hota, execution context destroy hota

📌 TypeScript Execution Ka Extra Feature: Static Type Checking

TypeScript compile hone se pehle errors detect kar leta hai. Example:

function addNumbers(a: number, b: number): number {
    return a + b;
}

console.log(addNumbers(10, 5));  // ✅ Output: 15
console.log(addNumbers("10", 5));  // ❌ ERROR: Argument of type 'string' is not assignable to parameter of type 'number'.

TypeScript compiler pehle hi bata dega ki string aur number ka addition allowed nahi hai!

JavaScript me runtime error hota, par TypeScript compile time pe hi error detect kar leta hai.

🎥 Watch Live on YouTube Below:

💡 Conclusion
	•	Execution Context ek container hai jisme TypeScript ka code run hota hai.
	•	TypeScript ka execution JavaScript jaisa hi hota hai, bas extra step hota hai Compilation Phase.
	•	TypeScript Synchronous aur Single-Threaded hota hai, yani ek time pe ek hi task execute hota hai.
	•	Compilation Phase Type Errors detect kar leta hai jo JavaScript mein runtime error de sakta hai.
	•	Har function call ke liye naya execution context create hota hai jo Call Stack me push aur pop hota hai.

Agar aapko yeh samajh aaya toh agla step Hoisting, Scope Chain, aur TypeScript Features ko samajhna hoga! 🚀


