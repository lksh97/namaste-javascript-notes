# Episode 5 : Shortest JS Program, window & this keyword

* The shortest JS program is `empty file`. 
    * Because even then, JS engine does a lot of things. As always, <ins>even in this case, it creates the GEC</ins> which has memory space and the execution context.

* JS engine creates something known as `window`. 
    * It is an object, which is created in the global space.
        * It contains lots of functions and variables.
        * These functions and variables can be accessed from anywhere in the program.
* JS engine also creates a `this` keyword, which points to the `window object` at the <ins>global level</ins>. 

* In **summary**
    * <ins>`Along with GEC, a global object (window) and a this variable are created`</ins>.

* In different engines, the name of global object changes. `Window in browsers`, but <ins>in nodeJS it is called something else. At global level, this === window</ins>

* If we create any <ins><i>variable in the global scope</i></ins>, then the <ins><i>variables get attached to the global object</i></ins>.

eg:

```js
var x = 10;
console.log(x); // 10
console.log(this.x); // 10
console.log(window.x); // 10
```

<hr>

Watch Live On Youtube below:

<a href="https://www.youtube.com/watch?v=QCRpVw2KXf8&ab_channel=AkshaySaini" target="_blank"><img src="https://img.youtube.com/vi/QCRpVw2KXf8/0.jpg" width="750"
alt="Shortest JS Program, window & this keyword Youtube Link"/></a>