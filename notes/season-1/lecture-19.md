# Episode 19: Mastering `map`, `filter`, & `reduce`

### Overview
In JavaScript, `map`, `filter`, and `reduce` are essential higher-order functions. They allow you to perform complex operations on arrays in a clean, efficient, and readable manner. Understanding these functions will significantly improve your ability to write functional programming code in JavaScript.

### What are Higher-Order Functions?
Higher-order functions are functions that either take one or more functions as arguments or return a function as a result. These are powerful because they allow you to abstract and generalize common patterns in your code.

---

## `map` Function

### Purpose:
The `map` function is used to transform each element in an array according to a specified transformation function. It returns a new array containing the transformed elements, leaving the original array unchanged.

### How It Works:
```js
const output = arr.map(function);
// This transformation function tells `map` how to transform each element of the array.
```

### Example 1: Doubling Array Elements
```js
const arr = [5, 1, 3, 2, 6];

// Function to double each element
function double(x) {
  return x * 2;
}

const doubleArr = arr.map(double); // `map` will apply `double` to each element
console.log(doubleArr); // Output: [10, 2, 6, 4, 12]
```

### Example 2: Tripling Array Elements
```js
const arr = [5, 1, 3, 2, 6];

// Function to triple each element
function triple(x) {
  return x * 3;
}

const tripleArr = arr.map(triple);
console.log(tripleArr); // Output: [15, 3, 9, 6, 18]
```

### Example 3: Converting Elements to Binary
```js
const arr = [5, 1, 3, 2, 6];

// Function to convert elements to binary
function binary(x) {
  return x.toString(2);
}

const binaryArr = arr.map(binary);
console.log(binaryArr); // Output: ["101", "1", "11", "10", "110"]
```

### Takeaway:
The `map` function is like a translator that converts each element in an array into something else, creating a new array with the transformed elements.

---

## `filter` Function

### Purpose:
The `filter` function is used to filter elements in an array based on a condition. It returns a new array with elements that satisfy the specified condition.

### How It Works:
```js
const filteredArray = arr.filter(function);
// The function specifies the condition that each element needs to meet to be included in the new array.
```

### Example: Filtering Odd Values
```js
const array = [5, 1, 3, 2, 6];

// Function to check if a number is odd
function isOdd(x) {
  return x % 2 !== 0;
}

const oddArr = array.filter(isOdd);
console.log(oddArr); // Output: [5, 1, 3]
```

### Takeaway:
The `filter` function is like a sieve that only allows elements meeting a certain condition to pass through, forming a new array.

---

## `reduce` Function

### Purpose:
The `reduce` function is used to reduce an entire array to a single value by accumulating results as it iterates through the array.

### How It Works:
```js
const result = arr.reduce(function(accumulator, current), initialValue);
// `accumulator` is the accumulated result from previous iterations.
// `current` is the current element being processed.
```

### Example 1: Calculating the Sum of an Array
```js
const array = [5, 1, 3, 2, 6];

// Non-functional approach:
function findSum(arr) {
  let sum = 0;
  for (let i = 0; i < arr.length; i++) {
    sum += arr[i];
  }
  return sum;
}
console.log(findSum(array)); // Output: 17

// Using `reduce`:
const sumOfElem = array.reduce((accumulator, current) => {
  return accumulator + current;
}, 0);
console.log(sumOfElem); // Output: 17
```

### Example 2: Finding the Maximum Value in an Array
```js
const array = [5, 1, 3, 2, 6];

// Non-functional approach:
function findMax(arr) {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];
    }
  }
  return max;
}
console.log(findMax(array)); // Output: 6

// Using `reduce`:
const maxElem = array.reduce((max, current) => {
  return current > max ? current : max;
}, 0);
console.log(maxElem); // Output: 6
```

### Takeaway:
The `reduce` function is like a snowball rolling down a hill, accumulating more snow as it progresses, eventually forming a large snowball that represents the single output value.

---

## Tricky `map` Usage

### Example: Creating an Array of Full Names
```js
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 },
];

// Combine first and last names into full names
const fullNameArr = users.map((user) => user.firstName + " " + user.lastName);
console.log(fullNameArr); // Output: ["Alok Raj", "Ashish Kumar", "Ankit Roy", "Pranav Mukherjee"]
```

### Example: Counting People by Age
```js
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 },
];

// Count how many users have each age
const ageReport = users.reduce((acc, curr) => {
  if (acc[curr.age]) {
    acc[curr.age]++;
  } else {
    acc[curr.age] = 1;
  }
  return acc;
}, {});
console.log(ageReport); // Output: {23: 1, 29: 2, 50: 1}
```

---

## Function Chaining

Function chaining is a powerful technique where multiple functions are called in sequence on an array. 

### Example: Filtering and Mapping Together
```js
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 },
];

// Get first names of users who are younger than 30
const youngUserFirstNames = users
  .filter((user) => user.age < 30)
  .map((user) => user.firstName);
console.log(youngUserFirstNames); // Output: ["Alok", "Ashish", "Ankit"]
```

### Challenge: Implement the Above Using `reduce`
```js
const output = users.reduce((acc, curr) => {
  if (curr.age < 30) {
    acc.push(curr.firstName);
  }
  return acc;
}, []);
console.log(output); // Output: ["Alok", "Ashish", "Ankit"]
```

### Additional Points:
1. **Immutability**: `map`, `filter`, and `reduce` do not modify the original array. Instead, they create new arrays or values, which helps prevent bugs in your code.
2. **Pure Functions**: When using `map`, `filter`, and `reduce`, the functions you pass in should ideally be pure functions, meaning they don’t have side effects and return the same output given the same input.
3. **Declarative vs Imperative**: These higher-order functions allow for more declarative code, where you describe *what* you want to do, rather than *how* to do it, making your code more readable and expressive.

---

By mastering `map`, `filter`, and `reduce`, you'll be equipped with powerful tools to process and transform data in JavaScript, making your code more concise, readable, and maintainable.

---

Watch Live On YouTube below:

<a href="https://www.youtube.com/watch?v=zdp0zrpKzIE&list=PLlasXeu85E9cQ32gLCvAvr9vNaUccPVNP" target="_blank"><img src="https://img.youtube.com/vi/zdp0zrpKzIE/0.jpg" width="750"
alt="map, filter & reduce YouTube Link"/></a>

------


Let's dive deeper into the `reduce` function, breaking down the code and explaining it step by step.

### Understanding `reduce`

The `reduce` function is a powerful array method in JavaScript that allows you to "reduce" an array to a single value. It does this by executing a reducer function on each element of the array, passing the result of the previous calculation to the next one.

Here’s the basic syntax of `reduce`:

```javascript
arr.reduce((accumulator, currentValue, currentIndex, array) => {
  // operation to perform on each element
}, initialValue);
```

- **`accumulator`**: This is the accumulated value that results from the reduction process. It gets updated on each iteration.
- **`currentValue`**: The current element being processed in the array.
- **`currentIndex`**: (Optional) The index of the current element.
- **`array`**: (Optional) The array on which `reduce` was called.
- **`initialValue`**: The initial value of the accumulator. If not provided, the first element of the array is used as the initial value, and `reduce` starts from the second element.

### Example 1: Summing an Array

Let's start with a simple example where we sum all elements in an array using `reduce`.

```javascript
const array = [5, 1, 3, 2, 6];

const sum = array.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 0);

console.log(sum); // Output: 17
```

#### Breaking it Down

1. **Initialization**: 
   - `accumulator` starts with the `initialValue`, which is `0` in this case.
   - `currentValue` starts with the first element of the array, which is `5`.

2. **First Iteration**:
   - `accumulator` = 0
   - `currentValue` = 5
   - `accumulator + currentValue` = 0 + 5 = 5
   - `accumulator` is now updated to `5`.

3. **Second Iteration**:
   - `accumulator` = 5
   - `currentValue` = 1
   - `accumulator + currentValue` = 5 + 1 = 6
   - `accumulator` is now updated to `6`.

4. **Third Iteration**:
   - `accumulator` = 6
   - `currentValue` = 3
   - `accumulator + currentValue` = 6 + 3 = 9
   - `accumulator` is now updated to `9`.

5. **Fourth Iteration**:
   - `accumulator` = 9
   - `currentValue` = 2
   - `accumulator + currentValue` = 9 + 2 = 11
   - `accumulator` is now updated to `11`.

6. **Fifth Iteration**:
   - `accumulator` = 11
   - `currentValue` = 6
   - `accumulator + currentValue` = 11 + 6 = 17
   - `accumulator` is now updated to `17`.

7. **End**: 
   - The `reduce` function completes, and the final `accumulator` value is `17`, which is returned and logged.

### Example 2: Finding the Maximum Value in an Array

Now, let's use `reduce` to find the maximum value in an array:

```javascript
const array = [5, 1, 3, 2, 6];

const max = array.reduce((accumulator, currentValue) => {
  if (currentValue > accumulator) {
    accumulator = currentValue;
  }
  return accumulator;
}, 0);

console.log(max); // Output: 6
```

#### Breaking it Down

1. **Initialization**:
   - `accumulator` starts with the `initialValue`, which is `0`.
   - `currentValue` starts with the first element of the array, which is `5`.

2. **First Iteration**:
   - `accumulator` = 0
   - `currentValue` = 5
   - Since `5 > 0`, `accumulator` is updated to `5`.

3. **Second Iteration**:
   - `accumulator` = 5
   - `currentValue` = 1
   - Since `1 < 5`, `accumulator` remains `5`.

4. **Third Iteration**:
   - `accumulator` = 5
   - `currentValue` = 3
   - Since `3 < 5`, `accumulator` remains `5`.

5. **Fourth Iteration**:
   - `accumulator` = 5
   - `currentValue` = 2
   - Since `2 < 5`, `accumulator` remains `5`.

6. **Fifth Iteration**:
   - `accumulator` = 5
   - `currentValue` = 6
   - Since `6 > 5`, `accumulator` is updated to `6`.

7. **End**:
   - The `reduce` function completes, and the final `accumulator` value is `6`, which is returned and logged.

### Example 3: Counting Unique Values

Let’s say we have an array of objects representing users, and we want to count how many users belong to each age group using `reduce`.

```javascript
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 },
];

const ageCount = users.reduce((acc, curr) => {
  if (acc[curr.age]) {
    acc[curr.age] += 1;
  } else {
    acc[curr.age] = 1;
  }
  return acc;
}, {});

console.log(ageCount); // Output: { '23': 1, '29': 2, '50': 1 }
```

#### Breaking it Down

1. **Initialization**:
   - `accumulator` starts as an empty object `{}`.
   - `currentValue` starts with the first user object `{ firstName: "Alok", lastName: "Raj", age: 23 }`.

2. **First Iteration**:
   - `accumulator` = `{}` (empty)
   - `currentValue` = `23`
   - Since `23` is not in `accumulator`, it is added with a count of `1`.

3. **Second Iteration**:
   - `accumulator` = `{ '23': 1 }`
   - `currentValue` = `29`
   - Since `29` is not in `accumulator`, it is added with a count of `1`.

4. **Third Iteration**:
   - `accumulator` = `{ '23': 1, '29': 1 }`
   - `currentValue` = `29`
   - Since `29` is already in `accumulator`, the count is incremented to `2`.

5. **Fourth Iteration**:
   - `accumulator` = `{ '23': 1, '29': 2 }`
   - `currentValue` = `50`
   - Since `50` is not in `accumulator`, it is added with a count of `1`.

6. **End**:
   - The `reduce` function completes, and the final `accumulator` value is `{ '23': 1, '29': 2, '50': 1 }`, which is returned and logged.

### Key Points to Remember

- **`reduce` is versatile**: It can be used for various tasks like summing, finding the maximum, counting, and much more.
- **The accumulator**: Think of the accumulator as a rolling result that you build upon with each iteration.
- **Initial value**: Always set an initial value for the accumulator to avoid unexpected behavior, especially when working with empty arrays.

### Practical Tip

Whenever you use `reduce`, take a moment to think about what you want to accumulate and how you want to initialize the accumulator. This will help you structure your `reduce` function correctly and avoid common pitfalls.


------

# Episode 19 : map, filter & reduce

> map, filter & reducer are Higher Order Functions.

## Map function

It is basically used to transform a array. The map() method creates a new array with the results of calling a function for every array element.

const output = arr.map(_function_) // this _function_ tells map that what transformation I want on each element of array

```js
const arr = [5, 1, 3, 2, 6];
// Task 1: Double the array element: [10, 2, 6, 4, 12]
function double(x) {
  return x * 2;
}
const doubleArr = arr.map(double); // Internally map will run double function for each element of array and create a new array and returns it.
console.log(doubleArr); // [10, 2, 6, 4, 12]
```

```js
// Task 2: Triple the array element
const arr = [5, 1, 3, 2, 6];
// Transformation logic
function triple(x) {
  return x * 3;
}
const tripleArr = arr.map(triple);
console.log(tripleArr); // [15, 3, 9, 6, 18]
```

```js
// Task 3: Convert array elements to binary
const arr = [5, 1, 3, 2, 6];
// Transformation logic:
function binary(x) {
	return x.toString(2);
}
const binaryArr = arr.map(binary);

// The above code can be rewritten as :
const binaryArr = arr.map(function binary(x) {
	return x.toString(2);
}

// OR -> Arrow function
const binaryArr = arr.map((x) => x.toString(2));
```

So basically map function is mapping each and every value and transforming it based on given condition.

## Filter function

Filter function is basically used to filter the value inside an array. The arr.filter() method is used to create a new array from a given array consisting of only those elements from the given array which satisfy a condition set by the argument method.

```js
const array = [5, 1, 3, 2, 6];
// filter odd values
function isOdd(x) {
  return x % 2;
}
const oddArr = array.filter(isOdd); // [5,1,3]

// Other way of writing the above:
const oddArr = arr.filter((x) => x % 2);
```

Filter function creates an array and store only those values which evaluates to true.

## Reduce function

It is a function which take all the values of array and gives a single output of it. It reduces the array to give a single output.

```js
const array = [5, 1, 3, 2, 6];
// Calculate sum of elements of array - Non functional programming way
function findSum(arr) {
  let sum = 0;
  for (let i = 0; i < arr.length; i++) {
    sum = sum + arr[i];
  }
  return sum;
}
console.log(findSum(array)); // 17

// reduce function way
const sumOfElem = arr.reduce(function (accumulator, current) {
  // current represent the value of array
  // accumulator is used the result from element of array.
  // In comparison to previous code snippet, *sum* variable is *accumulator* and *arr[i]* is *current*
  accumulator = accumulator + current;
  return accumulator;
}, 0); //In above example sum was initialized with 0, so over here accumulator also needs to be initialized, so the second argument to reduce function represent the initialization value.
console.log(sumOfElem); // 17
```

```js
// find max inside array: Non functional programming way:
const array = [5, 1, 3, 2, 6];
function findMax(arr) {
    let max = 0;
    for(let i = 0; i < arr.length; i++ {
        if (arr[i] > max) {
            max = arr[i]
        }
    }
    return max;
}
console.log(findMax(array)); // 6

// using reduce
const output = arr.reduce((acc, current) => {
	if (current > acc ) {
		acc = current;
	}
	return acc;
}, 0);
console.log(output); // 6

// acc is just a label which represent the accumulated value till now,
// so we can also label it as max in this case
const output = arr.reduce((max, current) => {
	if (current > max) {
		max= current;
	}
	return max;
}, 0);
console.log(output); // 6
```

## Tricky MAP

```js
const users = [
	{ firstName: "Alok", lastName: "Raj", age: 23 },
	{ firstName: "Ashish", lastName: "Kumar", age: 29 },
	{ firstName: "Ankit", lastName: "Roy", age: 29 },
	{ firstName: "Pranav", lastName: "Mukherjee", age: 50 },
];
// Get array of full name : ["Alok Raj", "Ashish Kumar", ...]
const fullNameArr = users.map((user) => user.firstName + " " + user.lastName);
console.log(fullNameArr); // ["Alok Raj", "Ashish Kumar", ...]

----------------------------------------------------------

// Get the count/report of how many unique people with unique age are there
// like: {29 : 2, 75 : 1, 50 : 1}
// We should use reduce, why? we want to deduce some information from the array. Basically we want to get a single object as output
const report = users.reduce((acc, curr) => {
	if(acc[curr.age]) {
		acc[curr.age] = ++ acc[curr.age] ;
	} else {
		acc[curr.age] = 1;
	}

	return acc;  //to every time return update object
}, {})
console.log(report) // {29 : 2, 75 : 1, 50 : 1}
```

## Function Chaining

```js
// First name of all people whose age is less than 30
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 },
];

// function chaining
const output = users
  .filter((user) => user.age < 30)
  .map((user) => user.firstName);
console.log(output); // ["Alok", "Ashish", "Ankit"]

// Homework challenge: Implement the same logic using reduce
const output = users.reduce((acc, curr) => {
  if (curr.age < 30) {
    acc.push(curr.firstName);
  }
  return acc;
}, []);
console.log(output); // ["Alok", "Ashish", "Ankit"]
```

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=zdp0zrpKzIE&list=PLlasXeu85E9cQ32gLCvAvr9vNaUccPVNP" target="_blank"><img src="https://img.youtube.com/vi/zdp0zrpKzIE/0.jpg" width="750"
alt="map, filter & reduce Youtube Link"/></a>
