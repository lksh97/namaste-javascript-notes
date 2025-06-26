# OOP in JavaScript – Ek Beginner Guide (Hinglish Mein)

Yeh article aapko JavaScript mein Object-Oriented Programming (OOP) ko samajhne mein madad karega. Isme hum theory ko detail mein cover karenge, saath hi text diagram aur examples ke through practical implementation bhi dekhenge. Chaliye shuru karte hain!

---

## **Overview**

- **OOP Kya Hai?**  
  OOP ek programming paradigm hai jo data (properties) aur methods (actions) ko ek saath group karta hai, jaise real-world objects.  
  - **Note:** JavaScript ek pure object-oriented language nahi hai. JavaScript mein objects aur prototypes ke through OOP concepts implement hote hain. Isliye hum kehte hain ki JavaScript ek **prototype-based procedural language** hai, jisme OOP aur functional programming dono ke elements maujood hain.

- **OOP ke 4 Primary Pillars:**
  1. **Encapsulation:** Data aur methods ko ek "box" mein band kar dena.
  2. **Inheritance:** Ek object ya class, dusre object ya class ke properties aur methods inherit kar sakta hai.
  3. **Polymorphism:** Ek hi function alag-alag context mein alag tarike se behave karta hai.
  4. **Abstraction:** Sirf zaroori details ko expose karna, baaki complex details ko hide karna.

- **JavaScript ka OOP Approach:**  
  JavaScript mein classes nahi hain (ya phir, ES6 ke baad `class` keyword sirf syntactic sugar hai prototypes ka use karne ka). Isliye, hum objects ko directly create karte hain aur prototypes ke through inheritance achieve karte hain.

---

## **Text Diagram: JavaScript OOP Structure**

             +-------------------------+
             |      JavaScript         |
             |  (Prototype-Based)      |
             +-------------------------+
                       |
      +----------------+----------------+
      |                                 |

+———––+                   +—————+
|  Objects    |                   |  Functions    |
| (Data &     |                   | (Methods,     |
|  Behavior)  |                   |  Callbacks)   |
+———––+                   +—————+
|                                 |
|                                 |
v                                 v
+—————––+            +–––––––––––––+
| Encapsulation     |            | Inheritance (Prototypes) |
| (Data + Methods)  |            | (Objects extend others)  |
+—————––+            +–––––––––––––+
|
v
+———————–+
| Polymorphism &        |
| Abstraction           |
| (Same function can    |
| behave differently)   |
+———————–+

---

## **1. Objects in JavaScript**

### **A. Using Object Literal**

