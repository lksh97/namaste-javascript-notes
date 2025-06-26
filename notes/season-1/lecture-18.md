# Episode 18: Higher-Order Functions ft. Functional Programming

## Higher-Order Function Kya Hota Hai?

**Answer:**  
Higher-order function woh function hota hai jo ya toh:  
1. **Ek ya zyada functions ko argument ke roop mein leta hai:**  
    - Isse aap dynamically decide kar sakte ho ki function ko kaunsa operation perform karna hai, based on jo function pass kiya gaya hai.  
2. **Ek function return karta hai:**  
    - Isse complex aur reusable code likhna asaan ho jata hai.

**Example:**

```js
// Ek simple function jo "Hi" print karta hai.
function sayHello() {
    console.log("Hi");
}

// Ek higher-order function jo ek function ko argument ke roop me leta hai aur use execute karta hai.
function executeFunction(fn) {
    fn();
}

executeFunction(sayHello); // Output: Hi

// Yahan, executeFunction ek higher-order function hai kyunki yeh ek function (sayHello) ko argument ke roop mein leta hai.
// Aur sayHello ek callback function hai.

Callback Function Kya Hota Hai?

Answer:
Callback function woh function hota hai jo kisi aur function ko argument ke roop mein pass kiya jata hai aur us function ke andar call hota hai.
Isse aap koi action defer kar sakte ho jab tak koi condition satisfy na ho jaye ya koi event na ho.

Example:

// Ek greet function jo naam ke saath greeting print karta hai.
function greet(name) {
    console.log("Hello, " + name);
}

// processUserInput function, jo ek callback function leta hai.
function processUserInput(callback) {
    const name = "Alice";
    callback(name);
}

processUserInput(greet); // Output: Hello, Alice

// Yahan, greet ek callback function hai kyunki isse processUserInput function ko argument ke roop me diya gaya hai aur later call kiya gaya hai.

Applying Higher-Order Functions in Problem Solving

Higher-order functions real-world problems solve karne mein kaafi useful hote hain.
Chaliye dekhte hain kaise aap ek array ke data ke saath operations apply kar sakte hain, jaise ki area aur circumference calculate karna.

Scenario: Calculating Area and Circumference

Suppose:
Aapke paas ek array hai radii ka, aur aapko har radius ke liye area calculate karna hai aur results ko ek nayi array mein store karna hai.

Text Diagram

          +----------------+
          |  radiusArr     |     // [1, 2, 3, 4]
          +----------------+
                   |
                   v
          +------------------+          +--------------------+
          | Higher-Order     |  ------> | Operation (area)   |
          | Function         |          +--------------------+
          | calculate()      |          | Operation (circumference) |
          +------------------+          +--------------------+
                   |
                   v
          +------------------+
          |  Output Array    |   // [calculated values]
          +------------------+

First Approach (Without Higher-Order Function)

// Array of radii
const radius = [1, 2, 3, 4];

// Calculate area without using higher-order functions
const calculateArea = function(radius) {
    const output = [];
    for (let i = 0; i < radius.length; i++) {
        output.push(Math.PI * radius[i] * radius[i]);
    } 
    return output;
}

console.log(calculateArea(radius)); // Expected Output: [3.14, 12.57, 28.27, 50.27]

	Note:
Yeh solution sahi hai lekin agar aapko circumference bhi calculate karna ho, toh aapko alag se code likhna padega.

const calculateCircumference = function(radius) {
    const output = [];
    for (let i = 0; i < radius.length; i++) {
        output.push(2 * Math.PI * radius[i]);
    } 
    return output;
}

console.log(calculateCircumference(radius)); // Expected Output: [6.28, 12.57, 18.85, 25.13]

	Problem:
Yeh do alag functions likhne se DRY (Don’t Repeat Yourself) principle violate hota hai.

Better Approach Using Higher-Order Functions

Is approach mein hum ek higher-order function banayenge jo kisi bhi operation (function) ko array ke har element par apply karega. Isse code repeat nahi karna padega.

// Array of radii
const radiusArr = [1, 2, 3, 4];

// Function to calculate area (πr²)
const area = function(radius) {
    // Yeh function input radius se area calculate karta hai.
    return Math.PI * radius * radius;
}

// Function to calculate circumference (2πr)
const circumference = function(radius) {
    // Yeh function input radius se circumference calculate karta hai.
    return 2 * Math.PI * radius;
}

// Higher-order function jo kisi bhi operation ko array ke har element par apply karta hai.
const calculate = function(radiusArr, operation) {
    const output = [];
    // Har element par operation apply karo.
    for (let i = 0; i < radiusArr.length; i++) {
        output.push(operation(radiusArr[i])); // Operation call ho raha hai.
    } 
    return output;
}

console.log(calculate(radiusArr, area));          // Expected Output: [3.14, 12.57, 28.27, 50.27]
console.log(calculate(radiusArr, circumference)); // Expected Output: [6.28, 12.57, 18.85, 25.13]

// Yahan, 'calculate' ek higher-order function hai jo kisi bhi function (area ya circumference) ko callback ke roop mein leta hai.

Detailed Code Comments in Hinglish:

// radiusArr: Ek array jo different radii ko store karta hai.
const radiusArr = [1, 2, 3, 4];

// area function: Yeh function radius ka use karke area calculate karta hai.
// Formula: π * r * r
const area = function(radius) {
    return Math.PI * radius * radius;
}

// circumference function: Yeh function radius ka use karke circumference calculate karta hai.
// Formula: 2 * π * r
const circumference = function(radius) {
    return 2 * Math.PI * radius;
}

// calculate function: Higher-order function jo array ke har element par di gayi operation apply karta hai.
const calculate = function(radiusArr, operation) {
    const output = [];
    // Loop chala ke har element par operation apply karo.
    for (let i = 0; i < radiusArr.length; i++) {
        output.push(operation(radiusArr[i])); // Yahan, area ya circumference function call hota hai.
    } 
    return output;
}

// calculate function ka use karke results print karna.
console.log(calculate(radiusArr, area));          // Output: [3.14, 12.57, 28.27, 50.27]
console.log(calculate(radiusArr, circumference)); // Output: [6.28, 12.57, 18.85, 25.13]

Polyfill for the map Function

calculate function built-in map function ke jaisa hi hai.
map function array ke har element par ek function apply karke ek nayi array return karta hai.

Example:

console.log(radiusArr.map(area)); // Expected Output: [3.14, 12.57, 28.27, 50.27]

// Custom map function create karne ke liye:
Array.prototype.calculate = function(operation) {
    const output = [];
    for (let i = 0; i < this.length; i++) {
        output.push(operation(this[i]));
    } 
    return output;
}

console.log(radiusArr.calculate(area)); // Expected Output: [3.14, 12.57, 28.27, 50.27]

Quick Recap with Diagram

Diagram:

          [1, 2, 3, 4]  <-- radiusArr
                |
                v
         +--------------+      apply operation (area / circumference)
         | calculate()  |  -------------------------->
         +--------------+      
                |
                v
         [calculated values]   <-- Output Array

Detailed Code Comments (Recap):
	•	radiusArr: Array of radii.
	•	area function: Calculates area using formula π * r * r.
	•	circumference function: Calculates circumference using formula 2 * π * r.
	•	calculate function: A higher-order function that applies a given operation (area or circumference) to each element in radiusArr and returns a new array of results.
	•	Built-in map: JavaScript’s built-in map function does the same, so our custom calculate function mimics that behavior.

Conclusion

Higher-order functions aur callback functions se aap apna code modular, reusable, aur maintainable bana sakte ho. Is approach se aap complex operations ko simplify karke easily test aur debug kar sakte ho.

Watch Live on YouTube:
```


