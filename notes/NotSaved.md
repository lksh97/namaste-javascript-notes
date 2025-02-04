Certainly! Let's break down the concept of callbacks, `setTimeout`, and the example provided with more detailed explanations, code examples, and diagrams to make it clear for beginners.

---

# Episode 20: Understanding Callback Functions in JavaScript

## What is a Callback Function?

A **callback function** is a function that you pass to another function as an argument, and the receiving function is responsible for executing it at some point in the future.

### Simple Example

Let's start with a simple example to demonstrate how a callback works:

```javascript
function greet(name) {
    console.log("Hello " + name);
}

function sayGoodbye(name) {
    console.log("Goodbye " + name);
}

function performAction(action, name) {
    action(name); // Here, the `action` is the callback function that gets executed.
}

performAction(greet, "Alice");    // Output: Hello Alice
performAction(sayGoodbye, "Bob"); // Output: Goodbye Bob
```

In the above code:
- `greet` and `sayGoodbye` are two simple functions.
- `performAction` is a higher-order function that accepts another function (`action`) as an argument and executes it.
- We pass `greet` and `sayGoodbye` as callbacks to `performAction`, which then executes them.

### Using `setTimeout` to Introduce Asynchronous Behavior

JavaScript is synchronous, which means it executes code line by line. But what if you want to delay some code? That's where `setTimeout` comes in.

```javascript
console.log("Step 1: Start");

setTimeout(function () {
    console.log("Step 3: Executed after 2 seconds");
}, 2000);

console.log("Step 2: Continue");

// Output:
// Step 1: Start
// Step 2: Continue
// Step 3: Executed after 2 seconds
```

In this example:
- The first `console.log` runs immediately.
- The `setTimeout` function sets a timer for 2 seconds (2000 milliseconds) and schedules the callback function to run after that time.
- The code doesn't wait for 2 seconds; it immediately moves on to the next line and logs "Step 2: Continue".
- After 2 seconds, the callback function inside `setTimeout` is executed, logging "Step 3: Executed after 2 seconds".

## Real-World Example: e-Commerce Website Workflow

Let's imagine you’re working on an e-commerce website. When a user places an order, several steps happen in sequence:

1. **Create the Order**: The order is created in the system.
2. **Proceed to Payment**: The user is charged for the order.
3. **Show Order Summary**: The order summary is displayed to the user.

You want to make sure that each step is completed before moving on to the next. Here’s how you might implement this using callbacks:

### Initial Approach (Without Callbacks)

```javascript
const cart = ["shoes", "shirt"];

api.createOrder(cart);
api.proceedToPayment();
api.showOrderSummary();
```

In the above code:
- We assume `createOrder`, `proceedToPayment`, and `showOrderSummary` are functions provided by the `api`.
- This code works if all operations are synchronous, but in the real world, creating an order might take some time, such as when communicating with a server. We need to ensure that the order is fully created before moving on to payment.

### Improved Approach Using Callbacks

Here’s how you would ensure each step happens only after the previous one is completed:

```javascript
api.createOrder(cart, function () {
    api.proceedToPayment(function () {
        api.showOrderSummary();
    });
});
```

Let's break it down:

1. **Step 1: Create Order**:
   - The `api.createOrder` function is called with the `cart` and a callback function.
   - The callback function is executed once the order creation is complete.

2. **Step 2: Proceed to Payment**:
   - The callback function passed to `createOrder` then calls `api.proceedToPayment`.
   - This, too, takes a callback function that will execute only after payment is completed.

3. **Step 3: Show Order Summary**:
   - Finally, the callback passed to `proceedToPayment` calls `api.showOrderSummary`, which shows the user their order details.

### Visualizing the Callback Flow

Here’s a visual representation of how the callbacks work:

```
createOrder -----> proceedToPayment -----> showOrderSummary
   |
   +--> Calls callback to proceedToPayment
        |
        +--> Calls callback to showOrderSummary
```

- **createOrder** starts the process and passes control to **proceedToPayment** after it’s done.
- **proceedToPayment** then passes control to **showOrderSummary** once payment is complete.

### Callback Hell (Pyramid of Doom)

As you can see, the code starts to get nested and harder to manage. This is known as **Callback Hell** or the **Pyramid of Doom** because of the way the code structure starts looking like a pyramid.

```javascript
api.createOrder(cart, function () {
    api.proceedToPayment(function () {
        api.showOrderSummary(function () {
            api.updateWallet(function () {
                console.log("All done!");
            });
        });
    });
});
```

With each additional step, the code gets more indented and difficult to read, making it harder to maintain and debug.

### Problems with Callbacks: Inversion of Control

**Inversion of Control** occurs when you hand over control of your code to another function. In the example above, the flow of what happens when is no longer fully under your control—it’s up to the functions you’ve passed callbacks to.

```javascript
api.createOrder(cart, function () {
    api.proceedToPayment();
    // Trusting createOrder to call proceedToPayment at the right time
});
```

This can be risky because:
- What if `createOrder` never calls the callback?
- What if it calls the callback twice?
- What if something goes wrong inside `createOrder` and the callback is never executed?

This loss of control is why callbacks can be tricky, especially in larger, more complex systems.

### A Better Solution: Promises (Sneak Peek)

To avoid the problems of callback hell and inversion of control, JavaScript provides **Promises**, which allow you to write asynchronous code in a more readable and manageable way. Promises will be covered in the next session, but for now, just know that they help in avoiding the issues we discussed.

---

## Summary

- **Callbacks** are essential for handling asynchronous operations in JavaScript, but they can lead to complex and hard-to-maintain code, especially when heavily nested.
- **setTimeout** is a simple example of how asynchronous behavior can be introduced in JavaScript using callbacks.
- **Callback Hell** and **Inversion of Control** are two main issues with using callbacks in complex applications.
- **Promises** are a modern way to handle asynchronous operations more cleanly, which we'll explore next.

---

By understanding these examples and concepts, you’ll have a solid foundation for working with asynchronous code in JavaScript. As you move on to more advanced topics like Promises, this knowledge will be invaluable.



--------------------------------------------------------------------------------------------------------------------

Difference between JavaScript and TypeScript
JavaScript
JavaScript is the most popular programming language of HTML and the Web. JavaScript is an object-based scripting language which is lightweight and cross-platform. It is used to create client-side dynamic pages. The programs in JavaScript language are called scripts. The scripts are written in HTML pages and executed automatically as the page loads. It is provided and executed as plain text and does not need special preparation or compilation to run.

History of JavaScript
Netscape Communications Corporation programmer Brendan Eich developed JavaScript. It was introduced in September 1995, which was initially called Mocha. However, after gaining popularity as the best scripting tool, it was renamed as JavaScript to reflect Netscape's support of Java within its browser. In November 1996, Netscape submitted JavaScript to ECMA (European Computer Manufacturers Association). The current version of JavaScript is ECMAScript 2018, which was released in June 2018./p>

TypeScript
TypeScript is an open-source pure object-oriented programing language. It is a strongly typed superset of JavaScript which compiles to plain JavaScript. TypeScript is developed and maintained by Microsoft under the Apache 2 license. It is not directly run on the browser. It needs a compiler to compile and generate in JavaScript file. TypeScript source file is in ".ts" extension. We can use any valid ".js" file by renaming it to ".ts" file. TypeScript is the ES6 version of JavaScript with some additional features.

History of TypeScript
Anders Hejlsberg developed TypeScript. It was first introduced for the public in the month of 1st October 2012. After two years of internal development at Microsoft, the new version of TypeScript 0.9 was released in 2013. The current version of TypeScript is TypeScript 3.4.5 which was released on 24 April 2019.

AD
Advantage of TypeScript over JavaScript
TypeScript always highlights errors at compilation time during the time of development, whereas JavaScript points out errors at the runtime.
TypeScript supports strongly typed or static typing, whereas this is not in JavaScript.
TypeScript runs on any browser or JavaScript engine.
Great tooling supports with IntelliSense which provides active hints as the code is added.
It has a namespace concept by defining a module.
Disadvantage of TypeScript over JavaScript
TypeScript takes a long time to compile the code.
TypeScript does not support abstract classes.
If we run the TypeScript application in the browser, a compilation step is required to transform TypeScript into JavaScript.
AD
JavaScript vs TypeScripts
TypeScript Vs. JavaScript
SN	JavaScript	TypeScript
1.	It doesn't support strongly typed or static typing.	It supports strongly typed or static typing feature.
2.	Netscape developed it in 1995.	Anders Hejlsberg developed it in 2012.
3.	JavaScript source file is in ".js" extension.	TypeScript source file is in ".ts" extension.
4.	It is directly run on the browser.	It is not directly run on the browser.
5.	It is just a scripting language.	It supports object-oriented programming concept like classes, interfaces, inheritance, generics, etc.
6.	It doesn't support optional parameters.	It supports optional parameters.
7.	It is interpreted language that's why it highlighted the errors at runtime.	It compiles the code and highlighted errors during the development time.
8.	JavaScript doesn't support modules.	TypeScript gives support for modules.
9.	In this, number, string are the objects.	In this, number, string are the interface.
10.	JavaScript doesn't support generics.	TypeScript supports generics.
11.	Example:
<script>
function addNumbers(a, b) {  
    return a + b;  
}  
var sum = addNumbers(15, 25);  
document.write('Sum of the numbers is: ' + sum); 
</script>


Features of TypeScript
Features of TypeScript
Object-Oriented language: TypeScript provides a complete feature of an object-oriented programming language such as classes, interfaces, inheritance, modules, etc. In TypeScript, we can write code for both client-side as well as server-side development.

TypeScript supports JavaScript libraries: TypeScript supports each JavaScript elements. It allows the developers to use existing JavaScript code with the TypeScript. Here, we can use all of the JavaScript frameworks, tools, and other libraries easily.

JavaScript is TypeScript: It means the code written in JavaScript with valid .js extension can be converted to TypeScript by changing the extension from .js to .ts and compiled with other TypeScript files.

TypeScript is portable: TypeScript is portable because it can be executed on any browsers, devices, or any operating systems. It can be run in any environment where JavaScript runs on. It is not specific to any virtual-machine for execution.

DOM Manipulation: TypeScript can be used to manipulate the DOM for adding or removing elements similar to JavaScript.

TypeScript is just a JS: TypeScript code is not executed on any browsers directly. The program written in TypeScript always starts with JavaScript and ends with JavaScript. Hence, we only need to know JavaScript to use it in TypeScript. The code written in TypeScript is compiled and converted into its JavaScript equivalent for the execution. This process is known as Trans-piled. With the help of JavaScript code, browsers can read the TypeScript code and display the output.

Advantage of TypeScript over JavaScript
TypeScript always highlights errors at compilation time during the time of development, whereas JavaScript points out errors at the runtime.
TypeScript supports strongly typed or static typing, whereas this is not in JavaScript.
TypeScript runs on any browser or JavaScript engine.
Great tooling supports with IntelliSense, which provides active hints as the code is added.
It has a namespace concept by defining a module.
Disadvantage of TypeScript over JavaScript
TypeScript takes a long time to compile the code.
TypeScript does not support abstract classes.
If we run the TypeScript application in the browser, a compilation step is required to transform TypeScript into JavaScript.


Components of TypeScript
The TypeScript language is internally divided into three main layers. Each of these layers is divided into sublayers or components. In the following diagram, we can see the three layers and each of their internal components. These layers are:

Language
The TypeScript Compiler
The TypeScript Language Services
Components of TypeScript
1. Language
It features the TypeScript language elements. It comprises elements like syntax, keywords, and type annotations.

2. The TypeScript Compiler
The TypeScript compiler (TSC) transform the TypeScript program equivalent to its JavaScript code. It also performs the parsing, and type checking of our TypeScript code to JavaScript code.

Components of TypeScript
Browser doesn't support the execution of TypeScript code directly. So the program written in TypeScript must be re-written in JavaScript equivalent code which supports the execution of code in the browser directly. To perform this, TypeScript comes with TypeScript compiler named "tsc." The current version of TypeScript compiler supports ES6, by default. It compiles the source code in any module like ES6, SystemJS, AMD, etc.

We can install the TypeScript compiler by locally, globally, or both with any npm package. Once installation completes, we can compile the TypeScript file by running "tsc" command on the command line.

Example:
$ tsc helloworld.ts   // It compiles the TS file helloworld into the helloworld.js file.  
Compiler Configuration
The TypeScript compiler configuration is given in tsconfig.json file and looks like the following:

{  
  "compilerOptions": {  
    "declaration": true,  
    "emitDecoratorMetadata": false,  
    "experimentalDecorators": false,  
    "module": "none",  
    "moduleResolution": "node",  
    "noFallthroughCasesInSwitch": false,  
    "noImplicitAny": false,  
    "noImplicitReturns": false,  
    "removeComments": false,  
    "sourceMap": false,  
    "strictNullChecks": false,  
    "target": "es3"  
  },  
  "compileOnSave": true  
}  
Declaration file
When we compile the TypeScript source code, it gives an option to generate a declaration file with the extension .d.ts. This file works as an interface to the components in the compiled JavaScript. If a file has an extension .d.ts, then each root level definition must have the declare keyword prefixed to it. It makes clear that there will be no code emitted by TypeScript, which ensures that the declared item will exist at runtime. The declaration file provides IntelliSense for JavaScript libraries like jQuery.

AD
3. The TypeScript Language Services
The language service provides information which helps editors and other tools to give better assistance features such as automated refactoring and IntelliSense. It exposes an additional layer around the core-compiler pipeline. It supports some standard typical editor operations like code formatting and outlining, colorization, statement completion, signature help, etc.


TypeScript First Program
In this section, we are going to learn how we can write a program in TypeScript, how to compile it, and how to run it. Also, we will see how to compiles the program and shows the error, if any.

Let us write a program in the text editor, save it, compile it, run it, and display the output to the console. To do this, we need to perform the following steps.

Step-1 Open the Text Editor and write/copy the following code.

function greeter(person) {  
    return "Hello, " + person;  
}  
let user = 'JavaTpoint';  
console.log(greeter(user));  
Step-2 Save the above file as ".ts" extension.

Step-3 Compile the TypeScript code. To compile the source code, open the command prompt, and then goes to the file directory location where we saved the above file. For example, if we save the file on the desktop, go to the terminal window and type: - cd Desktop/folder_name. Now, type the following command tsc filename.ts for compilation and press Enter.

TypeScript First Program
It will generate JavaScript file with ".js" extension at the same location where the TypeScript source file exists. The below ".js" file is the output of TypeScript (.ts) file.

TypeScript First Program
NOTE: If we directly run ".ts" file on the web browser, it will throw an error message. But after the compilation of ".ts" file, we will get a ".js" file, which can be executed on any browser.
Step-4 Now, to run the above JavaScript file, type the following command in the terminal window: node filename.js and press Enter. It gives us the final output as:

TypeScript First Program
Compile-Time error
TypeScript always gives an error at compilation time. For this, we need to write the program in TypeScript, compile it, and see the error, if found.

Step 1 Open the Text Editor and write/copy the following code.

function addNumbers(a, b) {  
    return a + b;  
}  
var sum = addNumbers("JavaTpoint", 25);  
console.log('Sum of the numbers is: ' + sum);  
Step-2 Save the above file as ".ts" extension.

Step-3 Compile the TypeScript code. To compile the source code, open the command prompt, and then goes to the file directory location where we saved the above file. For example, if we save the file on the desktop, go to the terminal window and type: - cd Desktop/folder_name. Now, type the following command tsc filename.ts for compilation and press Enter.

This TypeScript source file will generate an error which can be shown in the following image.

TypeScript First Program
NOTE: This program gives an error because we were taking the variable "a" and "b" as of number type. But, we were passing the variable "a" as the string and variable "b" as the number.

TypeScript Type
The TypeScript language supports different types of values. It provides data types for the JavaScript to transform it into a strongly typed programing language. JavaScript doesn't support data types, but with the help of TypeScript, we can use the data types feature in JavaScript. TypeScript plays an important role when the object-oriented programmer wants to use the type feature in any scripting language or object-oriented programming language. The Type System checks the validity of the given values before the program uses them. It ensures that the code behaves as expected.

TypeScript provides data types as an optional Type System. We can classify the TypeScript data type as following.

TypeScript Type
1. Static Types
In the context of type systems, static types mean "at compile time" or "without running a program." In a statically typed language, variables, parameters, and objects have types that the compiler knows at compile time. The compiler used this information to perform the type checking.

Static types can be further divided into two sub-categories:

Built-in or Primitive Type
The TypeScript has five built-in data types, which are given below.

TypeScript Type
Number
Like JavaScript, all the numbers in TypeScript are stored as floating-point values. These numeric values are treated like a number data type. The numeric data type can be used to represents both integers and fractions. TypeScript also supports Binary(Base 2), Octal(Base 8), Decimal(Base 10), and Hexadecimal(Base 16) literals.

Syntax:

let identifier: number = value;  
Examples:-

let first: number = 12.0;             // number   
let second: number = 0x37CF;          // hexadecimal  
let third: number = 0o377 ;           // octal  
let fourth: number = 0b111001;        // binary   
  
console.log(first);           // 123  
console.log(second);          // 14287  
console.log(third);           // 255  
console.log(fourth);          // 57  
String
We will use the string data type to represents the text in TypeScript. String type work with textual data. We include string literals in our scripts by enclosing them in single or double quotation marks. It also represents a sequence of Unicode characters. It embedded the expressions in the form of $ {expr}.

Syntax

let identifier: string = " ";  
                Or   
let identifier: string = ' ';  
Examples

let empName: string = "Rohan";   
let empDept: string = "IT";   
  
// Before-ES6  
let output1: string = employeeName + " works in the " + employeeDept + " department.";   
  
// After-ES6  
let output2: string = `${empName} works in the ${empDept} department.`;   
  
console.log(output1);//Rohan works in the IT department.   
console.log(output2);//Rohan works in the IT department.  
Boolean
The string and numeric data types can have an unlimited number of different values, whereas the Boolean data type can have only two values. They are "true" and "false." A Boolean value is a truth value which specifies whether the condition is true or not.

Syntax

let identifier: BooleanBoolean = Boolean value;  
Examples

let isDone: boolean = false;  
Void
A void is a return type of the functions which do not return any type of value. It is used where no data type is available. A variable of type void is not useful because we can only assign undefined or null to them. An undefined data type denotes uninitialized variable, whereas null represents a variable whose value is undefined.

Syntax

let unusable: void = undefined;  
Examples
AD

1. function helloUser(): void {  
       alert("This is a welcome message");  
       }  
2. let tempNum: void = undefined;  
  tempNum = null;      
  tempNum = 123;      //Error  
Null
Null represents a variable whose value is undefined. Much like the void, it is not extremely useful on its own. The Null accepts the only one value, which is null. The Null keyword is used to define the Null type in TypeScript, but it is not useful because we can only assign a null value to it.

Examples

let num: number = null;  
let bool: boolean = null;   
let str: string = null;  
Undefined
The Undefined primitive type denotes all uninitialized variables in TypeScript and JavaScript. It has only one value, which is undefined. The undefined keyword defines the undefined type in TypeScript, but it is not useful because we can only assign an undefined value to it.

Example

let num: number = undefined;  
let bool: boolean = undefined;  
let str: string = undefined;  
Any Type
It is the "super type" of all data type in TypeScript. It is used to represents any JavaScript value. It allows us to opt-in and opt-out of type-checking during compilation. If a variable cannot be represented in any of the basic data types, then it can be declared using "Any" data type. Any type is useful when we do not know about the type of value (which might come from an API or 3rd party library), and we want to skip the type-checking on compile time.
AD

Syntax

let identifier: any = value;  
Examples

1. let val: any = 'Hi';  
      val = 555;   // OK  
      val = true;   // OK           
2. function ProcessData(x: any, y: any) {  
                       return x + y;  
            }  
            let result: any;  
            result = ProcessData("Hello ", "Any!"); //Hello Any!  
            result = ProcessData(2, 3); //5  
User-Defined DataType
TypeScript supports the following user-defined data types:

TypeScript Type
Array
An array is a collection of elements of the same data type. Like JavaScript, TypeScript also allows us to work with arrays of values. An array can be written in two ways:

1. Use the type of the elements followed by [] to denote an array of that element type:

var list : number[] = [1, 3, 5];  
2. The second way uses a generic array type:

var list : Array<number> = [1, 3, 5];  
Touple
The Tuple is a data type which includes two sets of values of different data types. It allows us to express an array where the type of a fixed number of elements is known, but they are not the same. For example, if we want to represent a value as a pair of a number and a string, then it can be written as:

// Declare a tuple  
let a: [string, number];  
  
// Initialize it  
a = ["hi", 8, "how", 5]; // OK  
Interface
An Interface is a structure which acts as a contract in our application. It defines the syntax for classes to follow, means a class which implements an interface is bound to implement all its members. It cannot be instantiated but can be referenced by the class which implements it. The TypeScript compiler uses interface for type-checking that is also known as "duck typing" or "structural subtyping."

Example

interface Calc {  
    subtract (first: number, second: number): any;  
}  
   
let Calculator: Calc = {  
    subtract(first: number, second: number) {  
        return first - second;  
    }  
}  
Class
Classes are used to create reusable components and acts as a template for creating objects. It is a logical entity which store variables and functions to perform operations. TypeScript gets support for classes from ES6. It is different from the interface which has an implementation inside it, whereas an interface does not have any implementation inside it.

Example

class Student  
{  
    RollNo: number;  
    Name: string;   
    constructor(_RollNo: number, Name: string)   
    {  
        this.RollNo = _rollNo;  
        this.Name = _name;  
    }  
    showDetails()  
    {  
        console.log(this.rollNo + " : " + this.name);  
    }  
}  
Enums
Enums define a set of named constant. TypeScript provides both string-based and numeric-based enums. By default, enums begin numbering their elements starting from 0, but we can also change this by manually setting the value to one of its elements. TypeScript gets support for enums from ES6.

Example

enum Color {  
        Red, Green, Blue  
};  
let c: Color;  
ColorColor = Color.Green;  
Functions
A function is the logical blocks of code to organize the program. Like JavaScript, TypeScript can also be used to create functions either as a named function or as an anonymous function. Functions ensure that our program is readable, maintainable, and reusable. A function declaration has a function's name, return type, and parameters.

Example

//named function with number as parameters type and return type  
function add(a: number, b: number): number {  
            return a + b;  
}  
  
//anonymous function with number as parameters type and return type  
let sum = function (a: number, y: number): number {  
            return a + b;  
};  
AD
2. Generic
Generic is used to create a component which can work with a variety of data type rather than a single one. It allows a way to create reusable components. It ensures that the program is flexible as well as scalable in the long term. TypeScript uses generics with the type variable <T> that denotes types. The type of generic functions is just like non-generic functions, with the type parameters listed first, similarly to function declarations.

Example

function identity<T>(arg: T): T {  
    return arg;  
}  
let output1 = identity<string>("myString");  
let output2 = identity<number>( 100 );  
3. Decorators
A decorator is a special of data type which can be attached to a class declaration, method, property, accessor, and parameter. It provides a way to add both annotations and a meta-programing syntax for classes and functions. It is used with "@" symbol.

A decorator is an experimental feature which may change in future releases. To enable support for the decorator, we must enable the experimentalDecorators compiler option either on the command line or in our tsconfig.json.

Example

function f() {  
    console.log("f(): evaluated");  
    return function (target, propertyKey: string, descriptor: PropertyDescriptor) {  
        console.log("f(): called");  
    }  
}  
  
class C {  
    @f()  
    method() {}  
}  


Difference between Null and Undefined
Null
Null is used to represent an intentional absence of value. It represents a variable whose value is undefined. It accepts only one value, which is null. The Null keyword is used to define the Null type in TypeScript, but it is not useful because we can only assign a null value to it.

Example
//Variable declared and assigned to null  
var a = null;   
console.log( a );   //output: null  
console.log( typeof(a) );   //output: object  
Output:

Null vs Undefined
Undefined
It represents uninitialized variables in TypeScript and JavaScript. It has only one value, which is undefined. The undefined keyword defines the undefined type in TypeScript, but it is not useful because we can only assign an undefined value to it.

Example
//Variable declaration without assigning any value to it  
var a;        
console.log(a);  //undefined  
console.log(typeof(a));  //undefined  
console.log(undeclaredVar);  //Uncaught ReferenceError: undeclaredVar is not defined  
Output:

Null vs Undefined
AD
Null vs. Undefined
The important difference between Null and Undefined are:

SN	Null	Undefined
1.	It is an assignment value. It can be assigned to a variable which indicates that a variable does not point any object.	It is not an assignment value. It means a variable has been declared but has not yet been assigned a value.
2.	It is an object.	It is a type itself.
3.	The null value is a primitive value which represents the null, empty, or non-existent reference.	The undefined value is a primitive value, which is used when a variable has not been assigned a value.
4.	Null indicates the absence of a value for a variable.	Undefined indicates the absence of the variable itself.
5.	Null is converted to zero (0) while performing primitive operations.	Undefined is converted to NaN while performing primitive operations.

TypeScript Variables
A variable is the storage location, which is used to store value/information to be referenced and used by programs. It acts as a container for value in code and must be declared before the use. We can declare a variable by using the var keyword. In TypeScript, the variable follows the same naming rule as of JavaScript variable declaration. These rules are-

The variable name must be an alphabet or numeric digits.
The variable name cannot start with digits.
The variable name cannot contain spaces and special character, except the underscore(_) and the dollar($) sign.
In ES6, we can define variables using let and const keyword. These variables have similar syntax for variable declaration and initialization but differ in scope and usage. In TypeScript, there is always recommended to define a variable using let keyword because it provides the type safety.

The let keyword is similar to var keyword in some respects, and const is an let which prevents prevents re-assignment to a variable.

Variable Declaration
We can declare a variable in one of the four ways:

1. Declare type and value in a single statement

var [identifier] : [type-annotation] = value;  
2. Declare type without value. Then the variable will be set to undefined.

var [identifier] : [type-annotation];  
3. Declare its value without type. Then the variable will be set to any.

var [identifier] = value;  
4. Declare without value and type. Then the variable will be set to any and initialized with undefined.

var [identifier];  
Let's understand all the three variable keywords one by one.

var keyword
Generally, var keyword is used to declare a variable in JavaScript.

var x = 50;  
We can also declare a variable inside the function:

function a() {  
     var msg = " Welcome to JavaTpoint !! ";  
     return msg;  
}  
a();  
We can also access a variable of one function with the other function:

function a() {  
    var x = 50;  
    return function b() {  
         var y = x+5;  
         return y;  
    }  
}  
var  b = a();  
b();       //returns '55'  
Scoping rules
For other language programmers, they are getting some odd scoping rules for var declaration in JavaScript. Variables declared in TypeScript with the var keyword have function scope. This variable has global scope in the function where they are declared. It can also be accessed by any function which shares the same scope.

Example

function f()  
{  
    var X = 5; //Available globally inside f()  
    if(true)  
    {  
        var Y = 10; //Available globally inside f()   
        console.log(X); //Output 5  
        console.log(Y); //Output 10  
    }      
    console.log(X); //Output 5  
    console.log(Y); //Output 10  
}  
f();  
console.log(X); //Returns undefined because value cannot accesses from outside function  
console.log(Y); //Returns undefined because value cannot accesses from outside function  
NOTE: As var declarations are accessible anywhere within their containing module, function, global scope, or namespace, some people call this var-scoping or function-scoping. Parameters are also called function scoped.
AD
let declarations
The let keyword is similar to the var keyword. The var declaration has some problems in solving programs, so ES6 introduced let keyword to declare a variable in TypeSript and JavaScript. The let keyword has some restriction in scoping in comparison of the var keyword.

