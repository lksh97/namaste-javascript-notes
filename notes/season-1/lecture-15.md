# Episode 15 : Asynchronous JavaScript & EVENT LOOP from scratch

> Note: Call stack will execeute any execeution context which enters it. Time, tide and JS waits for none. TLDR; Call stack has no timer.

* Browser has JS Engine which has Call Stack which has Global execution context, local execution context etc.
  * But browser has many other superpowers - Local storage space, Timer, place to enter URL, Bluetooth access, Geolocation access and so on.
  * Now JS needs some way to connect the callstack with all these superpowers. This is done using Web APIs.
  ![Event Loop 1 Demo](/assets/eventloop1.jpg)

### WebAPIs
None of the below are part of Javascript! These are extra superpowers that browser has. Browser gives access to JS callstack to use these powers.
![Event Loop 2 Demo](/assets/eventloop2.jpg)

* setTimeout(), DOM APIs, fetch(), localstorage, console (yes, even console.log is not JS!!), location and so many more.
    * setTimeout() : Timer function
    * DOM APIs : eg.Document.xxxx ; Used to access HTML DOM tree. (Document Object Manipulation
    * fetch() : Used to make connection with external servers eg. Netflix servers etc.

* We get all these inside call stack through global object ie. window
    * Use window keyword like : window.setTimeout(), window.localstorage, window.console.log() to log something inside console.
    * As window is global obj, and all the above functions are present in global object, we don't explicity write window but it is implied.

* Let's undertand the below code image and its explaination:
    ![Event Loop 3 Demo](/assets/eventloop3.jpg)
    * ```js
      console.log("start");
      setTimeout(function cb() {
          console.log("timer");
      }, 5000);
      console.log("end");
      // start end timer
      ```
    * First a GEC is created and put inside call stack.
    * console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console.
    * setTimeout(function cb() { //this calls the setTimeout web api which gives access to timer feature. It stores the callback cb() and starts timer. console.log("Callback");}, 5000);
    * console.log("End"); // calls console api and logs in console window. After this GEC pops from call stack.
    * While all this is happening, the timer is constantly ticking. After it becomes 0, the callback cb() has to run.
    * Now we need this cb to go into call stack. Only then will it be executed. For this we need **event loop** and **Callback queue**

### Event Loops and Callback Queue

Q: How after 5 secs timer is console?
* cb() cannot simply directly go to callstack to be execeuted. It goes through the callback queue when timer expires.
* Event loop keep checking the callback queue, and see if it has any element to puts it into call stack. It is like a gate keeper.
* Once cb() is in callback queue, eventloop pushes it to callstack to run. Console API is used and log printed
* ![Event Loop 4 Demo](/assets/eventloop4.jpg)

Q: Another example to understand Eventloop & Callback Queue.

See the below Image and code and try to understand the reason:
![Event Loop 5 Demo](/assets/eventloop5.jpg)
Explaination?

* ```js
  console.log("Start"); 
  document. getElementById("btn").addEventListener("click", function cb() { 
    // cb() registered inside webapi environment and event(click) attached to it. i.e. REGISTERING CALLBACK AND ATTACHING EVENT TO IT. 
    console.log("Callback");
  });
  console.log("End"); // calls console api and logs in console window. After this GEC get removed from call stack.
  // In above code, even after console prints "Start" and "End" and pops GEC out, the eventListener stays in webapi env(with hope that user may click it some day) until explicitly removed, or the browser is closed.
  ```

* Eventloop has just one job to keep checking callback queue and if found something push it to call stack and delete from callback queue.

Q: Need of callback queue?

**Ans**: Suppose user clciks button x6 times. So 6 cb() are put inside callback queue. Event loop sees if call stack is empty/has space and whether callback queue is not empty(6 elements here). Elements of callback queue popped off, put in callstack, executed and then popped off from call stack.

<br>

### Behaviour of fetch (**Microtask Queue?**)
Let's observe the code below and try to understand
```js
console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console. 
setTimeout(function cbT() { 
  console.log("CB Timeout");
}, 5000);
fetch("https://api.netflix.com").then(function cbF() {
    console.log("CB Netflix");
}); // take 2 seconds to bring response
// millions lines of code
console.log("End"); 

Code Explaination:
* Same steps for everything before fetch() in above code.
* fetch registers cbF into webapi environment along with existing cbT.
* cbT is waiting for 5000ms to end so that it can be put inside callback queue. cbF is waiting for data to be returned from Netflix servers gonna take 2 seconds.
* After this millions of lines of code is running, by the time millions line of code will execute, 5 seconds has finished and now the timer has expired and response from Netflix server is ready.
* Data back from cbF ready to be executed gets stored into something called a Microtask Queue.
* Also after expiration of timer, cbT is ready to execute in Callback Queue.
* Microtask Queue is exactly same as Callback Queue, but it has higher priority. Functions in Microtask Queue are executed earlier than Callback Queue.
* In console, first Start and End are printed in console. First cbF goes in callstack and "CB Netflix" is printed. cbF popped from callstack. Next cbT is removed from callback Queue, put in Call Stack, "CB Timeout" is printed, and cbT removed from callstack.
* See below Image for more understanding
```
![Event Loop 6 Demo](/assets/eventloop6.jpg)
Microtask Priority Visualization
![Event Loop 7 Demo](/assets/microtask.gif)

#### What enters the Microtask Queue ?
* All the callback functions that come through promises go in microtask Queue.
* **Mutation Observer** : Keeps on checking whether there is mutation in DOM tree or not, and if there, then it execeutes some callback function.
* Callback functions that come through promises and mutation observer go inside **Microtask Queue**.
* All the rest goes inside **Callback Queue aka. Task Queue**.
* If the task in microtask Queue keeps creating new tasks in the queue, element in callback queue never gets chance to be run. This is called **starvation**


### Some Important Questions 

1. **When does the event loop actually start ? -** Event loop, as the name suggests, is a single-thread, loop that is *almost infinite*. It's always running and doing its job.

2. **Are only asynchronous web api callbacks are registered in web api environment? -** YES, the synchronous callback functions like what we pass inside map, filter and reduce aren't registered in the Web API environment. It's just those async callback functions which go through all this.

3. **Does the web API environment stores only the callback function and pushes the same callback to queue/microtask queue? -** Yes, the callback functions are stored, and a reference is scheduled in the queues. Moreover, in the case of event listeners(for example click handlers), the original callbacks stay in the web API environment forever, that's why it's adviced to explicitly remove the listeners when not in use so that the garbage collector does its job.

4. **How does it matter if we delay for setTimeout would be 0ms. Then callback will move to queue without any wait ? -** No, there are trust issues with setTimeout() 😅. The callback function needs to wait until the Call Stack is empty. So the 0 ms callback might have to wait for 100ms also if the stack is busy.

<br>

### Observation of Eventloop, Callback Queue & Microtask Queue [**GiF**]
![microtask 1 Demo](/assets/microtask1.gif)
![microtask 2 Demo](/assets/microtask2.gif)
![microtask 3 Demo](/assets/microtask3.gif)
![microtask 4 Demo](/assets/microtask4.gif)
![microtask 5 Demo](/assets/microtask5.gif)
![microtask 6 Demo](/assets/microtask6.gif)

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=8zKuNo4ay8E&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/8zKuNo4ay8E/0.jpg" width="750"
alt="Asynchronous JavaScript & EVENT LOOP from scratch in JS Youtube Link"/></a>

---------

Chalo bhai, ab main aapko Episode 15 – **Asynchronous JavaScript & EVENT LOOP** ke baare mein Hinglish mein detail se samjhata hun. Is explanation mein hum text diagrams, examples aur detailed code comments use karenge, taaki aapko poori concept samajh aa jaye.

---

## 1. JavaScript Execution Context and Call Stack

**Concept:**  
JavaScript ka code ek call stack ke through execute hota hai. Call stack ek LIFO (Last-In, First-Out) structure hai jismein sabse pehle global execution context aata hai, phir har function call ka ek local execution context push hota hai aur jab execution complete ho jata hai, pop ho jata hai.

**Text Diagram:**  
```
           +-----------------------+
           |  Local Execution      | <-- function call (most recent)
           |  Context (f3)         |
           +-----------------------+
           |  Local Execution      | <-- function call (f2)
           |  Context (f2)         |
           +-----------------------+
           |  Local Execution      | <-- function call (f1)
           |  Context (f1)         |
           +-----------------------+
           |  Global Execution     | <-- Global Context
           |  Context              |
           +-----------------------+
```

**Example Code:**
```js
console.log("Global Start");   // Global Execution Context (GEC) mein execute hoga

function f1() {
  console.log("Inside f1");      // f1 ke local context mein execute
}

function f2() {
  f1();                        // f2 ke call se f1 ka context push hoga
  console.log("Inside f2");      // f2 ke context mein execute
}

f2();                          // Global context se f2 call, then f1 call inside f2
console.log("Global End");     // Global context mein execute, jab f2 complete ho jaye
```

---

## 2. Web APIs: Browser Ki Extra Superpowers

**Concept:**  
Browser ke paas JavaScript engine ke alawa bahut saare extra functionalities (superpowers) hote hain jaise ki:  
- **setTimeout()** – Timer function  
- **DOM APIs** – HTML elements ko access aur modify karne ke liye  
- **fetch()** – External servers se data fetch karne ke liye  
- **LocalStorage**, **Geolocation**, etc.  

Ye functions JavaScript ke native part nahi hote, balki browser ke environment (global object `window`) se provide kiye jate hain.

**Text Diagram:**
```
         +--------------------+
         |   Browser (Window) |
         |--------------------|
         |  Web APIs          |
         |  - setTimeout()    |
         |  - fetch()         |
         |  - DOM APIs        |
         |  - LocalStorage    |
         +--------------------+
                 │
                 ▼
         +--------------------+
         |   JS Engine        |  <-- Call Stack, Memory, Heap
         |  (V8, SpiderMonkey)|
         +--------------------+
```

**Example Code:**
```js
console.log("Start");
setTimeout(function cb() {
  console.log("Timer Callback");
}, 5000); // 5000 ms ka timer, browser ke Web API ko call karta hai
console.log("End");
```

**Explanation:**  
- **"Start"** aur **"End"** synchronous code hai, isliye pehle print honge.
- **setTimeout()** ek asynchronous Web API call hai. Jab timer expire hota hai, callback function (cb) callback queue mein chala jata hai.
- **Event Loop** dekhte hai jab call stack empty hota hai, tab callback queue se cb ko call stack mein push karke execute karta hai.

---

## 3. Event Loop, Callback Queue, aur Microtask Queue

### Event Loop and Callback Queue

**Concept:**  
- **Event Loop:** Ye ek infinite loop hai jo continuously check karta hai ki call stack empty hai ya nahi. Jab call stack empty hota hai, ye callback queue se pending callback functions ko call stack mein bhej deta hai execute karne ke liye.
- **Callback Queue (Task Queue):** Ye queue asynchronous callbacks (jaise setTimeout, DOM events) rakhti hai. In callbacks ko event loop tab check karta hai jab call stack empty ho.

**Text Diagram:**
```
      +-----------------+         +--------------------+
      |  Call Stack     | <------ |  Callback Queue    |
      +-----------------+         +--------------------+
               ▲                          │
               │                          │
         [Event Loop]  <------------------┘
```

**Example Code (setTimeout):**
```js
console.log("Start");
setTimeout(() => {
  console.log("Callback from setTimeout");
}, 3000);
console.log("End");
```
**Explanation:**  
- "Start" aur "End" call stack mein execute hote hain.
- setTimeout callback 3 sec ke baad callback queue mein chala jata hai.
- Event loop check karega jab call stack khali ho, tab callback ko call stack mein le aayega.

### Microtask Queue

**Concept:**  
- **Microtasks:** Ye queue promises, async/await ke callbacks, aur MutationObserver se related tasks rakhti hai.
- **Priority:** Microtask queue ka priority callback queue se zyada hota hai. Matlab, jab call stack empty hota hai, pehle microtasks execute hote hain, phir regular callbacks.

**Text Diagram:**
```
      +-----------------+ 
      |  Call Stack     |
      +-----------------+
              ▲
              │
      [Event Loop]
              │
      +-----------------+
      | Microtask Queue |  <-- Higher priority than callback queue
      +-----------------+
              │
      +-----------------+
      | Callback Queue  |
      +-----------------+
```

**Example Code (Promise):**
```js
console.log("Start");
setTimeout(() => {
  console.log("Callback from setTimeout");
}, 0);
Promise.resolve().then(() => {
  console.log("Promise Callback");
});
console.log("End");
```
**Explanation:**  
- "Start" aur "End" synchronous execution ke liye call stack mein chalte hain.
- setTimeout ke liye 0 ms ka timer milta hai, par callback queue mein jaata hai.
- Promise callback microtask queue mein jaata hai, jiski priority callback queue se zyada hai.
- Result: "Start", "End", "Promise Callback", phir "Callback from setTimeout" print hoga.

---

## 4. Detailed Code Explanation with Comments (Hinglish)

### Code Example:

```js
// Global Execution Start
console.log("Start"); // "Start" console pe print hota hai

// setTimeout Web API call: Timer start ho jata hai
setTimeout(function timerCallback() {
  // Ye callback function timer expire hone ke baad execute hoga
  console.log("Timer Callback"); // "Timer Callback" print hoga jab timer 5 sec complete ho jayega
}, 5000);

// Promise for asynchronous operation: Microtask
Promise.resolve().then(function promiseCallback() {
  // Ye callback promise resolution ke baad microtask queue mein jayega
  console.log("Promise Callback"); // "Promise Callback" jaldi execute hoga, even agar timer abhi pending ho
});

console.log("End"); // "End" console pe print hota hai

// Explanation:
// 1. Global Execution Context (GEC) create hota hai, jismein sabhi synchronous code execute hota hai.
// 2. "Start" print hota hai.
// 3. setTimeout call hota hai, callback function ko web API environment ko pass karta hai, timer shuru hota hai.
// 4. Promise.resolve() turant resolve hota hai aur promise callback microtask queue mein add hota hai.
// 5. "End" print hota hai.
// 6. Call Stack empty hote hi, Event Loop sabse pehle Microtask Queue check karta hai aur "Promise Callback" execute karta hai.
// 7. Fir Callback Queue check karta hai, timer expire hone par "Timer Callback" execute hota hai.
```

### Diagram for This Code:
```
Global Execution Context:
+------------------+
| console.log("Start")  |  // Print "Start"
| setTimeout(cb, 5000)  |  // Set timer for 5 sec, callback goes to Web API
| Promise.resolve().then(cb)  | // Promise callback queued in Microtask Queue
| console.log("End")    |  // Print "End"
+------------------+
       ↓ Call Stack becomes empty ↓
Event Loop:
    ┌───────────────────────────┐
    │    Microtask Queue        │   <-- Contains Promise Callback
    └───────────────────────────┘
            ↓ Execute Microtask
            "Promise Callback" printed
            ↓ Then check Callback Queue
            "Timer Callback" printed after 5 sec
```

---

## 5. Summary and What Next?

### Summary:
- **Call Stack:** Synchronous code execution ka stack, jahan functions LIFO order mein execute hote hain.
- **Web APIs:** Browser ke extra features jaise timer, fetch, DOM manipulation, etc. Ye functions JavaScript ka native part nahi hote.
- **Callback Queue:** Asynchronous callbacks (setTimeout, DOM events) yahan jaate hain, aur event loop call stack jab khali ho tab inhe execute karta hai.
- **Microtask Queue:** Promises aur mutation observers ke callbacks yahan aate hain, jinki priority callback queue se zyada hoti hai.
- **Event Loop:** Constantly check karta hai call stack, microtask queue aur callback queue, aur pending callbacks ko execute karta hai.

### What Next?
1. **Practice with Examples:**  
   - Khud se kuch asynchronous functions likho, setTimeout, Promise, async/await ke examples banao.
2. **Experiment with Microtask Queue:**  
   - Promise ke callbacks aur setTimeout ka compare karo, dekhna kaise microtasks pehle execute hote hain.
3. **Use Debugger:**  
   - Browser ke developer tools ka use karke step-by-step code execution observe karo.
4. **Learn Error Handling:**  
   - Try/Catch ka use karo asynchronous code mein aur samjho kaise exceptions propagate hote hain.
5. **Watch Related Videos:**  
   - YouTube pe "Asynchronous JavaScript & Event Loop" ke videos dekho, jisse aapko visual explanation milegi.
6. **Build Small Projects:**  
   - Chhota project banao jismein asynchronous code heavily use ho, jaise weather app ya API data fetch karna.

---

Is detailed explanation ke baad, aapko asynchronous JavaScript, event loop, microtask queue, aur callback queue ke concepts clear ho gaye honge. Agar koi specific sawal ho ya further examples chahiye ho, toh puch sakte ho. Happy coding, bhai!