-------

# Jist Episode 18: Higher-Order Functions aur Functional Programming ka Maza

### Q: Higher-Order Function kya hota hai?
**Ans**: Higher-order function ek aisa function hota hai jo:
1. **Doosre functions ko argument ke taur pe leta hai**: Isse function dynamically decide kar sakta hai ki kya operation perform karna hai.
2. **Ya phir ek function ko result ke taur pe return karta hai**: Isse complex aur reusable code banaya ja sakta hai.

**Example**:
```js
function namaste() {
    console.log("Namaste ji!");
};

function functionChalao(kuchBhiFunction) {
    kuchBhiFunction();
};

functionChalao(namaste); // Output: Namaste ji!
// functionChalao ek higher-order function hai kyunki ye doosre function (namaste) ko argument ke taur pe le raha hai.
// namaste ek callback function hai.
```

### Q: Callback Function kya hota hai?
**Ans**: Callback function wo function hota hai jo doosre function ko argument ke taur pe pass kiya jata hai aur baad mein execute hota hai. Isse aap kisi action ko tab tak delay kar sakte hain jab tak koi condition meet na ho jaye ya koi event na ho jaye.

**Example**:
```js
function swagat(naam) {
    console.log("Namaste, " + naam + " ji!");
}

function userKaNaamLo(callback) {
    const naam = "Rajesh";
    callback(naam);
}

userKaNaamLo(swagat); // Output: Namaste, Rajesh ji!
// Yahan pe, swagat ek callback function hai kyunki ye userKaNaamLo ko pass kiya gaya hai aur baad mein execute hota hai.
```

### Real Life Mein Higher-Order Functions Ka Use

Chalo dekhte hain ki real life mein higher-order functions kaise useful ho sakte hain. Maan lo humein ek array mein se har radius ke liye area nikalna hai.

**Pehla Tareeka**:
```js
const radius = [3, 1, 2, 4];

const areaNikalo = function(radius) {
    const result = [];
    for (let i = 0; i < radius.length; i++) {
        result.push(Math.PI * radius[i] * radius[i]);
    } 
    return result;
}

console.log(areaNikalo(radius)); // Output: [28.27, 3.14, 12.57, 50.27]
// Ye sahi kaam kar raha hai, lekin agar humein ab circumference bhi nikalna ho to?
```

**Doosra Tareeka (Higher-Order Function Ka Use Karke)**: