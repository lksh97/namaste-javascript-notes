# Episode 18: Higher-Order Functions ft. Functional Programming

### Q: What is a Higher-Order Function?
**Ans**: A higher-order function is a function that does one or both of the following:
1. **Takes one or more functions as arguments**: This allows the function to dynamically decide what operation to perform based on the function passed to it.
2. **Returns a function as its result**: This enables the creation of more complex and reusable code.

**Example**:
```js
function sayHello() {
    console.log("Hi");
};

function executeFunction(fn) {
    fn();
};

executeFunction(sayHello); // Output: Hi
// executeFunction is a higher-order function because it takes another function (sayHello) as an argument.
// sayHello is a callback function.
```

### Q: What is a Callback Function?
**Ans**: A callback function is a function that is passed as an argument to another function and is executed inside that function. It allows you to defer actions until a certain condition is met or an event occurs.

**Example**:
```js
function greet(name) {
    console.log("Hello, " + name);
}

function processUserInput(callback) {
    const name = "Alice";
    callback(name);
}

processUserInput(greet); // Output: Hello, Alice
// Here, greet is a callback function because it is passed to processUserInput and is executed later.
```

### Applying Higher-Order Functions in Problem Solving

Let's explore how higher-order functions can be useful in solving real-world problems, such as calculating values based on an array of data.

### Scenario: Calculating Area and Circumference

Suppose you have an array of radii, and you need to calculate the area for each radius and store the results in a new array. Here's one way to do it:

**First Approach**:
```js
const radius = [1, 2, 3, 4];

const calculateArea = function(radius) {
    const output = [];
    for (let i = 0; i < radius.length; i++) {
        output.push(Math.PI * radius[i] * radius[i]);
    } 
    return output;
}

console.log(calculateArea(radius)); // Output: [3.14, 12.57, 28.27, 50.27]
```
This works fine, but what if you now need to calculate the circumference as well? You would end up duplicating a lot of code:

**Second Approach**:
```js
const radius = [1, 2, 3, 4];

const calculateCircumference = function(radius) {
    const output = [];
    for (let i = 0; i < radius.length; i++) {
        output.push(2 * Math.PI * radius[i]);
    } 
    return output;
}

console.log(calculateCircumference(radius)); // Output: [6.28, 12.57, 18.85, 25.13]
```
However, this violates the DRY (Don't Repeat Yourself) principle. Instead, let's refactor the code using a higher-order function:

**Better Approach Using Higher-Order Functions**:
```js
const radiusArr = [1, 2, 3, 4];

// Function to calculate area
const area = function(radius) {
    return Math.PI * radius * radius;
}

// Function to calculate circumference
const circumference = function(radius) {
    return 2 * Math.PI * radius;
}

// Higher-order function that applies any operation to each element in the array
const calculate = function(radiusArr, operation) {
    const output = [];
    for (let i = 0; i < radiusArr.length; i++) {
        output.push(operation(radiusArr[i]));
    } 
    return output;
}

console.log(calculate(radiusArr, area)); // Output: [3.14, 12.57, 28.27, 50.27]
console.log(calculate(radiusArr, circumference)); // Output: [6.28, 12.57, 18.85, 25.13]

// Here, calculate is a higher-order function because it takes another function (operation) as an argument.
// area and circumference are callback functions.
```
By using higher-order functions, you make your code more modular, reusable, and easier to maintain.

### Polyfill for `map` Function

In fact, the `calculate` function above is very similar to how JavaScript's built-in `map` function works. The `map` function applies a given operation to every element in an array and returns a new array.

**Example**:
```js
console.log(radiusArr.map(area)); // Output: [3.14, 12.57, 28.27, 50.27]
// This is functionally the same as:
console.log(calculate(radiusArr, area));
```

You can even create a custom `map` function by extending the `Array` prototype:

```js
Array.prototype.calculate = function(operation) {
    const output = [];
    for (let i = 0; i < this.length; i++) {
        output.push(operation(this[i]));
    } 
    return output;
}

console.log(radiusArr.calculate(area)); // Output: [3.14, 12.57, 28.27, 50.27]
```

### Additional Points:
1. **Functional Programming**: In functional programming, functions are treated as first-class citizens. This means you can pass functions around as arguments, return them from other functions, and assign them to variables.

2. **Immutability**: Functional programming often emphasizes immutability, where data is not modified but instead new data is created. This makes your code more predictable and easier to debug.

3. **Pure Functions**: A pure function is one that, given the same input, will always return the same output and has no side effects. Higher-order functions often rely on pure functions to maintain consistency.

4. **Real-World Example**: Consider an e-commerce website where you need to apply different discount calculations to various products. Using higher-order functions, you can create a `calculatePrice` function that takes a product array and a discount function as arguments, applying the discount dynamically based on the function provided.

**Example**:
```js
const products = [
    { name: "Laptop", price: 1000 },
    { name: "Phone", price: 500 }
];

const tenPercentDiscount = function(price) {
    return price * 0.9;
}

const calculatePrice = function(products, discountFn) {
    return products.map(product => {
        return {
            name: product.name,
            price: discountFn(product.price)
        };
    });
}

console.log(calculatePrice(products, tenPercentDiscount));
// Output: [{ name: "Laptop", price: 900 }, { name: "Phone", price: 450 }]
```

By understanding and using higher-order functions and callback functions effectively, you can write more flexible, reusable, and maintainable JavaScript code.

---

Watch Live On YouTube below:

<a href="https://www.youtube.com/watch?v=HkWxvB1RJq0&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/HkWxvB1RJq0/0.jpg" width="750"
alt="Higher-Order Functions ft. Functional Programming in JS YouTube Link"/></a>

-----

# Episode 18 : Higher-Order Functions ft. Functional Programming

### Q: What is Higher Order Function?
**Ans**: Higher-order functions are regular functions that take one or more functions as arguments and/or return functions as a value from it. Eg: 
```js
function x() {
    console.log("Hi");
};
function y(x) {
    x();
};
y(x); // Hi
// y is a higher order function
// x is a callback function
```

Let's try to understand how we should approach solution in interview.
I have an array of radius and I have to calculate area using these radius and store in an array.

First Approach:
```js
const radius = [1, 2, 3, 4];
const calculateArea = function(radius) {
    const output = [];
    for (let i = 0; i < radius.length; i++) {
        output.push(Math.PI * radius[i] * radius[i]);
    } 
    return output;
}
console.log(calculateArea(radius));
```
The above solution works perfectly fine but what if we have now requirement to calculate array of circumference. Code now be like
```js
const radius = [1, 2, 3, 4];
const calculateCircumference = function(radius) {
    const output = [];
    for (let i = 0; i < radius.length; i++) {
        output.push(2 * Math.PI * radius[i]);
    } 
    return output;
}
console.log(calculateCircumference(radius));
```
But over here we are violating some principle like DRY Principle, now lets observe the better approach.
```js
const radiusArr = [1, 2, 3, 4];

// logic to calculate area
const area = function (radius) {
    return Math.PI * radius * radius;
}

// logic to calculate circumference
const circumference = function (radius) {
    return 2 * Math.PI * radius;
}

const calculate = function(radiusArr, operation) {
    const output = [];
    for (let i = 0; i < radiusArr.length; i++) {
        output.push(operation(radiusArr[i]));
    } 
    return output;
}
console.log(calculate(radiusArr, area));
console.log(calculate(radiusArr, circumference));
// Over here calculate is HOF
// Over here we have extracted logic into separate functions. This is the beauty of functional programming.

Polyfill of map
// Over here calculate is nothing but polyfill of map function
// console.log(radiusArr.map(area)) == console.log(calculate(radiusArr, area));

***************************************************
Lets convert above calculate function as map function and try to use. So,

Array.prototype.calculate = function(operation) {
    const output = [];
    for (let i = 0; i < this.length; i++) {
        output.push(operation(this[i]));
    } 
    return output;
}
console.log(radiusArr.calculate(area))
```




<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=HkWxvB1RJq0&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/HkWxvB1RJq0/0.jpg" width="750"
alt="Higher-Order Functions ft. Functional Programming in JS Youtube Link"/></a>
