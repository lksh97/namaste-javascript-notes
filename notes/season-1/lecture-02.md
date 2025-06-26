# Episode 2 : How JS is executed & Call Stack

* When a JS program is ran, a **global execution context** is created.

* The execution context is created in two phases.
  * Memory creation phase - JS will allocate memory to variables and functions.
  * Code execution phase

* Let's consider the below example and its code execution steps:
```js
1. var n = 2;

2. function square(num) {
3.    var ans = num * num;
4.    return ans;
5. }

6. var square2 = square(n);
7. var square4 = square(4);
```
- The very **first** thing which JS does is **memory creation phase**
  - So it goes to `line one` of above code snippet, and `allocates a memory space` for variable **'n'** 
    - When allocating memory **for n it stores 'undefined'**, a special value for 'n'. 

  - Then goes to `line two`, and **allocates a memory space** for **function 'square'**
    - **For 'square', it stores the whole code of the function inside its memory space.** 

  - Then, as square2 and square4 are variables as well, it allocates memory and stores 'undefined' for them, and this is the **end of first phase** i.e. `memory creation phase`.

So O/P will look something like

![Execution Context Phase 1](/assets/phase1.jpg "Execution Context")

##### Same code as above
```js
1. var n = 2;

2. function square(num) {
3.    var ans = num * num;
4.    return ans;
5. }

6. var square2 = square(n);
7. var square4 = square(4);
```

- Now, in **2nd phase** i.e. `code execution phase`,
  - it starts going through the whole code line by line. 
    - As it encounters var n = 2, it assigns 2 to 'n'. Until now, the value of 'n' was undefined. For function, there is nothing to execute. As these lines were already dealt with in memory creation phase.
    
  - Coming to line 6 i.e. `var square2 = square(n)`, here **functions are a bit different than any other language**. `A new execution context is created altogether`.
    - Again in this new execution context, in `memory creation phase`, we allocate memory to num and ans the two variables. And undefined is placed in them.
    - Now, `in code execution phase` of this execution context, first <ins>2 is assigned to num</ins>.
      - Then `var ans = num * num` will <ins>store 4 in ans</ins>. 
      - After that, `return ans` returns the control of program back to where this function was invoked from.

![Execution Context Phase 2](/assets/phase2.jpg "Execution Context")

- When <ins>return</ins> keyword is encountered, It returns the control to the called line and also `the function execution context is deleted`.
  - Same thing will be repeated for **square4** and then after that is finished, the `global execution context will be destroyed` (As entire code is executed).

- So the **Final diagram** before deletion would look something like:

![Execution Context Phase 2](/assets/final_execution_context.jpg "Execution Context")

* Javascript manages code execution context creation and deletion with the the help of `Call Stack`.

* Call Stack is a mechanism to keep track of its place in script that calls multiple function.

* Call Stack maintains the `order of execution of execution contexts`. It is also known as Program Stack, Control Stack, Runtime stack, Machine Stack, Execution context stack.

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=iLWTnMzWtj4&t=1s&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/iLWTnMzWtj4/0.jpg" width="750"
alt="How JS is executed & Call Stack Youtube Link"/></a>

---

🚀 JavaScript Execution & Call Stack - Hinglish Explanation for Beginners

JavaScript ek single-threaded language hai, jo synchronous execution follow karti hai. Jab bhi hum koi JavaScript program run karte hain, ek Global Execution Context (GEC) create hota hai jo 2 phases mein execute hota hai:
	1.	Memory Creation Phase
	•	Variables aur functions ka memory allocation hota hai.
	•	Variables ko undefined assign hota hai.
	•	Functions ka pura code memory mein store hota hai.
	2.	Code Execution Phase
	•	Line-by-line code execute hota hai.
	•	Variables ko actual value assign hoti hai.
	•	Functions call hone par naya execution context banta hai.

📝 Execution Flow Ka Example Samjhein

1. var n = 2;

2. function square(num) {
3.    var ans = num * num;
4.    return ans;
5. }

6. var square2 = square(n);
7. var square4 = square(4);

🔵 Step 1: Memory Creation Phase (Allocation of Variables & Functions)
	•	Sabhi variables aur functions ko memory allocate hoti hai.
	•	Variables ko undefined assign hota hai.
	•	Functions ka pura function definition store hota hai.

Variable / Function	Allocated Value
n	undefined
square	function square(num) {...}
square2	undefined
square4	undefined

🔹 Diagram Representation:

Global Execution Context (GEC)
----------------------------------
Memory Component:
    n          -> undefined
    square     -> function square(num) { var ans = num * num; return ans; }
    square2    -> undefined
    square4    -> undefined
Code Component:
    (Execution will start)
----------------------------------

🔴 Step 2: Code Execution Phase (Line-by-Line Execution)
	•	n = 2 assign hota hai.
	•	Function square(num) abhi execute nahi hoga, bas memory mein pada rahega.

Line Number	Code Execution
Line 1	n = 2;
Line 2-5	Function square ka definition hai, execute nahi hoga.
Line 6	square(n) call hota hai. Naya Execution Context create hota hai!

🟡 Step 3: Function Call Execution (New Execution Context)

Jab square(n) call hota hai, ek naya execution context create hota hai, jisme 2 phases hote hain:

1️⃣ Memory Creation Phase (New Execution Context)
	•	num ko undefined assign hota hai.
	•	ans ko undefined assign hota hai.

2️⃣ Code Execution Phase
	•	num = 2 assign hota hai.
	•	ans = num * num → ans = 2 * 2 = 4.
	•	return ans → 4 return hoke square2 mein store hota hai.

🔹 Diagram Representation:

Call Stack:
----------------------------------
Global Execution Context (GEC)
   n = 2
   square2 = undefined (waiting for return)
   square4 = undefined

Execution Context for square(n)
   num = 2
   ans = 4
   return 4  --> Goes back to GEC
----------------------------------

Jaise hi return ans execute hota hai, ye execution context destroy ho jata hai!

🟠 Step 4: Next Function Call (square(4))

Jab square(4) call hota hai, naya execution context create hota hai:

1️⃣ Memory Creation Phase
	•	num ko undefined assign hota hai.
	•	ans ko undefined assign hota hai.

2️⃣ Code Execution Phase
	•	num = 4 assign hota hai.
	•	ans = 4 * 4 = 16 assign hota hai.
	•	return ans → square4 = 16 store hota hai.

🔹 Diagram Representation:

Call Stack:
----------------------------------
Global Execution Context (GEC)
   n = 2
   square2 = 4
   square4 = undefined (waiting for return)

Execution Context for square(4)
   num = 4
   ans = 16
   return 16  --> Goes back to GEC
----------------------------------

Jaise hi return ans execute hota hai, ye execution context bhi destroy ho jata hai.

🔵 Step 5: Call Stack Ka Role
	•	JavaScript ka Call Stack execution contexts ka order maintain karta hai.
	•	Jaise hi koi function call hota hai, Call Stack pe push hota hai.
	•	Jab function return hota hai, Call Stack se pop hota hai.

🔹 Final Call Stack Flow:

1️⃣ Global Execution Context (Start)
2️⃣ square(n) Execution Context (Push)
3️⃣ square(n) Execution Context (Pop - Return 4)
4️⃣ square(4) Execution Context (Push)
5️⃣ square(4) Execution Context (Pop - Return 16)
6️⃣ Global Execution Context (Destroy after Execution)

💡 Jaise hi saara code execute ho jata hai, Global Execution Context bhi destroy ho jata hai!

🎯 Summary
	•	Jab JavaScript ka program execute hota hai, ek Global Execution Context (GEC) create hota hai.
	•	2 Phases hote hain:
	1.	Memory Creation Phase: Variables ko undefined assign hota hai, aur functions ka pura definition store hota hai.
	2.	Code Execution Phase: Line-by-line execution hota hai, aur function calls ke liye New Execution Context create hota hai.
	•	Call Stack track karta hai ki kaunsa execution context kab push aur pop hoga.
	•	Jaise hi saara code execute ho jata hai, Call Stack empty ho jata hai aur Global Execution Context destroy hota hai.

🚀 Watch Live on YouTube Below

💡 Next Steps:

Agar yeh samajh aaya, toh Scope Chain, Hoisting, aur Closures ki taraf badhna chahiye! 🚀


----

🚀 JavaScript & TypeScript Execution Flow with Call Stack - Hinglish Guide for Beginners

JavaScript aur TypeScript dono single-threaded languages hain jo synchronous execution follow karti hain. Jab bhi hum koi program execute karte hain, ek Global Execution Context (GEC) create hota hai jo 2 phases mein execute hota hai:
	1.	Memory Creation Phase
	•	Variables aur functions ka memory allocation hota hai.
	•	Variables ko undefined assign hota hai.
	•	Functions ka pura code memory mein store hota hai.
	2.	Code Execution Phase
	•	Line-by-line code execute hota hai.
	•	Variables ko actual value assign hoti hai.
	•	Functions call hone par naya execution context banta hai.

🔷 JavaScript Execution Context Flow (Example Samjhein)

🔹 JavaScript Code:

var n = 2;

function square(num) {
    var ans = num * num;
    return ans;
}

var square2 = square(n);
var square4 = square(4);

🟢 Step 1: Memory Creation Phase (Allocation of Variables & Functions)
	•	JavaScript variables ko “undefined” set karta hai aur functions ka pura code memory mein store hota hai.

Variable / Function	Allocated Value
n	undefined
square	function square(num) {...}
square2	undefined
square4	undefined

🔹 Diagram Representation (JS Memory Allocation Phase)

Global Execution Context (GEC)
----------------------------------
Memory Component:
    n          -> undefined
    square     -> function square(num) { var ans = num * num; return ans; }
    square2    -> undefined
    square4    -> undefined
Code Component:
    (Execution will start)
----------------------------------

🔴 Step 2: Code Execution Phase (Line-by-Line Execution)
	•	n = 2 assign hota hai.
	•	Function square(num) abhi execute nahi hoga, bas memory mein pada rahega.
	•	Jab square(n) call hota hai, ek naya execution context create hota hai!

🔹 Diagram Representation (JS Execution Phase)

Call Stack:
----------------------------------
Global Execution Context (GEC)
   n = 2
   square2 = undefined (waiting for return)
   square4 = undefined

Execution Context for square(n)
   num = 2
   ans = 4
   return 4  --> Goes back to GEC
----------------------------------

Jaise hi return ans execute hota hai, ye execution context destroy ho jata hai!

🟠 TypeScript Mein Kya Difference Hai?

TypeScript mein strict typing aur compile-time error detection hoti hai, jo JavaScript mein nahi hoti. Agar same code TypeScript mein likhein:

🔹 TypeScript Code (With Type Annotations):

let n: number = 2;

function square(num: number): number {
    let ans: number = num * num;
    return ans;
}

let square2: number = square(n);
let square4: number = square(4);

✨ TypeScript Mein Execution Flow Mein Kya Differences Hain?

1️⃣ Memory Creation Phase
	•	JavaScript ki tarah TypeScript bhi variables ko memory allocate karta hai, lekin TypeScript pehle hi ensure karega ki variable ka correct type assign ho.
	•	Agar hum n ko undefined assign karne ki koshish karein, to TypeScript compile-time error dega.

2️⃣ Code Execution Phase
	•	Code execution bilkul JavaScript ki tarah hoga, lekin compile hone ke baad TypeScript ka code JavaScript mein convert ho jata hai (Transpilation).
	•	TypeScript bas compile-time safety provide karta hai, execution runtime pe same hoga JavaScript ke jaise.

🔹 Diagram Representation (TS Memory Allocation Phase)

Global Execution Context (GEC) - TypeScript
----------------------------------
Memory Component:
    n          -> 2 (Type is strictly number)
    square     -> function square(num: number) { let ans: number = num * num; return ans; }
    square2    -> undefined
    square4    -> undefined
Code Component:
    (Execution will start)
----------------------------------

🔹 Diagram Representation (TS Execution Phase)

Call Stack:
----------------------------------
Global Execution Context (GEC) - TypeScript
   n = 2
   square2 = undefined (waiting for return)
   square4 = undefined

Execution Context for square(n)
   num = 2
   ans = 4
   return 4  --> Goes back to GEC
----------------------------------

💡 Key Difference in TypeScript:
✅ Compile-Time Checking: Agar hum square("hello") pass karein to TypeScript error dega, whereas JavaScript runtime pe crash hoga.
✅ Type Annotations: Variables aur function parameters mein types specify kar sakte hain.
✅ Transpilation: TypeScript code ko JS mein convert karna padta hai before execution.

🔵 Call Stack in JavaScript vs TypeScript

JavaScript aur TypeScript dono same Call Stack mechanism follow karte hain, lekin TypeScript additional compile-time safety provide karta hai.

🔹 JavaScript Call Stack Flow:

1️⃣ Global Execution Context (Start)
2️⃣ square(n) Execution Context (Push)
3️⃣ square(n) Execution Context (Pop - Return 4)
4️⃣ square(4) Execution Context (Push)
5️⃣ square(4) Execution Context (Pop - Return 16)
6️⃣ Global Execution Context (Destroy after Execution)

🔹 TypeScript Call Stack Flow (Same as JavaScript)

1️⃣ Global Execution Context (Start)
2️⃣ square(n) Execution Context (Push)
3️⃣ square(n) Execution Context (Pop - Return 4)
4️⃣ square(4) Execution Context (Push)
5️⃣ square(4) Execution Context (Pop - Return 16)
6️⃣ Global Execution Context (Destroy after Execution)

💡 Difference: TypeScript sirf Compile-Time Type Checking provide karta hai, execution time pe JavaScript aur TypeScript exactly same behave karte hain!

🎯 Summary (JS vs TS Execution)

Feature	JavaScript	TypeScript
Type System	Dynamic Typing (No Type Safety)	Static Typing (Type Safety Provided)
Compile-Time Errors	No Compile-Time Error Checking	Errors Detect Before Execution
Variable Initialization	undefined Assign Hota Hai	Type Constraints Ke Saath Initialize Hota Hai
Call Stack Behavior	Same Execution as TypeScript	Same Execution as JavaScript
Function Execution	Directly Executed	Compile Hone Ke Baad Execute Hota Hai
Transpilation	Not Needed	TypeScript → JavaScript Convert Hota Hai

🚀 Conclusion
	1.	JavaScript aur TypeScript ka execution flow same hota hai, bas TypeScript me Compile-Time Error Checking hoti hai.
	2.	Call Stack dono languages mein same kaam karta hai, execution order same hota hai.
	3.	TypeScript Compile hone ke baad JavaScript me convert hota hai, isliye runtime behavior same rehta hai.

Agar yeh samajh aaya toh Next Step: Scope Chain, Hoisting, Closures padho! 🚀

📺 Watch Live on YouTube