The let keyword can enhance our code readability and decreases the chance of programming error.

The let statement are written as same syntax as the var statement:
AD

var declaration: var b = 50;  
let declaration: let b = 50;  
The key difference between var and let is not in the syntax, but it differs in the semantics. The Variable declared with the let keyword are scoped to the nearest enclosing block which can be smaller than a function block.

Block Scoping
When the variable declared using the let keyword, it uses block scoping or lexical scoping. Unlike variable declared using var keyword whose scopes leak out to their containing function, a block-scoped variable cannot visible outside of its containing block.

function f(input: boolean) {  
    let x = 100;  
    if (input) {  
        // "x" exists here        
        let y = x + 1;  
        return y;  
    }  
    // Error: "y" doesn't exist here  
    return y;  
}  
Here, we have two local variables x and y. Scope of x is limited to the body of the function f() while the scope of y is limited to the containing if statement's block.

NOTE- The variables declared in a try-catch clause also have similar scoping rules. For example:
try {  
    throw "Hi!!";  
}catch (e) {  
    console.log("Hello");  
}  
// 'e' doesn't exist here, so error will found  
console.log(e);  
Re-declaration and Shadowing
With the var declaration, it did not matter how many time's we declared variables. We just got only one. In the below example, all declarations of x refer to the same x, which is perfectly valid. But there is some bug, which can be found by the let declaration.

Example without let keyword:
AD

function f(a) {  
    var a;  
    var a;  
    if (true) {  
        var a;  
    }  
}  
Example with let keyword:

let a = 10;  
let a = 20; // it gives error: can't re-declare 'a' in the same scope  
Shadowing is the act of introducing a new name in a more nested scope. It declares an identifier which has already been declared in an outer scope. This is not incorrect, but there might be confusion. It will make the outer identifier unavailable inside the loop where the loop variable is shadowing it. It can introduce bugs on its own in the event of accidental Shadowing, as well as it also helps in preventing the application from certain bugs.

Example

var currencySymbol = "$";  
function showMoney(amount) {  
  var currencySymbol = "€";  
  document.write(currencySymbol + amount);  
}  
showMoney("100");  
The above example has a global variable name that shares the same name as the inner method. The inner variable used only in that function, and all other functions will use the global variable declaration.

The Shadowing is usually avoided in writing of clearer code. While in some scenarios, if there may be fitting to take advantage of it, we should use it with the best judgment.

Hoisting
Hoisting of var
Hoisting is a mechanism of JavaScript. In hoisting, variables and function declarations are moved to the top of their enclosing scope before code execution. We can understand it with the following example.

Note: Hoisting does not happen if we initialize the variable.
Example

function get(x){     
  console.log(a);  //printing x variable. Value is undefined       
  //declared variable after console hoisted to the top at run time    
  var a = x;        
  //again printing x variable. Value is 3.  
  console.log(a);    
}    
get(4);  
Output:

undefined
4
Hoisting of let
A variable declared with let keyword is not hoisted. If we try to use a let variable before it is declared, then it will result in a ReferenceError.

Example

{  
  //program doesn't know about variable b so it will give me an error.  
  console.log(b); // ReferenceError: b is not defined  
  let b = 3;  
}  
const declarations
The const declaration is used to declare permanent value, which cannot be changed later. It has a fixed value. The const declaration follows the same scoping rules as let declaration, but we cannot re-assign any new value to it.

Note: According to the naming standards, the const variable must be declared in capital letters. Naming standards should be followed to maintain the code for the long run.
Example

function constTest(){  
  const VAR = 10;  
  console.log("Value is: " +VAR);  
}  
constTest();  
Output:

Value is: 10
What will happen when we try to re-assign the const variable?
If we try to re-assign the existing const variable in a code, the code will throw an error. So, we cannot re-assign any new value to an existing const variable.

Example

function constTest(){  
  const VAR = 10;  
  console.log("Output: " +VAR);  // Output: 10  
  const VAR = 10;  
  console.log("Output: " +VAR);  //Uncaught TypeError: Assignment to constant variable  
}  
constTest();  
Output:

SyntaxError: Identifier 'VAR' has already been declared.

Difference between let and var keyword
var keyword
The var statement is used to declare a variable in JavaScript. A variable declared with the var keyword is defined throughout the program.

Example
var greeter = "hey hi";  
var times = 5;  
if (times > 3) {  
   var greeter = "Say Hello JavaTpoint";   
}  
console.log(greeter) //Output: Say Hello JavaTpoint  
Output:

let and var keyword
let keyword
The let statement is used to declare a local variable in TypeScript. It is similar to the var keyword, but it has some restriction in scoping in comparison of the var keyword. The let keyword can enhance our code readability and decreases the chance of programming error. A variable declared with the let keyword is limited to the block-scoped only.

Note: The key difference between var and let is not in the syntax, but it differs in the semantics.
Example
let greeter = "hey hi";  
let times = 5;  
if (times > 3) {  
   let hello = "Say Hello JavaTpoint";   
   console.log(hello) // Output: Say Hello JavaTpoint  
}  
console.log(hello) // Compile error: greeter is not defined  
Output:

The above code snippet throws an error because the variable "hello" is not defined globally.

let and var keyword
AD
Var vs. Let Keyword
SN	var	let
1.	The var keyword was introduced with JavaScript.	The let keyword was added in ES6 (ES 2015) version of JavaScript.
2.	It has global scope.	It is limited to block scope.
3.	It can be declared globally and can be accessed globally.	It can be declared globally but cannot be accessed globally.
4.	Variable declared with var keyword can be re-declared and updated in the same scope.
Example:
function varGreeter(){
  var a = 10;        
  var a = 20; //a is replaced
  console.log(a);
}
varGreeter();
Variable declared with let keyword can be updated but not re-declared.
Example:
function varGreeter(){
  let a = 10;        
 let a = 20; //SyntaxError: 
 //Identifier 'a' has already been declared
  console.log(a);
}
varGreeter();
5.	It is hoisted.
Example:
{
  console.log(c); // undefined. 
  //Due to hoisting
  var c = 2;
}
It is not hoisted.
Example:
{
  console.log(b); // ReferenceError: 
  //b is not defined
  let b = 3;
}


TypeScript Operators
An Operator is a symbol which operates on a value or data. It represents a specific action on working with data. The data on which operators operates is called operand. It can be used with one or more than one values to produce a single value. All of the standard JavaScript operators are available with the TypeScript program.

Example
10 + 10 = 20;  
In the above example, the values '10' and '20' are known as an operand, whereas '+' and '=' are known as operators.

Operators in TypeScript
In TypeScript, an operator can be classified into the following ways.

Arithmetic operators
Comparison (Relational) operators
Logical operators
Bitwise operators
Assignment operators
Ternary/conditional operator
Concatenation operator
Type Operator
Arithmetic Operators
Arithmetic operators take numeric values as their operands, performs an action, and then returns a single numeric value. The most common arithmetic operators are addition(+), subtraction(-), multiplication(*), and division(/).

Operator	Operator_Name	Description	Example
+	Addition	It returns an addition of the values.	
let a = 20;
let b = 30;
let c = a + b;
console.log( c ); //
Output
30
-	Subtraction	It returns the difference of the values.	
let a = 30;
let b = 20;
let c = a - b;
console.log( c ); //
Output
10
*	Multiplication	It returns the product of the values.	
let a = 30;
let b = 20;
let c = a * b;
console.log( c ); //
Output
600
/	Division	It performs the division operation, and returns the quotient.	
let a = 100;
let b = 20;
let c = a / b;
console.log( c ); //
Output
5
%	Modulus	It performs the division operation and returns the remainder.	
let a = 95;
let b = 20;
let c = a % b;
console.log( c ); //
Output
15
++	Increment	It is used to increments the value of the variable by one.	
let a = 55;
a++;
console.log( a ); //
Output
56
--	Decrement	It is used to decrements the value of the variable by one.	
let a = 55;
a--;
console.log( a ); //
Output
54
AD
Comparison (Relational) Operators
The comparison operators are used to compares the two operands. These operators return a Boolean value true or false. The important comparison operators are given below.