Yeh sabse simple tareeka hai object create karne ka.  
**Example:**
```js
// Object literal se student object banaya
const student = {
    first_name: 'Mary',           // Property: first_name
    last_name: 'Green',           // Property: last_name
    display_full_name: function() {  // Method: display_full_name
        // "this" current object ko refer karta hai
        return `${this.first_name} ${this.last_name}`;
    }
};

console.log(student.display_full_name()); // Output: Mary Green

	Note: Object literal se banaye gaye objects hard-coded hote hain, jo 100 users ke liye likhna mushkil ho sakta hai.

B. Using Object Constructor

Constructor functions ka use karke aap easily multiple instances create kar sakte ho.
Example:

// Constructor function for student objects
function Student(first_name, last_name) {
    // "this" se current object ki properties define hoti hain
    this.first_name = first_name;
    this.last_name = last_name;
    this.display_full_name = function() {
        return `${this.first_name} ${this.last_name}`;
    };
}

// Constructor ke through instances create karna
const student1 = new Student("Mary", "Green");
const student2 = new Student("Lary", "Smith");

console.log(student2.display_full_name()); // Output: Lary Smith

	Detailed Comment:
		•	new keyword se ek naya object create hota hai aur this us object ko refer karta hai.
	•	Yeh approach DRY principle ko follow karti hai kyunki aap same constructor se multiple objects bana sakte ho.

C. Using Object.create() Method

Object.create() method se existing object ko prototype bana ke new object create kar sakte hain.
Example:

// Ek base student object define karte hain
const studentPrototype = {
    first_name: 'Mary',
    last_name: 'Green',
    display_full_name: function() {
        // "this" current object ke properties ko refer karega
        return `${this.first_name} ${this.last_name}`;
    }
};

// Object.create() se new instance create karna
const student1 = Object.create(studentPrototype);
student1.last_name = "Smith"; // Override property

console.log(student1.display_full_name()); // Output: Mary Smith

	Detailed Comment:
		•	Object.create() se jo object banta hai, uska prototype base object se linked hota hai.
	•	Isse inheritance achieve hoti hai bina constructor function ke.

2. Classes in JavaScript

JavaScript mein ES6 classes sirf syntactic sugar hai prototypes par based inheritance ka.

A. ES6 Class Syntax

Example:

// ES6 class syntax se Student class define karna
class Student {
    constructor(first_name, last_name) {
        // Constructor me properties initialize hoti hain
        this.first_name = first_name;
        this.last_name = last_name;
    }
    // Method ko class ke andar define karte hain
    display_full_name() {
        return `${this.first_name} ${this.last_name}`;
    }
}

// Instances create karna using new keyword
const student1 = new Student("Mary", "Green");
const student2 = new Student("Lary", "Smith");

console.log(student1.display_full_name()); // Output: Mary Green

	Detailed Comment:
		•	class keyword se class define karte hain.
	•	constructor function se object properties set hoti hain.
	•	Methods ko directly class ke body mein define kiya jata hai.

B. Traditional Constructor Function with Prototype

ES6 classes ke aane se pehle, aise hi constructor functions use kiye jate the.
Example:

// Constructor function se Student object banana
function Student(first_name, last_name) {
    this.first_name = first_name;
    this.last_name = last_name;
}

// Prototype pe method add karna, jisse saare instances me available ho jata hai
Student.prototype.display_full_name = function() {
    return `${this.first_name} ${this.last_name}`;
};

const student1 = new Student("Mary", "Green");
const student2 = new Student("Lary", "Smith");

console.log(student1.display_full_name()); // Output: Mary Green

	Detailed Comment:
		•	Yeh method memory efficient hai kyunki prototype ke methods har instance me share hote hain.

3. Encapsulation in JavaScript

Encapsulation ka matlab hai data aur functions ko ek box mein band kar dena, taaki bahar se direct access na ho. JavaScript mein, encapsulation achieve karne ke do tareeke hain:

A. Function Scope

function messageFunc() {
    // Yeh variable function ke andar encapsulated hai
    const message = "Hey there!";
    console.log("Inside function:", message);
}

messageFunc(); // Output: Hey there!

// Yahan, 'message' ko access karne ki koshish karein to error aayega
// console.log(message); // Error: message is not defined

	Detailed Comment:
		•	Function ke andar declare kiye gaye variables function ke bahar accessible nahi hote, isse encapsulation achieve hoti hai.

B. Closures

function messageFunc() {
    const message = "Hey there!";
    // Inner function jo outer function ke variable 'message' ko access kar sakta hai
    const displayMessage = function() {
        console.log("Inside closure:", message);
    }
    displayMessage();
}

messageFunc(); // Output: Hey there!

// 'message' variable abhi bhi bahar accessible nahi hai.

	Detailed Comment:
		•	Closure create hota hai jab inner function outer function ke scope se variables access karta hai, bina unko expose kiye.

4. Inheritance in JavaScript

JavaScript mein inheritance ko prototype-based inheritance ke through achieve kiya jata hai.

A. Prototypal Inheritance

// Parent object
const person = {
    legs: 2,
    arms: 2,
    describe: function() {
        return `I have ${this.legs} legs and ${this.arms} arms.`;
    }
};

// Child object inherits from person
const student = Object.create(person);
student.school = "Scaler Academy";

console.log(student.describe()); // Output: I have 2 legs and 2 arms.

	Detailed Comment:
		•	Object.create(person) se student object banata hai jiska prototype person object hai.
	•	Student object apne prototype (person) ke properties and methods inherit karta hai.

5. Polymorphism in JavaScript

Polymorphism ka matlab hai ek hi function ko alag-alag tarah se use karna.

A. Generic Functions (Parametric Polymorphism)

// Generic concat function jo arrays ya strings ke saath kaam karta hai.
const concatData = (x, y) => {
    return y.concat(x);
};

console.log(concatData([1, 2], [3, 4]));      // Output: [3, 4, 1, 2]
console.log(concatData("John", "Smith"));      // Output: SmithJohn

	Detailed Comment:
		•	Yeh function type-agnostic hai; arrays aur strings dono ke saath kaam karta hai, isliye yeh polymorphic hai.

6. JavaScript OOP in Real-World Scenario

JavaScript mein objects ka use karke hum real-world problems ko model kar sakte hain.

Example: Modeling a Student Object

Using Object Literal

// Object literal se ek student banaya
const student = {
    firstName: "Mary",
    lastName: "Green",
    // Function to display full name using this keyword
    displayFullName: function() {
        return `${this.firstName} ${this.lastName}`;
    }
};

console.log(student.displayFullName()); // Output: Mary Green

Using Constructor Function

// Constructor function se student objects create karna
function Student(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
}

// Prototype me method add karna
Student.prototype.displayFullName = function() {
    return `${this.firstName} ${this.lastName}`;
};

const student1 = new Student("Mary", "Green");
const student2 = new Student("Lary", "Smith");

console.log(student1.displayFullName()); // Output: Mary Green
console.log(student2.displayFullName()); // Output: Lary Smith

Using ES6 Class

// ES6 class syntax se student class banayein
class Student {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    // Method defined within class body
    displayFullName() {
        return `${this.firstName} ${this.lastName}`;
    }
}

const student1 = new Student("Mary", "Green");
const student2 = new Student("Lary", "Smith");

console.log(student1.displayFullName()); // Output: Mary Green
console.log(student2.displayFullName()); // Output: Lary Smith

Conclusion

JavaScript ek prototype-based language hai jisme traditional OOP ke principles (encapsulation, inheritance, polymorphism, abstraction) implement kiye ja sakte hain, chahe aap object literal, constructor functions, ya ES6 classes ka use karo.
	•	Encapsulation: Function scope aur closures ke through data ko secure rakhna.
	•	Inheritance: Prototypes ke through parent-child relationship establish karna.
	•	Polymorphism: Generic functions ya overloaded behavior se code reusability aur flexibility.

Yeh concepts aapko end-to-end OOP in JavaScript samajhne mein madad karenge, chahe aap beginner ho ya advanced. Detailed code comments aur diagrams se aap theory ko practically implement karna seekh sakte ho.

Challenge:
Ab aap in concepts ko khud practice karo aur apne projects mein implement karo. Isse aapko real-world applications mein OOP ke principles ka istemal samajh aayega.

Watch Live on YouTube:

