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