Operator	Operator_Name	Description	Example
==	Is equal to	It checks whether the values of the two operands are equal or not.	
let a = 10;
let b = 20;
console.log(a==b);     //false
console.log(a==10);    //true
console.log(10=='10'); //true
===	Identical(equal and of the same type)	It checks whether the type and values of the two operands are equal or not.	
let a = 10;
let b = 20;
console.log(a===b);    //false
console.log(a===10);   //true
console.log(10==='10'); //false
!=	Not equal to	It checks whether the values of the two operands are equal or not.	
let a = 10;
let b = 20;
console.log(a!=b);     //true
console.log(a!=10);    //false
console.log(10!='10'); //false
!==	Not identical	It checks whether the type and values of the two operands are equal or not.	
let a = 10;
let b = 20;
console.log(a!==b);     //true
console.log(a!==10);    /false
console.log(10!=='10'); //true
>	Greater than	It checks whether the value of the left operands is greater than the value of the right operand or not.	
let a = 30;
let b = 20;
console.log(a>b);     //true
console.log(a>30);    //false
console.log(20> 20'); //false
>=	Greater than or equal to	It checks whether the value of the left operands is greater than or equal to the value of the right operand or not.	
let a = 20;
let b = 20;
console.log(a>=b);     //true
console.log(a>=30);    //false
console.log(20>='20'); //true
<	Less than	It checks whether the value of the left operands is less than the value of the right operand or not.	
let a = 10;
let b = 20;
console.log(a<b);      //true
console.log(a<10);     //false
console.log(10<'10');  //false
<=	Less than or equal to	It checks whether the value of the left operands is less than or equal to the value of the right operand or not.	
let a = 10;
let b = 20;
console.log(a<=b);     //true
console.log(a<=10);    //true
console.log(10<='10'); //true
Logical Operators
Logical operators are used for combining two or more condition into a single expression and return the Boolean result true or false. The Logical operators are given below.

Operator	Operator_Name	Description	Example
&&	Logical AND	It returns true if both the operands(expression) are true, otherwise returns false.	
let a = false;
let b = true;
console.log(a&&b);      /false
console.log(b&&true);   //true
console.log(b&&10);     //10 which is also 'true'
console.log(a&&'10');  //false
||	Logical OR	It returns true if any of the operands(expression) are true, otherwise returns false.	
let a = false;
let b = true;
console.log(a||b);      //true
console.log(b||true);   //true
console.log(b||10);     //true
console.log(a||'10');   //'10' which is also 'true'
!	Logical NOT	It returns the inverse result of an operand(expression).	
let a = 20;
let b = 30;
console.log(!true);    //false
console.log(!false);   //true
console.log(!a);       //false
console.log(!b);       /false
console.log(!null);    //true
Bitwise Operators
The bitwise operators perform the bitwise operations on operands. The bitwise operators are as follows.

Operator	Operator_Name	Description	Example
&	Bitwise AND	It returns the result of a Boolean AND operation on each bit of its integer arguments.	
let a = 2;
let b = 3;
let c = a & b;
console.log(c);   //
Output
2
|	Bitwise OR	It returns the result of a Boolean OR operation on each bit of its integer arguments.	
let a = 2;
let b = 3;
let c = a | b;
console.log(c);   //
Output 
3
^	Bitwise XOR	It returns the result of a Boolean Exclusive OR operation on each bit of its integer arguments.	
let a = 2;
let b = 3;
let c = a ^ b;
console.log(c);   //
Output 
1
~	Bitwise NOT	It inverts each bit in the operands.	
let a = 2;
let c = ~ a;
console.log(c);   //
Output 
-3
>>	Bitwise Right Shift	The left operand's value is moved to the right by the number of bits specified in the right operand.	
let a = 2;
let b = 3;
let c = a >> b;
console.log(c);   //
Output 
0
<<	Bitwise Left Shift	The left operand's value is moved to the left by the number of bits specified in the right operand. New bits are filled with zeroes on the right side.	
let a = 2;
let b = 3;
let c = a << b;
console.log(c);   //
Output 
16
>>>	Bitwise Right Shift with Zero	The left operand's value is moved to the right by the number of bits specified in the right operand and zeroes are added on the left side.	
let a = 3;
let b = 4;
let c = a >>> b;
console.log(c);   //
Output 
0
AD
Assignment Operators
Assignment operators are used to assign a value to the variable. The left side of the assignment operator is called a variable, and the right side of the assignment operator is called a value. The data-type of the variable and value must be the same otherwise the compiler will throw an error. The assignment operators are as follows.

Operator	Operator_Name	Description	Example
=	Assign	It assigns values from right side to left side operand.	
let a = 10;
let b = 5;
console.log("a=b:" +a);   //
Output 
10
+=	Add and assign	It adds the left operand with the right operand and assigns the result to the left side operand.	
let a = 10;
let b = 5;
let c = a += b;
console.log(c);   //
Output 
15
-=	Subtract and assign	It subtracts the right operand from the left operand and assigns the result to the left side operand.	
let a = 10;
let b = 5;
let c = a -= b;
console.log(c);   //
Output 
5
*=	Multiply and assign	It multiplies the left operand with the right operand and assigns the result to the left side operand.	
let a = 10;
let b = 5;
let c = a *= b;
console.log(c);   //
Output 
50
/=	Divide and assign	It divides the left operand with the right operand and assigns the result to the left side operand.	
let a = 10;
let b = 5;
let c = a /= b;
console.log(c);   //
Output 
2
%=	Modulus and assign	It divides the left operand with the right operand and assigns the result to the left side operand.	
let a = 16;
let b = 5;
let c = a %= b;
console.log(c);   //
Output 
1
AD
Ternary/Conditional Operator
The conditional operator takes three operands and returns a Boolean value based on the condition, whether it is true or false. Its working is similar to an if-else statement. The conditional operator has right-to-left associativity. The syntax of a conditional operator is given below.

expression ? expression-1 : expression-2;  
expression: It refers to the conditional expression.
expression-1: If the condition is true, expression-1 will be returned.
expression-2: If the condition is false, expression-2 will be returned.
AD
Example
let num = 16;  
let result = (num > 0) ? "True":"False"   
console.log(result);  
Output:

True
AD
Concatenation Operator
The concatenation (+) operator is an operator which is used to append the two string. In concatenation operation, we cannot add a space between the strings. We can concatenate multiple strings in a single statement. The following example helps us to understand the concatenation operator in TypeScript.

Example
let message = "Welcome to " + "JavaTpoint";  
console.log("Result of String Operator: " +message);  
Output:

Result of String Operator: Welcome to JavaTpoint
Type Operators
There are a collection of operators available which can assist you when working with objects in TypeScript. Operators such as typeof, instanceof, in, and delete are the examples of Type operator. The detail explanation of these operators is given below.

Operator_Name	Description	Example
in	It is used to check for the existence of a property on an object.	
let Bike = {make: 'Honda', model: 'CLIQ', year: 2018};
console.log('make' in Bike);   // 
Output:
true
delete	It is used to delete the properties from the objects.	
let Bike = { Company1: 'Honda',
             Company2: 'Hero',
             Company3: 'Royal Enfield'
           };
delete Bike.Company1;
console.log(Bike);   // 
Output:
{ Company2: 'Hero', Company3: 'Royal Enfield' }
typeof	It returns the data type of the operand.	
let message = "Welcome to " + "JavaTpoint";
console.log(typeof message);  // 
Output:
String
instanceof	It is used to check if the object is of a specified type or not.	
let arr = [1, 2, 3];
console.log( arr instanceof Array ); // true
console.log( arr instanceof String ); // false


TypeScript Type Annotation
We know that JavaScript is not a typed language so we cannot specify the type of a variable such as a number, string, Boolean in JavaScript. However, in TypeScript, we can specify the type of variables, function parameters, and object properties because TypeScript is a typed language.

Type Annotations are annotations which can be placed anywhere when we use a type. The use of Type annotation is not mandatory in TypeScript. It helps the compiler in checking the types of variable and avoid errors when dealing with the data types.

We can specify the type by using a colon(: Type) after a variable name, parameter, or property. There can be a space between the colon and variable name, parameter, or property. TypeScript includes all the primitive data types of JavaScript such as number, string, Boolean, etc.

Syntax
var variableName: TypeAnnotation = value;  
The following example demonstrates type annotations for variables with different data types.

var age: number = 44;          // number variable  
var name: string = "Rahul";     // string variable  
var isUpdated: boolean = true; // Boolean variable   
In the above example, the variables are declared with their data type. These examples demonstrate type annotations. Here, we cannot change the value by using a different data type with the available data type. If we try to do this, TypeScript compiler will throw an error. For example, if we assign a string to a variable age or number to the name, then it will give a compilation error.

Use of Type Annotation as a parameter

The below example demonstrates the type annotation with parameters.

Example
function display(id:number, name:string)  
{  
    console.log("Id = " + id + ", Name = " + name);  
}  
display(101, "Rohit Sharma");  
Output:

Id = 101, Name = Rohit Sharma
Inline Type Annotation
In TypeScript, inline type annotations allow us to declare an object for each of the properties of the object.

Syntax
:{ /*Structure*/ }  
Example
var student : {   
    id: number;   
    name: string;   
};   
  
student = {   
  id: 100,   
  name : "John"  
}  
Here, we declare an object student with two properties "id" and "name" with the data type number and string, respectively. If we try to assign a string value to id, the TypeScript compiler will throw an error: Type of property are incompatible.

TypeScript Type Inference
In TypeScript, it is not necessary to annotate type always. The TypeScript compiler infers the type information when there is no explicit information available in the form of type annotations.

In TypeScript, TypeScript compiler infers the type information when:

Variables and members are initialized
Setting default values for parameters
Determined function return types
For example

let x = 3;  
In the above, the type of the variable "x" infers in a number. The type inference takes place when initializing variables and members, setting parameter default values, and determining function return types.

Let us take another example.

var x = "JavaTpoint";  
var y = 501;  
x = y; // Compile-time Error: Type 'number' is not assignable to type 'string'  
In the above example, we get an error because while inferring types, TypeScript inferred the type of variable "x" as a string and variable "y" as a number. When we try to assign y to x, the compiler generates an error that a number type is not assignable to a string type.

Best Common Type: Type Inference
Type inference is helpful in type-checking when there are no explicit type annotation is available. In type inference, there can be a situation where an object may be initialized with multiple types.

For example

let arr = [ 10, 20, null, 40 ];  
In the above example, we have an array with values 10, 20, null, and, 30. Here, we have given two choices for the type of an array: number and null. The best common type algorithm picks the one which is compatible with all types, i.e., number and null.

Let us take another example.

let arr2 = [ 10, 20, "JavaTpoint" ];  
In the above example, the array contains values of type number and string both. Now, the TypeScript compiler uses the most common type algorithm and picks the one which is compatible with all types. In such cases, the compiler treats the type as a union of all types in the array. Here, the type would be (string or number), which means that the array can hold either string values or numeric values.

The return type of a function is also inferred by the returning value. For example:

function sum(x: number, y: number )  
{  
    return x + y;      
}  
let Addition: number = sum(10,20); // Correct  
let str: string = sum(10,20); // Compiler Error   
In the above example, the return type of the function sum is number. So, its result will be stored in a number type variable, not a string type variable.

ypeScript Type Assertion
In TypeScript, type assertion is a mechanism which tells the compiler about the type of a variable. When TypeScript determines that the assignment is invalid, then we have an option to override the type using a type assertion. If we use a type assertion, the assignment is always valid, so we need to be sure that we are right. Otherwise, our program may not work correctly.

Type assertion is explicitly telling the compiler that we want to treat the entity as a different type. It allows us to treat any as a number, or number as a string. Type assertion is commonly used when we are migrating over code from JavaScript to TypeScript.

Type assertion works like typecasting, but it does not perform type checking or restructuring of data just like other languages can do like C# and Java. The typecasting comes with runtime support, whereas type assertion has no impact on runtime. However, type assertions are purely a compile-time construct and provide hints to the compiler on how we want our code to be analyzed.

Example
let empCode: any = 111;   
let employeeCode = <number> code;   
console.log(typeof(employeeCode)); //Output: number  
In the above example, we have declared a variable empCode as of type any. In the next line, we assign a value of this variable to another variable named employeeCode. Here, we know that empCode is of type number, even though we declared it as 'any.' when we are assigning empCode to employeeCode, we have asserted that empCode is of type number. Now the type of employeeCode is number.

TypeScript provides two ways to do Type Assertion. They are

Using Angular Bracket <>
Using as keyword
Using Angular Bracket <>
In TypeScript, we can use angular "bracket <>" to show Type Assertion.

Example

let empCode: any = 111;   
let employeeCode = <number> code;   
Using as Keyword
TypeScript provides another way to show Type Assertion by using "as" keyword.

Example

let empCode: any = 111;   
let employeeCode = code as number;   
Type Assertion with object
Sometimes, we might have a situation where we have an object which is declared without any properties. For this, the compiler gives an error. But, by using type assertion we can avoid this situation. We can understand it with the following example.

Example

let student = { };  
student.name = "Rohit"; //Compiler Error: Property 'name' doesn?t exist on type '{}'  
student.code = 123; //Compiler Error: Property 'code' doesn?t exist on type '{}'  
In the above example, we will get a compilation error, because the compiler assumes that the type of student is {} without properties. We can avoid this situation by using a type assertion, which can be shown below.

interface Student {   
    name: string;   
    code: number;   
}  
let student = <Student> { };   
student.name = "Rohit"; // Correct  
student.code = 123; // Correct  
In the above example, we have created an interface Student with the properties name and code. Then, we used type assertion on the student, which is the correct way to use type assertion.


---------------------------------

Compiling TypeScript code
The tsc compilation command comes with typescript, which can be used to compile code. tsc my-code.ts
This creates a my-code.js file.
Compile using tsconfig.js
   
   
  
  
You can also provide compilation options that travel with your code via a tsconfig.json file. To start a new TypeScript project, cd into your project's root directory in a terminal window and run tsc --init. This command will generate a tsconfig.json file with minimal configuration options, similar to below.
     {
    "compilerOptions": {
"module": "commonjs", "target": "es5", "noImplicitAny": false, "sourceMap": false, "pretty": true
    },
    "exclude": [
        "node_modules"
] }
With a tsconfig.json file placed at the root of your TypeScript project, you can use the tsc command to run the compilation.
Section 1.2: Basic syntax
TypeScript is a typed superset of JavaScript, which means that all JavaScript code is valid TypeScript code. TypeScript adds a lot of new features on top of that.
TypeScript makes JavaScript more like a strongly-typed, object-oriented language akin to C# and Java. This means that TypeScript code tends to be easier to use for large projects and that code tends to be easier to understand and maintain. The strong typing also means that the language can (and is) precompiled and that variables cannot be assigned values that are out of their declared range. For instance, when a TypeScript variable is declared as a number, you cannot assign a text value to it.
This strong typing and object orientation makes TypeScript easier to debug and maintain, and those were two of the weakest points of standard JavaScript.
Type declarations
You can add type declarations to variables, function parameters and function return types. The type is written after a colon following the variable name, like this: var num: number = 5; The compiler will then check the types (where possible) during compilation and report type errors.
The basic types are :
number (both integers and floating point numbers)
string
boolean
Array. You can specify the types of an array's elements. There are two equivalent ways to define array types: Array<T> and T[]. For example:
number[] - array of numbers
Array<string> - array of strings
Tuples. Tuples have a fixed number of elements with specific types.
[boolean, string] - tuple where the first element is a boolean and the second is a string. [number, number, number] - tuple of three numbers.
   var num: number = 5;
num = "this is a string"; // error: Type 'string' is not assignable to type 'number'.
      

{} - object, you can define its properties or indexer
{name:
string, age: number} - object with name and age attributes
  string]: number} - a dictionary of numbers indexed by string
{[key:
enum - { Red = 0, Blue, Green } - enumeration mapped to numbers Function. You specify types for the parameters and return value:
   (param: number) => string - function taking one number parameter returning string
() => number - function with no parameters returning an number.
(a: string, b?: boolean) => void - function taking a string and optionally a boolean with no return value.
any - Permits any type. Expressions involving any are not type checked.
void - represents "nothing", can be used as a function return value. Only null and undefined are part of the void type.
never
let foo: never; -As the type of variables under type guards that are never true.
function error(message: string): never { throw new Error(message); } - As the return type of functions that never return.
null - type for the value null. null is implicitly part of every type, unless strict null checks are enabled. Casting
You can perform explicit casting through angle brackets, for instance:
This example shows a derived class which is treated by the compiler as a MyInterface. Without the casting on the second line the compiler would throw an exception as it does not understand someSpecificMethod(), but casting through <ImplementingClass>derived suggests the compiler what to do.
Another way of casting in TypeScript is using the as keyword:
Since TypeScript 1.6, the default is using the as keyword, because using <> is ambiguous in .jsx files. This is mentioned in TypeScript official documentation.
Classes
Classes can be defined and used in TypeScript code. To learn more about classes, see the Classes documentation page.
Section 1.3: Hello World
              var derived: MyInterface; (<ImplementingClass>derived).someSpecificMethod();
      var derived: MyInterface;
(derived as ImplementingClass).someSpecificMethod();
  class Greeter {
    greeting: string;
constructor(message: string) { this.greeting = message;
    }
    greet(): string {
return this.greeting; }
}

 let greeter = new Greeter("Hello, world!"); console.log(greeter.greet());
Here we have a class, Greeter, that has a constructor and a greet method. We can construct an instance of the class using the new keyword and pass in a string we want the greet method to output to the console. The instance of our Greeter class is stored in the greeter variable which we then us to call the greet method.
Section 1.4: Running TypeScript using ts-node
ts-node is an npm package which allows the user to run typescript files directly, without the need for precompilation using tsc. It also provides REPL.
Install ts-node globally using
npm install -g ts-node
ts-node does not bundle typescript compiler, so you might need to install it.
npm install -g typescript Executing script
To execute a script named main.ts, run ts- ts
Example usage
Running REPL
To run REPL run command ts-node Example usage
To exit REPL use command .exit or press CTRL+C twice.
Section 1.5: TypeScript REPL in Node.js
For use TypeScript REPL in Node.js you can use tsun package Install it globally with
         node main.
 // main.ts
console.log("Hello world");
 $ ts-node main.ts
Hello world
 $ ts-node
> const sum = (a, b): number => a + b; undefined
> sum(2, 2)
4
> .exit

 npm install -g tsun
and run in your terminal or command prompt with tsun command
Usage example:
 $ tsun
TSUN : TypeScript Upgraded Node
type in TypeScript expression to evaluate type :help for commands in repl
$ function multiply(x, y) {
..return x * y;
..}
undefined
$ multiply(3, 4)
1

Chapter 2: Why and when to use TypeScript
If you find the arguments for type systems persuasive in general, then you'll be happy with TypeScript.
It brings many of the advantages of type system (safety, readability, improved tooling) to the JavaScript ecosystem. It also suffers from some of the drawbacks of type systems (added complexity and incompleteness).
Section 2.1: Safety
TypeScript catches type errors early through static analysis:
Section 2.2: Readability
TypeScript enables editors to provide contextual documentation:
You'll never forget whether String.prototype.slice takes (start, stop) or (start, length) again! Section 2.3: Tooling
TypeScript allows editors to perform automated refactors which are aware of the rules of the languages.
Here, for instance, Visual Studio Code is able to rename references to the inner foo without altering the outer foo. This would be difficult to do with a simple find/replace.
 function double(x: number): number { return 2 * x;
}
double('2');
// ~~~ Argument of type '"2"' is not assignable to parameter of type 'number'.
  

Chapter 3: TypeScript Core Types Section 3.1: String Literal Types
String literal types allow you to specify the exact value a string can have.
Any other string will give an error.
Together with Type Aliases and Union Types you get a enum-like behavior.
 let myFavoritePet: "dog"; myFavoritePet = "dog";
 // Error: Type '"rock"' is not assignable to type '"dog"'.
// myFavoritePet = "rock";
 type Species = "cat" | "dog" | "bird";
function buyPet(pet: Species, name: string) : Pet { /*...*/ } buyPet(myFavoritePet /* "dog" as defined above */, "Rocky");
// Error: Argument of type '"rock"' is not assignable to parameter of type "'cat' | "dog" | "bird".
Type '"rock"' is not assignable to type '"bird"'.
// buyPet("rock", "Rocky");
String Literal Types can be used to distinguish overloads.
 function buyPet(pet: Species, name: string) : Pet;
function buyPet(pet: "cat", name: string): Cat;
function buyPet(pet: "dog", name: string): Dog;
function buyPet(pet: "bird", name: string): Bird;
function buyPet(pet: Species, name: string) : Pet { /*...*/ }
let dog = buyPet(myFavoritePet /* "dog" as defined above */, "Rocky"); // dog is from type Dog (dog: Dog)
They works well for User-Defined Type Guards.
 interface Pet {
    species: Species;
    eat();
    sleep();
}
interface Cat extends Pet {
    species: "cat";
}
interface Bird extends Pet {
    species: "bird";
    sing();
}
function petIsCat(pet: Pet): pet is Cat { return pet.species === "cat";


 function petIsBird(pet: Pet): pet is Bird { return pet.species === "bird";
}
function playWithPet(pet: Pet){ if(petIsCat(pet)) {
        // pet is now from type Cat (pet: Cat)
pet.eat();
pet.sleep();
} else if(petIsBird(pet)) {
        // pet is now from type Bird (pet: Bird)
        pet.eat();
        pet.sing();
        pet.sleep();
} }
Full example code
 let myFavoritePet: "dog"; myFavoritePet = "dog";
// Error: Type '"rock"' is not assignable to type '"dog"'.
// myFavoritePet = "rock";
type Species = "cat" | "dog" | "bird";
interface Pet {
    species: Species;
    name: string;
    eat();
    walk();
    sleep();
}
interface Cat extends Pet {
    species: "cat";
}
interface Dog extends Pet {
    species: "dog";
}
interface Bird extends Pet {
    species: "bird";
    sing();
}
// Error: Interface 'Rock' incorrectly extends interface 'Pet'. Types of property 'species' are
incompatible. Type '"rock"' is not assignable to type '"cat" | "dog" | "bird"'. Type '"rock"' is not
assignable to type '"bird"'.
// interface Rock extends Pet {
//      type: "rock";
// }
function buyPet(pet: Species, name: string) : Pet; function buyPet(pet: "cat", name: string): Cat; function buyPet(pet: "dog", name: string): Dog; function buyPet(pet: "bird", name: string): Bird; function buyPet(pet: Species, name: string) : Pet {
if(pet === "cat") 0

 return {
species: "cat",
name: name,
eat: function () {
console.log(`${this.name} eats.`); }, walk: function () {
console.log(`${this.name} walks.`); }, sleep: function () {
console.log(`${this.name} sleeps.`); }
} as Cat;
} else if(pet === "dog") {
return {
species: "dog",
name: name,
eat: function () {
console.log(`${this.name} eats.`); }, walk: function () {
console.log(`${this.name} walks.`); }, sleep: function () {
console.log(`${this.name} sleeps.`); }
} as Dog;
} else if(pet === "bird") {
return {
species: "bird",
name: name,
eat: function () {
console.log(`${this.name} eats.`); }, walk: function () {
console.log(`${this.name} walks.`); }, sleep: function () {
console.log(`${this.name} sleeps.`); }, sing: function () {
console.log(`${this.name} sings.`); }
} as Bird; } else {
throw `Sorry we do not have a ${pet}. Would you like to buy a dog?`; }
}
function petIsCat(pet: Pet): pet is Cat { return pet.species === "cat";
}
function petIsDog(pet: Pet): pet is Dog { return pet.species === "dog";
}
function petIsBird(pet: Pet): pet is Bird { return pet.species === "bird";
}
function playWithPet(pet: Pet) {
console.log(`Hey ${pet.name}, lets play.`); if(petIsCat(pet)) {
        // pet is now from type Cat (pet: Cat)
        pet.eat();
        pet.sleep()1

         // Error: Type '"bird"' is not assignable to type '"cat"'.
        // pet.type = "bird";
        // Error: Property 'sing' does not exist on type 'Cat'.
        // pet.sing();
} else if(petIsDog(pet)) {
// pet is now from type Dog (pet: Dog)
        pet.eat();
        pet.walk();
        pet.sleep();
} else if(petIsBird(pet)) {
// pet is now from type Bird (pet: Bird)
        pet.eat();
        pet.sing();
        pet.sleep();
} else {
throw "An unknown pet. Did you buy a rock?";
} }
let dog = buyPet(myFavoritePet /* "dog" as defined above */, "Rocky"); // dog is from type Dog (dog: Dog)
// Error: Argument of type '"rock"' is not assignable to parameter of type "'cat' | "dog" | "bird".
Type '"rock"' is not assignable to type '"bird"'.
// buyPet("rock", "Rocky");
playWithPet(dog);
// Output: Hey Rocky, lets play.
//         Rocky eats.
//         Rocky walks.
//         Rocky sleeps.
Section 3.2: Tuple
Array type with known and possibly different types:
Section 3.3: Boolean
A boolean represents the most basic datatype in TypeScript, with the purpose of assigning true/false values.
 let day: [number, string];
day = [0, 'Monday']; // valid
day = ['zero', 'Monday']; // invalid: 'zero' is not numeric console.log(day[0]); // 0
console.log(day[1]); // Monday
day[2] = 'Saturday'; // valid: [0, 'Saturday']
day[3] = false; // invalid: must be union type of 'number | string'
 // set with initial value (either true or false)
let isTrue: boolean = true;
// defaults to 'undefined', when not explicitly set
let unsetBool: boolean2

 // can also be set to 'null' as well
let nullableBool: boolean = null;
Section 3.4: Intersection Types
A Intersection Type combines the member of two or more types.
 interface Knife {
    cut();
}
interface BottleOpener{
    openBottle();
}
interface Screwdriver{
    turnScrew();
}
type SwissArmyKnife = Knife & BottleOpener & Screwdriver;
function use(tool: SwissArmyKnife){ console.log("I can do anything!");
    tool.cut();
    tool.openBottle();
    tool.turnScrew();
}
Section 3.5: Types in function arguments and return value. Number
When you create a function in TypeScript you can specify the data type of the function's arguments and the data type for the return value
Example:
Here the syntax x: number, y: number means that the function can accept two argumentsx and y and they can only be numbers and (...): number { means that the return value can only be a number
Usage:
sum(84 + 76) // will be return 160 Note:
You can not do so
or
 function sum(x: number, y: number): number { return x + y;
}
     function sum(x: string, y: string): number { return x + y;
3

 function sum(x: number, y: number): string { return x + y;
}
it will receive the following errors:
error TS2322: Type 'string' is not assignable to type 'number' and error TS2322: Type 'number' is
not assignable to type 'string' respectively
Section 3.6: Types in function arguments and return value.
String
Example:
Here the syntax name: string means that the function can accept one name argument and this argument can only be string and (...): string { means that the return value can only be a string
Usage:
hello('StackOverflow Documentation') // will be return Hello StackOverflow Documentation! Section 3.7: const Enum
A const Enum is the same as a normal Enum. Except that no Object is generated at compile time. Instead, the literal values are substituted where the const Enum is used.
      function hello(name: string): string { return `Hello ${name}!`;
}
    // TypeScript: A const Enum can be defined like a normal Enum (with start value, specific values, etc.)
const enum NinjaActivity {
    Espionage,
    Sabotage,
    Assassination
}
// JavaScript: But nothing is generated
// TypeScript: Except if you use it
let myFavoriteNinjaActivity = NinjaActivity.Espionage; console.log(myFavoritePirateActivity); // 0
// JavaScript: Then only the number of the value is compiled into the code
// var myFavoriteNinjaActivity = 0 /* Espionage */;
// console.log(myFavoritePirateActivity); // 0
// TypeScript: The same for the other constant example console.log(NinjaActivity["Sabotage"]); // 1
// JavaScript: Just the number and in a comment the name of the value
// console.log(1 /* "Sabotage" */); // 1
// TypeScript: But without the object none runtime access is possible
// Error: A const enum member can only be accessed using a string literal.
// console.log(NinjaActivity[myFavoriteNinjaActivity])4

For comparison, a normal Enum
 // TypeScript: A normal Enum
enum PirateActivity {
    Boarding,
Drinking,
Fencing
}
// JavaScript: The Enum after the compiling
// var PirateActivity;
// (function (PirateActivity) {
//     PirateActivity[PirateActivity["Boarding"] = 0] = "Boarding";
//     PirateActivity[PirateActivity["Drinking"] = 1] = "Drinking";
//     PirateActivity[PirateActivity["Fencing"] = 2] = "Fencing";
// })(PirateActivity || (PirateActivity = {}));
// TypeScript: A normal use of this Enum
let myFavoritePirateActivity = PirateActivity.Boarding; console.log(myFavoritePirateActivity); // 0
// JavaScript: Looks quite similar in JavaScript
// var myFavoritePirateActivity = PirateActivity.Boarding;
// console.log(myFavoritePirateActivity); // 0
// TypeScript: And some other normal use console.log(PirateActivity["Drinking"]); // 1
// JavaScript: Looks quite similar in JavaScript
// console.log(PirateActivity["Drinking"]); // 1
// TypeScript: At runtime, you can access an normal enum console.log(PirateActivity[myFavoritePirateActivity]); // "Boarding"
// JavaScript: And it will be resolved at runtime
// console.log(PirateActivity[myFavoritePirateActivity]); // "Boarding"
Section 3.8: Number
Like JavaScript, numbers are floating point values.
ECMAScript 2015 allows binary and octal.
Section 3.9: String
Textual data type:
 let pi: number = 3.14; // base 10 decimal by default let hexadecimal: number = 0xFF; // 255 in decimal
 let binary: number = 0b10; // 2 in decimal let octal: number = 0o755; // 493 in decimal
 let singleQuotes: string = 'single';
let doubleQuotes: string = "double";
let templateString: string = `I am ${ singleQuotes }`; // I am singl5

Section 3.10: Array
An array of values:
Section 3.11: Enum
A type to name a set of numeric values: Number values default to 0:
Set a default starting number:
enum TenPlus { Ten = 10, Eleven, Twelve } or assign values:
enum MyOddSet { Three = 3, Five = 5, Seven = 7, Nine = 9 } Section 3.12: Any
When unsure of a type, any is available:
Section 3.13: Void
If you have no type at all, commonly used for functions that do not return anything:
void types Can only be assigned null or undefined.
 let threePigs: number[] = [1, 2, 3];
let genericStringArray: Array<string> = ['first', '2nd', '3rd'];
 enum Day { Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday }; let bestDay: Day = Day.Saturday;
   let anything: any = 'I am a string'; anything = 5; // but now I am the number 5
 function log(): void { console.log('I return nothing');
}6

Chapter 4: Arrays
Section 4.1: Finding Object in Array
Using find()
 const inventory = [
{name: 'apples', quantity: 2}, {name: 'bananas', quantity: 0}, {name: 'cherries', quantity: 5}
];
function findCherries(fruit) {
return fruit.name === 'cherries';
}
inventory.find(findCherries); // { name: 'cherries', quantity: 5 }
/* OR */
inventory.find(e => e.name === 'apples'); // { name: 'apples', quantity: 2 7

Chapter 5: Enums
Section 5.1: Enums with explicit values
By default all enum values are resolved to numbers. Let's say if you have something like
the real value behind e.g. MimeType.PDF will be 2.
But some of the time it is important to have the enum resolve to a different type. E.g. you receive the value from backend / frontend / another system which is definitely a string. This could be a pain, but luckily there is this method:
This resolves the MimeType.PDF to application/pdf. Since TypeScript 2.4 it's possible to declare string enums:
You can explicitly provide numeric values using the same method
Fancier types also work, since non-const enums are real objects at runtime, for example
becomes
 enum MimeType {
  JPEG,
PNG,
PDF
}
  enum MimeType {
  JPEG = <any>'image/jpeg',
  PNG = <any>'image/png',
  PDF = <any>'application/pdf'
}
    enum MimeType {
  JPEG = 'image/jpeg',
  PNG = 'image/png',
  PDF = 'application/pdf',
}
 enum MyType {
   Value = 3,
ValueEx = 30,
   ValueEx2 = 300
}
 enum FancyType {
   OneArr = <any>[1],
   TwoArr = <any>[2, 2],
   ThreeArr = <any>[3, 3, 3]
}
 var FancyType; (function (FancyType) {
    FancyType[FancyType["OneArr"] = [1]] = "OneArr";
    FancyType[FancyType["TwoArr"] = [2, 2]] = "TwoArr"8

     FancyType[FancyType["ThreeArr"] = [3, 3, 3]] = "ThreeArr";
})(FancyType || (FancyType = {}));
Section 5.2: How to get all enum values
 enum SomeEnum { A, B }
let enumValues:Array<string>= [];
for(let value in SomeEnum) {
if(typeof SomeEnum[value] === 'number') {
        enumValues.push(value);
    }
}
enumValues.forEach(v=> console.log(v)) //A
//B
Section 5.3: Extending enums without custom enum implementation
 enum SourceEnum {
  value1 = <any>'value1',
  value2 = <any>'value2'
}
enum AdditionToSourceEnum {
  value3 = <any>'value3',
  value4 = <any>'value4'
}
// we need this type for TypeScript to resolve the types correctly
type TestEnumType = SourceEnum | AdditionToSourceEnum;
// and we need this value "instance" to use values
let TestEnum = Object.assign({}, SourceEnum, AdditionToSourceEnum); // also works fine the TypeScript 2 feature
// let TestEnum = { ...SourceEnum, ...AdditionToSourceEnum };
function check(test: TestEnumType) { return test === TestEnum.value2;
}
console.log(TestEnum.value1);
console.log(TestEnum.value2 === <any>'value2');
console.log(check(TestEnum.value2));
console.log(check(TestEnum.value3));
Section 5.4: Custom enum implementation: extends for enums
Sometimes it is required to implement Enum on your own. E.g. there is no clear way to extend other enums. Custom implementation allows this:
 class Enum {
  constructor(protected value: string) {}
public toString() {
return String(this.value)9

 }
public is(value: Enum | string) { return this.value = value.toString();
} }
class SourceEnum extends Enum {
public static value1 = new SourceEnum('value1'); public static value2 = new SourceEnum('value2');
}
class TestEnum extends SourceEnum {
public static value3 = new TestEnum('value3'); public static value4 = new TestEnum('value4');
}
function check(test: TestEnum) { return test === TestEnum.value2;
}
let value1 = TestEnum.value1;
console.log(value1 + 'hello');
console.log(value1.toString() === 'value1');
console.log(value1.is('value1'));
console.log(!TestEnum.value3.is(TestEnum.value3));
console.log(check(TestEnum.value2));
// this works but perhaps your TSLint would complain // attention! does not work with ===
// use .is() instead
console.log(TestEnum.value1 == <any>'value1')0

Chapter 6: Functions
Section 6.1: Optional and Default Parameters
Optional Parameters
In TypeScript, every parameter is assumed to be required by the function. You can add a ? at the end of a
parameter name to set it as optional.
For example, the lastName parameter of this function is optional:
Optional parameters must come after all non-optional parameters:
function buildName(firstName?: string, lastName: string) // Invalid Default Parameters
If the user passes undefined or doesn't specify an argument, the default value will be assigned. These are called default-initialized parameters.
For example, "Smith" is the default value for the lastName parameter.
Section 6.2: Function as a parameter
Suppose we want to receive a function as a parameter, we can do it like this:
If we want to receive a constructor as a parameter:
Or to make it easier to read we can define an interface describing the constructor:
  function buildName(firstName: string, lastName?: string) { // ...
}
    function buildName(firstName: string, lastName = "Smith") { // ...
}
buildName('foo', 'bar'); // firstName == 'foo', lastName == 'bar' buildName('foo'); // firstName == 'foo', lastName == 'Smith' buildName('foo', undefined); // firstName == 'foo', lastName == 'Smith'
 function foo(otherFunc: Function): void { ...
}
 function foo(constructorFunc: { new() }) { new constructorFunc();
}
function foo(constructorWithParamsFunc: { new(num: number) }) { new constructorWithParamsFunc(1);
}
 interface IConstructor { new()1

 }
function foo(contructorFunc: IConstructor) { new constructorFunc();
}
Or with parameters:
Even with generics:
If we want to receive a simple function and not a constructor it's almost the same:
Or to make it easier to read we can define an interface describing the function:
Or with parameters:
Even with generics:
 interface INumberConstructor { new(num: number);
}
function foo(contructorFunc: INumberConstructor) { new contructorFunc(1);
}
 interface ITConstructor<T, U> { new(item: T): U;
}
function foo<T, U>(contructorFunc: ITConstructor<T, U>, item: T): U { return new contructorFunc(item);
}
 function foo(func: { (): void }) { func();
}
function foo(constructorWithParamsFunc: { (num: number): void }) { new constructorWithParamsFunc(1);
}
 interface IFunction { (): void;
}
function foo(func: IFunction ) { func();
}
 interface INumberFunction {
    (num: number): string;
}
function foo(func: INumberFunction ) { func(1);
2

 interface ITFunc<T, U> {
    (item: T): U;
}
function foo<T, U>(contructorFunc: ITFunc<T, U>, item: T): U { return func(item);
}
Section 6.3: Functions with Union Types
A TypeScript function can take in parameters of multiple, predefined types using union types.
 function whatTime(hour:number|string, minute:number|string):string{ return hour+':'+minute;
}
whatTime(1,30)
whatTime('1',30)
whatTime(1,'30')
whatTime('1','30')
//'1:30'
//'1:30'
//'1:30'
//'1:30'
TypeScript treats these parameters as a single type that is a union of the other types, so your function must be able to handle parameters of any type that is in the union.
Section 6.4: Types of Functions
Named functions
Anonymous functions
let multiply = function(a, b) { return a * b; }; Lambda / arrow functions
let multiply = (a, b) => { return a * b; };
 function addTen(start:number|string):number{ if(typeof number === 'string'){
return parseInt(number)+10; }else{
else return number+10; }
}
 function multiply(a, b) { return a * b;
}3

Chapter 7: Classes
TypeScript, like ECMAScript 6, support object-oriented programming using classes. This contrasts with older JavaScript versions, which only supported prototype-based inheritance chain.
The class support in TypeScript is similar to that of languages like Java and C#, in that classes may inherit from other classes, while objects are instantiated as class instances.
Also similar to those languages, TypeScript classes may implement interfaces or make use of generics.
Section 7.1: Abstract Classes
 abstract class Machine {
    constructor(public manufacturer: string) {
    }
    // An abstract class can define methods of its own, or...
summary(): string {
return `${this.manufacturer} makes this machine.`;
}
    // Require inheriting classes to implement methods
    abstract moreInfo(): string;
}
class Car extends Machine {
constructor(manufacturer: string, public position: number, protected speed: number) {
        super(manufacturer);
    }
move() {
this.position += this.speed;
}
moreInfo() {
return `This is a car located at ${this.position} and going ${this.speed}mph!`;
} }
let myCar = new Car("Konda", 10, 70);
myCar.move(); // position is now 80
console.log(myCar.summary()); // prints "Konda makes this machine." console.log(myCar.moreInfo()); // prints "This is a car located at 80 and going 70mph!"
Abstract classes are base classes from which other classes can extend. They cannot be instantiated themselves (i.e. youcannotdonew Machine("Konda")).
The two key characteristics of an abstract class in TypeScript are: 1. They can implement methods of their own.
2. They can define methods that inheriting classes must implement.
For this reason, abstract classes can conceptually be considered a combination of an interface and a class.
Section 7.2: Simple class
class Car4
     
     public position: number = 0;
    private speed: number = 42;
move() {
this.position += this.speed;
} }
In this example, we declare a simple class Car. The class has three members: a private property speed, a public property position and a public method move. Note that each member is public by default. That's why move() is public, even if we didn't use the public keyword.
Section 7.3: Basic Inheritance
   var car = new Car(); // create an instance of Car car.move(); // call a method console.log(car.position); // access a public property
 class Car {
    public position: number = 0;
    protected speed: number = 42;
move() {
this.position += this.speed;
} }
class SelfDrivingCar extends Car {
move() {
// start moving around :-) super.move(); super.move();
} }
This examples shows how to create a very simple subclass of the Car class using the extends keyword. The SelfDrivingCar class overrides the move() method and uses the base class implementation using super.
Section 7.4: Constructors
In this example we use the constructor to declare a public property position and a protected property speed in the base class. These properties are called Parameter properties. They let us declare a constructor parameter and a member in one place.
One of the best things in TypeScript, is automatic assignment of constructor parameters to the relevant property.
     class Car {
    public position: number;
    protected speed: number;
constructor(position: number, speed: number) { this.position = position;
this.speed = speed;
}
move() {
this.position += this.speed5

 }
}
All this code can be resumed in one single constructor:
 class Car {
    constructor(public position: number, protected speed: number) {}
move() {
this.position += this.speed;
} }
And both of them will be transpiled from TypeScript (design time and compile time) to JavaScript with same result, but writing significantly less code:
 var Car = (function () {
function Car(position, speed) {
this.position = position;
this.speed = speed; }
Car.prototype.move = function () { this.position += this.speed;
};
return Car; }());
Constructors of derived classes have to call the base class constructor with super().
 class SelfDrivingCar extends Car {
    constructor(startAutoPilot: boolean) {
super(0, 42);
if (startAutoPilot) {
this.move(); }
} }
let car = new SelfDrivingCar(true);
console.log(car.position); // access the public property position
Section 7.5: Accessors
In this example, we modify the "Simple class" example to allow access to the speed property. TypeScript accessors allow us to add additional code in getters or setters.
 class Car {
    public position: number = 0;
    private _speed: number = 42;
    private _MAX_SPEED = 100
move() {
this.position += this._speed;
}
get speed(): number { return this._speed;
6

 set speed(value: number) {
this._speed = Math.min(value, this._MAX_SPEED);
} }
let car = new Car();
car.speed = 120; console.log(car.speed); // 100
Section 7.6: Transpilation
Given a class SomeClass, let's see how the TypeScript is transpiled into JavaScript. TypeScript source
  class SomeClass {
public static SomeStaticValue: string = "hello"; public someMemberValue: number = 15;
private somePrivateValue: boolean = false;
constructor () {
SomeClass.SomeStaticValue = SomeClass.getGoodbye(); this.someMemberValue = this.getFortyTwo(); this.somePrivateValue = this.getTrue();
}
public static getGoodbye(): string { return "goodbye!";
}
public getFortyTwo(): number { return 42;
}
private getTrue(): boolean { return true;
} }
JavaScript source
When transpiled using TypeScript v2.2.2, the output is like so:
  var SomeClass = (function () { function SomeClass() {
this.someMemberValue = 15;
this.somePrivateValue = false; SomeClass.SomeStaticValue = SomeClass.getGoodbye(); this.someMemberValue = this.getFortyTwo(); this.somePrivateValue = this.getTrue();
}
SomeClass.getGoodbye = function () {
return "goodbye!"; };
SomeClass.prototype.getFortyTwo = function () { return 42;
};
SomeClass.prototype.getTrue = function () {
return true; }7

 return SomeClass; }());
SomeClass.SomeStaticValue = "hello";
Observations
The modification of the class' prototype is wrapped inside an IIFE.
Member variables are defined inside the main class function.
Static properties are added directly to the class object, whereas instance properties are added to the prototype.
Section 7.7: Monkey patch a function into an existing class
Sometimes it's useful to be able to extend a class with new functions. For example let's suppose that a string should be converted to a camel case string. So we need to tell TypeScript, that String contains a function called toCamelCase, which returns a string.
Now we can patch this function into the String implementation.
If this extension of String is loaded, it's usable like this:
"This is an example".toCamelCase(); // => "thisIsAnExample"
     interface String {
    toCamelCase(): string;
}
  String.prototype.toCamelCase = function() : string { return this.replace(/[^a-z ]/ig, '')
.replace(/(?:^\w|[A-Z]|\b\w|\s+)/g, (match: any, index: number) => {
return +match === 0 ? "" : match[index === 0 ? 'toLowerCase' : 'toUpperCase']();
}); }8

Chapter 8: Class Decorator
Parameter Details
target The class being decorated
Section 8.1: Generating metadata using a class decorator
This time we are going to declare a class decorator that will add some metadata to a class when we applied to it:
 function addMetadata(target: any) {
    // Add some metadata
    target.__customMetadata = {
        someKey: "someValue"
};
    // Return target
return target; }
We can then apply the class decorator:
 @addMetadata
class Person {
    private _name: string;
    public constructor(name: string) {
this._name = name; }
public greet() { return this._name;
} }
function getMetadataFromClass(target: any) { return target.__customMetadata;
}
console.log(getMetadataFromClass(Person));
The decorator is applied when the class is declared not when we create instances of the class. This means that the metadata is shared across all the instances of a class:
 function getMetadataFromInstance(target: any) { return target.constructor.__customMetadata;
}
let person1 = new Person("John");
let person2 = new Person("Lisa");
console.log(getMetadataFromInstance(person1));
console.log(getMetadataFromInstance(person2));
Section 8.2: Passing arguments to a class decorator
We can wrap a class decorator with another function to allow customization9

 function addMetadata(metadata: any) { return function log(target: any) {
        // Add metadata
target.__customMetadata = metadata; // Return target
return target; }
}
The addMetadata takes some arguments used as configuration and then returns an unnamed function which is the actual decorator. In the decorator we can access the arguments because there is a closure in place.
We can then invoke the decorator passing some configuration values:
  @addMetadata({ guid: "417c6ec7-ec05-4954-a3c6-73a0d7f9f5bf" })
class Person {
    private _name: string;
    public constructor(name: string) {
this._name = name; }
public greet() { return this._name;
} }
We can use the following function to access the generated metadata:
If everything went right the console should display:
{ guid: "417c6ec7-ec05-4954-a3c6-73a0d7f9f5bf" } Section 8.3: Basic class decorator
A class decorator is just a function that takes the class as its only argument and returns it after doing something with it:
 function getMetadataFromClass(target: any) { return target.__customMetadata;
}
console.log(getMetadataFromInstance(Person));
  function log<T>(target: T) { // Do something with target
console.log(target); // Return target
return target; }
We can then apply the class decorator to a class0

 @log
class Person {
    private _name: string;
    public constructor(name: string) {
this._name = name; }
public greet() { return this._name;
} 1

Chapter 9: Interfaces
An interfaces specifies a list of fields and functions that may be expected on any class implementing the interface. Conversely, a class cannot implement an interface unless it has every field and function specified on the interface.
The primary benefit of using interfaces, is that it allows one to use objects of different types in a polymorphic way. This is because any class implementing the interface has at least those fields and functions.
Section 9.1: Extending Interface
Suppose we have an interface:
And we want to create more specific interface that has the same properties of the person, we can do it using the extends keyword:
In addition it is possible to extend multiple interfaces.
Section 9.2: Class Interface
Declare public variables and methods type in the interface to define how other typescript code can interact with it.
Here we create a class that implements the interface.
 interface IPerson {
    name: string;
age: number; breath(): void;
}
  interface IManager extends IPerson {
    managerId: number;
managePeople(people: IPerson[]): void; }
  interface ISampleClassInterface {
  sampleVariable: string;
sampleMethod(): void; optionalVariable?: string;
}
 class SampleClass implements ISampleClassInterface {
  public sampleVariable: string;
  private answerToLifeTheUniverseAndEverything: number;
constructor() {
this.sampleVariable = 'string value'; this.answerToLifeTheUniverseAndEverything = 42;
}
public sampleMethod(): void { // do nothing
2

 private answer(q: any): number {
return this.answerToLifeTheUniverseAndEverything;
} }
The example shows how to create an interface ISampleClassInterface and a class SampleClass that implements the interface.
Section 9.3: Using Interfaces for Polymorphism
The primary reason to use interfaces to achieve polymorphism and provide developers to implement on their own way in future by implementing interface's methods.
Suppose we have an interface and three classes:
This is connector interface. Now we will implement that for Wifi communication.
    interface Connector{
    doConnect(): boolean;
}
 export class WifiConnector implements Connector{
    public doConnect(): boolean{
        console.log("Connecting via wifi");
        console.log("Get password");
        console.log("Lease an IP for 24 hours");
        console.log("Connected");
return true }
}
Here we have developed our concrete class named WifiConnector that has its own implementation. This is now type Connector.
Now we are creating our System that has a component Connector. This is called dependency injection.
constructor(private connector: Connector) this line is very important here. Connector is an interface and must have doConnect(). As Connector is an interface this class System has much more flexibility. We can pass any Type which has implemented Connector interface. In future developer achieves more flexibility. For example, now developer want to add Bluetooth Connection module:
     export class System {
    constructor(private connector: Connector){ #inject Connector type
        connector.doConnect()
    }
}
         export class BluetoothConnector implements Connector{
public doConnect(): boolean{ console.log("Connecting via Bluetooth"); console.log("Pair with PIN"); console.log("Connected");
return true
3

 }
See that Wifi and Bluetooth have its own implementation. Their own different way to connect. However, hence both have implemented Type Connector the are now Type Connector. So that we can pass any of those to System class as the constructor parameter. This is called polymorphism. The class System is now not aware of whether it is Bluetooth / Wifi even we can add another Communication module like Infrared, Bluetooth5 and whatsoever by just implementing Connector interface.
This is called Duck typing. Connector type is now dynamic as doConnect() is just a placeholder and developer implement this as his/her own.
if at constructor(private connector: WifiConnector) where WifiConnector is a concrete class what will happen? Then System class will tightly couple only with WifiConnector nothing else. Here interface solved our problem by polymorphism.
Section 9.4: Generic Interfaces
Like classes, interfaces can receive polymorphic parameters (aka Generics) too.
Declaring Generic Parameters on Interfaces
              interface IStatus<U> {
    code: U;
}
interface IEvents<T> {
    list: T[];
emit(event: T): void;
    getAll(): T[];
}
Here, you can see that our two interfaces take some generic parameters, T and U. Implementing Generic Interfaces
We will create a simple class in order to implements the interface IEvents.
 class State<T> implements IEvents<T> {
    list: T[];
constructor() { this.list = [];
}
emit(event: T): void { this.list.push(event);
}
getAll(): T[] { return this.list;
} }
Let's create some instances of our State class.
In our example, the State class will handle a generic status by using IStatus<T>. In this way, the interface4

IEvent<T> will also handle a IStatus<T>.
   const s = new State<IStatus<number>>();
// The 'code' property is expected to be a number, so: s.emit({ code: 200 }); // works
s.emit({ code: '500' }); // type error
s.getAll().forEach(event => console.log(event.code));
Here our State class is typed as IStatus<number>.
   const s2 = new State<IStatus<Code>>(); //We are able to emit code as the type Code
s2.emit({ code: { message: 'OK', status: 200 } });
s2.getAll().map(event => event.code).forEach(event => {
    console.log(event.message);
    console.log(event.status);
});
Our State class is typed as IStatus<Code>. In this way, we are able to pass more complex type to our emit method. As you can see, generic interfaces can be a very useful tool for statically typed code.
Section 9.5: Add functions or properties to an existing interface
Let's suppose we have a reference to the JQuery type definition and we want to extend it to have additional functions from a plugin we included and which doesn't have an official type definition. We can easily extend it by declaring functions added by plugin in a separate interface declaration with the same JQuery name:
The compiler will merge all declarations with the same name into one - see declaration merging for more details. Section 9.6: Implicit Implementation And Object Shape
TypeScript supports interfaces, but the compiler outputs JavaScript, which doesn't. Therefore, interfaces are effectively lost in the compile step. This is why type checking on interfaces relies on the shape of the object - meaning whether the object supports the fields and functions on the interface - and not on whether the interface is actually implemented or not.
    interface JQuery { pluginFunctionThatDoesNothing(): void;
  // create chainable function
  manipulateDOM(HTMLElement): JQuery;
}
  interface IKickable { kick(distance: number): void;
}
class Ball {
kick(distance: number): void { console.log("Kicked", distance, "meters!");
5

 }
let kickable: IKickable = new Ball(); kickable.kick(40);
So even if Ball doesn't explicitly implement IKickable, a Ball instance may be assigned to (and manipulated as) an IKickable, even when the type is specified.
Section 9.7: Using Interfaces to Enforce Types
One of the core benefits of TypeScript is that it enforces data types of values that you are passing around your code to help prevent mistakes.
Let's say you're making a pet dating application.
You have this simple function that checks if two pets are compatible with each other...
This is completely functional code, but it would be far too easy for someone, especially other people working on this application who didn't write this function, to be unaware that they are supposed to pass it objects with 'species' and 'age' properties. They may mistakenly try checkCompatible(petOne.species, petTwo.species) and then be left to figure out the errors thrown when the function tries to access petOne.species.species or petOne.species.age!
One way we can prevent this from happening is to specify the properties we want on the pet parameters:
In this case, TypeScript will make sure everything passed to the function has 'species' and 'age' properties (it is okay if they have additional properties), but this is a bit of an unwieldy solution, even with only two properties specified. With interfaces, there is a better way!
First we define our interface:
Now all we have to do is specify the type of our parameters as our new interface, like so...
... and TypeScript will make sure that the parameters passed to our function contain the properties specified in the Pet interface!
   checkCompatible(petOne, petTwo) {
if (petOne.species === petTwo.species &&
Math.abs(petOne.age - petTwo.age) <= 5) { return true;
} }
    checkCompatible(petOne: {species: string, age: number}, petTwo: {species: string, age: number}) { //...
}
 interface Pet {
species: string;
age: number;
//We can add more properties if we choose.
}
 checkCompatible(petOne: Pet, petTwo: Pet) { //...
6

Chapter 10: Generics Section 10.1: Generic Interfaces
Declaring a generic interface
Generic interface with multiple type parameters
Implementing a generic interface
Implement it with generic class:
 interface IResult<T> {
    wasSuccessful: boolean;
    error: T;
}
var result: IResult<string> = .... var error: string = result.error;
 interface IRunnable<T, U> {
    run(input: T): U;
}
var runnable: IRunnable<string, number> = ... var input: string;
var result: number = runnable.run(input);
 interface IResult<T>{
    wasSuccessful: boolean;
    error: T;
    clone(): IResult<T>;
}
 class Result<T> implements IResult<T> {
    constructor(public result: boolean, public error: T) {
    }
public clone(): IResult<T> {
return new Result<T>(this.result, this.error);
} }
Implement it with non generic class:
Section 10.2: Generic Class
class Result<T>7
 class StringResult implements IResult<string> {
    constructor(public result: boolean, public error: string) {
    }
public clone(): IResult<string> {
return new StringResult(this.result, this.error);
} }
   
     constructor(public wasSuccessful: boolean, public error: T) {
    }
    public clone(): Result<T> {
       ...
} }
let r1 let r2 let r3 let r4
= new Result(false, 'error: 42'); // Compiler infers T to string
= new Result(false, 42); // Compiler infers T to number
= new Result<string>(true, null); // Explicitly set T to string
= new Result<string>(true, 4); // Compilation error because 4 is not a string
Section 10.3: Type parameters as constraints
With TypeScript 1.8 it becomes possible for a type parameter constraint to reference type parameters from the same type parameter list. Previously this was an error.
 function assign<T extends U, U>(target: T, source: U): T { for (let id in source) {
        target[id] = source[id];
    }
return target; }
let x = { a: 1, b: 2, c: 3, d: 4 }; assign(x, { b: 10, d: 20 }); assign(x, { e: 0 }); // Error
Section 10.4: Generics Constraints
Simple constraint:
More complex constraint:
Even more complex:
 interface IRunnable { run(): void;
}
interface IRunner<T extends IRunnable> { runSafe(runnable: T): void;
}
 interface IRunnble<U> {
    run(): U;
}
interface IRunner<T extends IRunnable<U>, U> {
    runSafe(runnable: T): U;
}
 interface IRunnble<V> {
    run(parameter: U): V;
}
interface IRunner<T extends IRunnable<U, V>, U, V> 8

     runSafe(runnable: T, parameter: U): V;
}
Inline type constraints:
 interface IRunnable<T extends { run(): void }> { runSafe(runnable: T): void;
}
Section 10.5: Generic Functions
In interfaces:
In classes:
 interface IRunner {
runSafe<T extends IRunnable>(runnable: T): void;
}
 class Runner implements IRunner {
public runSafe<T extends IRunnable>(runnable: T): void { try {
runnable.run(); } catch(e) {
} }
}
Simple functions:
Section 10.6: Using generic Classes and Functions:
Create generic class instance:
var stringRunnable = new Runnable<string>(); Run generic function:
 function runSafe<T extends IRunnable>(runnable: T): void { try {
runnable.run(); } catch(e) {
} }
  function runSafe<T extends Runnable<U>, U>(runnable: T); // Specify the generic types:
runSafe<Runnable<string>, string>(stringRunnable);
// Let typescript figure the generic types by himself:
runSafe(stringRunnable)9

Chapter 11: Strict null checks Section 11.1: Strict null checks in action
By default, all types in TypeScript allow null:
TypeScript 2.0 adds support for strict null checks. If you set --strictNullChecks when running tsc (or set this flag in your tsconfig.json), then types no longer permit null:
You must permit null values explicitly:
With a proper guard, the code type checks and runs correctly:
Section 11.2: Non-null assertions
The non-null assertion operator, !, allows you to assert that an expression isn't null or undefined when the TypeScript compiler can't infer that automatically:
 function getId(x: Element) { return x.id;
}
getId(null); // TypeScript does not complain, but this is a runtime error.
   function getId(x: Element) { return x.id;
}
getId(null); // error: Argument of type 'null' is not assignable to parameter of type 'Element'.
 function getId(x: Element|null) {
return x.id; // error TS2531: Object is possibly 'null'.
} getId(null);
 function getId(x: Element|null) { if (x) {
return x.id; // In this branch, x's type is Element } else {
return null; // In this branch, x's type is null. }
} getId(null);
  type ListNode = { data: number; next?: ListNode; };
function addNext(node: ListNode) { if (node.next === undefined) {
        node.next = {data: 0};
    }
}
function setNextValue(node: ListNode, value: number) {
    addNext(node);
    // Even though we know `node.next` is defined because we just called `addNext`,
    // TypeScript isn't able to infer this in the line of code below:
    // node.next.data = value0

 // So, we can use the non-null assertion operator, !,
// to assert that node.next isn't undefined and silence the compiler warning node.next!.data = value;
1

Chapter 12: User-defined Type Guards
Section 12.1: Type guarding functions
You can declare functions that serve as type guards using any logic you'd like. They take the form:
If the function returns true, TypeScript will narrow the type to DesiredType in any block guarded by a call to the function.
For example (try it):
 function functionName(variableName: any): variableName is DesiredType { // body that returns boolean
}
   function isString(test: any): test is string { return typeof test === "string";
}
function example(foo: any) { if (isString(foo)) {
        // foo is type as a string in this block
console.log("it's a string: " + foo); } else {
        // foo is type any in this block
        console.log("don't know what this is! [" + foo + "]");
    }
}
example("hello world"); // prints "it's a string: hello world"
example({ something: "else" }); // prints "don't know what this is! [[object Object]]"
A guard's function type predicate (the foo is Bar in the function return type position) is used at compile time to narrow types, the function body is used at runtime. The type predicate and function must agree, or your code won't work.
Type guard functions don't have to use typeof or instanceof, they can use more complicated logic. For example, this code determines if you've got a jQuery object by checking for its version string.
    function isJQuery(foo): foo is JQuery { // test for jQuery's version string return foo.jquery !== undefined;
}
function example(foo) { if (isJQuery(foo)) {
        // foo is typed JQuery here
foo.eq(0); }
2

Section 12.2: Using instanceof
instanceof requires that the variable is of type any. This code (try it):
   class Pet { }
class Dog extends Pet {
    bark() {
        console.log("woof");
} }
class Cat extends Pet {
    purr() {
        console.log("meow");
    }
}
function example(foo: any) { if (foo instanceof Dog) {
        // foo is type Dog in this block
        foo.bark();
    }
if (foo instanceof Cat) {
// foo is type Cat in this block foo.purr();
} }
example(new Dog()); example(new Cat());
prints
to the console.
Section 12.3: Using typeof
typeof is used when you need to distinguish between types number, string, boolean, and symbol. Other string constants will not error, but won't be used to narrow types either.
Unlike instanceof, typeof will work with a variable of any type. In the example below, foo could be typed as number | string without issue.
This code (try it):
 woof meow
           function example(foo: any) {
if (typeof foo === "number") {
        // foo is type number in this block
        console.log(foo + 100);
    }
if (typeof foo === "string") 3

         // foo is type string in this block
        console.log("not a number: " + foo);
    }
example(23);
example("foo");
}
prints
 123
not a number: fo4

Chapter 13: TypeScript basic examples
Section 13.1: 1 basic class inheritance example using extends and super keyword
A generic Car class has some car property and a description method
 class Car{
    name:string;
    engineCapacity:string;
constructor(name:string,engineCapacity:string){ this.name = name;
this.engineCapacity = engineCapacity;
}
describeCar(){
console.log(`${this.name} car comes with ${this.engineCapacity} displacement`);
} }
new Car("maruti ciaz","1500cc").describeCar();
HondaCar extends the existing generic car class and adds new property.
 class HondaCar extends Car{
    seatingCapacity:number;
constructor(name:string,engineCapacity:string,seatingCapacity:number){ super(name,engineCapacity);
this.seatingCapacity=seatingCapacity;
}
describeHondaCar(){
super.describeCar();
console.log(`this cars comes with seating capacity of ${this.seatingCapacity}`);
} }
new HondaCar("honda jazz","1200cc",4).describeHondaCar();
Section 13.2: 2 static class variable example - count how many time method is being invoked
here countInstance is a static class variable
 class StaticTest{
static countInstance : number= 0; constructor(){
        StaticTest.countInstance++;
    }
}
new StaticTest();
new StaticTest(); console.log(StaticTest.countInstance)5

Chapter 14: Importing external libraries
Section 14.1: Finding definition files
for typescript 2.x:
definitions from DefinitelyTyped are available via @types npm package
but in case if you want use types from other repos then can be used old way: for typescript 1.x:
Typings is an npm package that can automatically install type definition files into a local project. I recommend that you read the quickstart.
npm install -global typings
Now we have access to the typings cli.
1. The first step is to search for the package used by the project
   npm i --save lodash
npm i --save-dev @types/lodash
    typings search lodash
NAME SOURCE HOMEPAGE
UPDATED
lodash dt http://lodash.com/
 2016-07-20T00:13:09.000Z
lodash            global
2016-07-01T20:51:07.000Z
lodash npm https://www.npmjs.com/package/lodash
 2016-07-01T20:51:07.000Z
DESCRIPTION VERSIONS
2 1 1
2. Then decide which source you should install from. I use dt which stands for DefinitelyTyped a GitHub repo where the community can edit typings, it's also normally the most recently updated.
3. Install the typings files
       typings install dt~lodash --global --save
Let's break down the last command. We are installing the DefinitelyTyped version of lodash as a global typings file in our project and saving it as a dependency in the typings.json. Now wherever we import lodash, typescript will load the lodash typings file.
4. If we want to install typings that will be used for development environment only, we can supply the --save- dev flag:
       typings install chai --save-dev
6

Section 14.2: Importing a module from npm
If you have a type definition file (d.ts) for the module, you can use an import statement. import _ = require('lodash');
If you don't have a definition file for the module, TypeScript will throw an error on compilation because it cannot find the module you are trying to import.
In this case, you can import the module with the normal runtime require function. This returns it as the any type, however.
As of TypeScript 2.0, you can also use a shorthand ambient module declaration in order to tell TypeScript that a module exists when you don't have a type definition file for the module. TypeScript won't be able to provide any meaningful typechecking in this case though.
As of TypeScript 2.1, the rules have been relaxed even further. Now, as long as a module exists in your node_modules directory, TypeScript will allow you to import it, even with no module declaration anywhere. (Note that if using the --noImplicitAny compiler option, the below will still generate a warning.)
Section 14.3: Using global external libraries without typings
Although modules are ideal, if the library you are using is referenced by a global variable (like $ or _), because it was loaded by a script tag, you can create an ambient declaration in order to refer to it:
declare const _: any;
Section 14.4: Finding definition files with TypeScript 2.x
With the 2.x versions of TypeScript, typings are now available from the npm @types repository. These are automatically resolved by the TypeScript compiler and are much simpler to use.
To install a type definition you simply install it as a dev dependency in your projects package.json e.g.
after install you simply use the module as before
    // The _ variable is of type any, so TypeScript will not perform any type checking.
const _: any = require('lodash');
 declare module "lodash";
// you can now import from lodash in any way you wish:
import { flatten } from "lodash";
import * as _ from "lodash";
   // Will work if `node_modules/someModule/index.js` exists, or if `node_modules/someModule/package.json` has a valid "main" entry point import { foo } from "someModule";
    npm i -S lodash
npm i -D @types/lodas7

 import * as _ from 'lodash8

Chapter 15: Modules - exporting and importing
Section 15.1: Hello world module
 //hello.ts
export function hello(name: string){ console.log(`Hello ${name}!`);
}
function helloES(name: string){
    console.log(`Hola ${name}!`);
}
export {helloES}; export default hello;
Load using directory index
If directory contains file named index.ts it can be loaded using only directory name (for index.ts filename is optional).
Example usage of defined modules
   //welcome/index.ts
export function welcome(name: string){ console.log(`Welcome ${name}!`);
}
 import {hello, helloES} from "./hello";
import defaultHello from "./hello";
import * as Bundle from "./hello";
import {welcome} from "./welcome";
hello("World");
helloES("Mundo");
defaultHello("World");
Bundle.hello("World");
Bundle.helloES("Mundo");
welcome("Human");
// load specified elements
// load default export into name defaultHello
// load all exports as Bundle
// note index.ts is omitted
// Hello World!
// Hola Mundo!
// Hello World!
// Hello World!
// Hola Mundo!
// Welcome Human!
Section 15.2: Re-export
TypeScript allow to re-export declarations.
//Add.ts
 import Operator from "./Operator";
 export class Add implements Operator {
eval(a: number, b: number): number { return a + b;
}
 //Operator.ts
interface Operator {
    eval(a: number, b: number): number;
}
export default Operator9

 }
//Mul.ts
 import Operator from "./Operator";
 export class Mul implements Operator {
eval(a: number, b: number): number { return a * b;
} }
You can bundle all operations in single library
Named declarations can be re-exported using shorter syntax
Default exports can also be exported, but no short syntax is available. Remember, only one default export per module is possible.
Possible is re-export of bundled import
When re-exporting bundle, declarations may be overridden when declared explicitly.
 //Operators.ts
import {Add} from "./Add";
import {Mul} from "./Mul";
export {Add, Mul};
 //NamedOperators.ts
export {Add} from "./Add";
export {Mul} from "./Mul";
 //Calculator.ts
export {Add} from "./Add";
export {Mul} from "./Mul";
import Operator from "./Operator";
export default Operator;
 //RepackedCalculator.ts
export * from "./Operators";
 //FixedCalculator.ts
export * from "./Calculator"
import Operator from "./Calculator";
export class Add implements Operator {
eval(a: number, b: number): number { return 42;
} }
Usage example
 //run.ts
import {Add, Mul} from "./FixedCalculator";
const add = new Add(); const mul = new Mul()0

 console.log(add.eval(1, 1)); // 42 console.log(mul.eval(3, 4)); // 12
Section 15.3: Exporting/Importing declarations
Any declaration (variable, const, function, class, etc.) can be exported from module to be imported in other module. TypeScript offer two export types: named and default.
Named export
When importing named exports, you can specify which elements you want to import.
Default export
Each module can have one default export
which can be imported using
Bundled import
TypeScript offers method to import whole module into variable
import * as Bundle from "./adams"; Bundle.hello(Bundle.answerToLifeTheUniverseAndEverything); // Hello 42! console.log(Bundle.unused); // 0
 // adams.ts
export function hello(name: string){ console.log(`Hello ${name}!`);
}
export const answerToLifeTheUniverseAndEverything = 42; export const unused = 0;
 import {hello, answerToLifeTheUniverseAndEverything} from "./adams"; hello(answerToLifeTheUniverseAndEverything); // Hello 42!
 // dent.ts
const defaultValue = 54; export default defaultValue;
 import dentValue from "./dent"; console.log(dentValue); // 54
 // adams.ts
export function hello(name: string){ console.log(`Hello ${name}!`);
}
export const answerToLifeTheUniverseAndEverything = 421

Chapter 16: Publish TypeScript definition files
Section 16.1: Include definition file with library on npm
Add typings to your package.json
Now whenever that library is imported typescript will load the typings file
 {
...
"typings": "path/file.d.ts"
...
2

Chapter 17: Using TypeScript with webpack
Section 17.1: webpack.config.js
install loaders npm install --save-dev ts-loader source-map-loader tsconfig.json
   {
  "compilerOptions": {
"sourceMap": true,
"noImplicitAny": true,
"module": "commonjs",
"target": "es5",
"jsx": "react" // if you want to use react jsx
} }
module.exports = {
    entry: "./src/index.ts",
    output: {
        filename: "./dist/bundle.js",
    },
    // Enable sourcemaps for debugging webpack's output.
    devtool: "source-map",
resolve: {
// Add '.ts' and '.tsx' as resolvable extensions.
extensions: ["", ".webpack.js", ".web.js", ".ts", ".tsx", ".js"]
},
    module: {
        loaders: [
            // All files with a '.ts' or '.tsx' extension will be handled by 'ts-loader'.
{test: /\.tsx?$/, loader: "ts-loader"} ],
preLoaders: [
// All output '.js' files will have any sourcemaps re-processed by 'source-map-loader'. {test: /\.js$/, loader: "source-map-loader"}
] },
    /*****************************
     *  If you want to use react *
     ****************************/
    // When importing a module whose path matches one of the following, just
    // assume a corresponding global variable exists and use that instead.
    // This is important because it allows us to avoid bundling all of our
    // dependencies, which allows browsers to cache those libraries between builds.
    // externals: {
    //     "react": "React",
    //     "react-dom": "ReactDOM"
    // },
}3

Chapter 18: Mixins
Parameter Description
derivedCtor The class that you want to use as the composition class baseCtors An array of classes to be added to the composition class
Section 18.1: Example of Mixins
To create mixins, simply declare lightweight classes that can be used as "behaviours".
 class Flies {
    fly() {
        alert('Is it a bird? Is it a plane?');
    }
}
class Climbs {
    climb() {
        alert('My spider-sense is tingling.');
    }
}
class Bulletproof {
    deflect() {
        alert('My wings are a shield of steel.');
    }
}
You can then apply these behaviours to a composition class:
The applyMixins function is needed to do the work of composition.
 class BeetleGuy implements Climbs, Bulletproof { climb: () => void;
deflect: () => void;
applyMixins (BeetleGuy, [Climbs, Bulletproof]);
}
  function applyMixins(derivedCtor: any, baseCtors: any[]) { baseCtors.forEach(baseCtor => {
Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => { if (name !== 'constructor') {
derivedCtor.prototype[name] = baseCtor.prototype[name]; }
}); });
4

Chapter 19: How to use a JavaScript library without a type definition file
While some existing JavaScript libraries have type definition files, there are many that don't. TypeScript offers a couple patterns to handle missing declarations.
Section 19.1: Make a module that exports a default any
For more complicated projects, or in cases where you intend to gradually type a dependency, it may be cleaner to create a module.
Using JQuery (although it does have typings available) as an example:
And then in any file in your project, you can import this definition with:
After this import, $ will be typed as any.
If the library has multiple top-level variables, export and import by name instead:
   // place in jquery.d.ts
declare let $: any; export default $;
 // some other .ts file
import $ from "jquery";
 // place in jquery.d.ts
declare module "jquery" { let $: any;
let jQuery: any;
export { $ };
   export { jQuery };
}
You can then import and use both names:
Section 19.2: Declare an any global
It is sometimes easiest to just declare a global of type any, especially in simple projects. If jQuery didn't have type declarations (it does), you could put
declare var $: any;
Now any use of $ will be typed any.
 // some other .ts file
import {$, jQuery} from "jquery";
$.doThing();
jQuery.doOtherThing();5

Section 19.3: Use an ambient module
If you just want to indicate the intent of an import (so you don't want to declare a global) but don't wish to bother with any explicit definitions, you can import an ambient module.
You can then import from the ambient module.
Anything imported from the declared module (like $ and jQuery) above will be of type any
 // in a declarations file (like declarations.d.ts)
declare module "jquery"; // note that there are no defined exports
 // some other .ts file
import {$, jQuery} from "jquery";6

Chapter 20: TypeScript installing typescript and running the typescript compiler tsc
How to install TypeScript and run the TypeScript compiler against a .ts file from the command line.
Section 20.1: Steps
Installing TypeScript and running typescript compiler. To install TypeScript Compiler
npm install -g typescript
To check with the typescript version
tsc -v
Download Visual Studio Code for Linux/Windows
Visual Code Download Link
1. Open Visual Studio Code
2. Open Same Folder where you have installed TypeScript compiler 3. Add File by clicking on plus icon on left pane
4. Create a basic class.
5. Compile your type script file and generate output.
7

 See the result in compiled javascript of written typescript code.
 Thank you8

Chapter 21: Configure typescript project to compile all files in typescript.
creating your first .tsconfig configuration file which will tell the TypeScript compiler how to treat your .ts files
Section 21.1: TypeScript Configuration file setup
Enter command "tsc --init" and hit enter.
Before that we need to compile ts file with command "tsc app.ts" now it is all defined in below config file automatically.
Now, You can compile all typescripts by command "tsc". it will automatically create ".js" file of your typescript file.9

 If you will create another typescript and hit "tsc" command in command prompt or terminal javascript file will be automatically created for typescript file.
