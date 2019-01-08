# learningWeb

## Javascript/ES6, DOM

### ECMAScript 6
ECMAScript 6 is also known as ES6 and ECMAScript 2015. Some people like to call it JavaScript 6.

New features in ES6:

#### Block-Scoped Declarations

* let and const
  
  [w3c let and const](https://www.w3schools.com/js/js_let.asp)

  ES2015 introduced two important new JavaScript keywords: let and const.

   These two keywords provide Block Scope variables (and constants) in JavaScript.

   Before ES2015, JavaScript had only two types of scope: Global Scope and Function Scope. 

  The reason to introduce this two keywords is that `var` has a few issues:
   1. Variables declared with the var keyword can not have Block Scope.
      ```
      { 
         var x = 2; 
      }
      // x CAN be used here
      ```
   2. Redeclaring a variable using the var keyword can impose problems.
      ```
      var x = 10;
      // Here x is 10
      { 
         var x = 2;
         // Here x is 2
      }
      // Here x is 2
      ```

      Redeclaring a variable using the let keyword can solve this problem.

      Redeclaring a variable inside a block will not redeclare the variable outside the block:
      ```
      var x = 10;
      // Here x is 10
      { 
         let x = 2;
         // Here x is 2
      }
      // Here x is 10
      ```

   3. Variables defined with var are hoisted to the top, means you can use a variable before it is declared:
      ```
      // you CAN use carName here
      var carName;
      ```
      ```
      // you can NOT use carName here
      let carName;
      ```

      let and const can solve the issues. const is read-only after its initial value is set.
      this works:
      ```
      const a = {};
      a.foo = foo;
      const b = [];
      b.push(1);
      ```
   

* Default parameter values

   ```
   function myFunction(x, y = 10) 
   {
      // y is 10 if not passed or undefined
      return x + y;
   }
   myFunction(5); // will return 15
   myFunction(5,1); // will return 6
   ```


* Arrow functions
  ```
  const x = (x, y) => { return x * y };
  ```
  TODO: this
  Arrow functions do not have their own this. They are not well suited for defining object methods.

   Arrow functions are not hoisted. They must be defined before they are used.

   Using const is safer than using var, because a function expression is always constant value.

* Rest and spread operator
* Destructuring
* For..of Loops
* Object Literal Extensions

* Array.find(), Array.findIndex()
  
   The find() method returns the value of the first array element that passes a `test` function.

   This example finds (returns the value of ) the first element that is larger than 18:
   ```
   var numbers = [4, 9, 16, 25, 29];
   var first = numbers.find(myFunction);

   function myFunction(value, index, array) {
   return value > 18;
   }
   ```
   Note that the function takes 3 arguments: The item value, The item index, The array itself.
  
* New global methods
  
  NaN: The global NaN property is a value representing Not-A-Number.

  The global isFinite() method returns false if the argument is Infinity or NaN.

   The global isNaN() method returns true if the argument is NaN. Otherwise it returns false.


### Javascript
* closure
* this
* Hoisting
* asynchronous
* promises and await (performance)
* prototype
* design pattern

## CSS3, Bootstrap, UI Library, Responsive web design

## Graph Library

## Cross-browser compatibility

## React, React Native (Mobile H5)

## Node.js, MVC, MongoDB, backend integration

## Performance, security, internet protocol

## Testing tool, test driven

## SCSS, SASS, Webpack, env build, Git, CI/CD, AWS, Docker

## Agile experience

## Open source community exposure, open source freamwork code

## Algorithm

## Design Pattern
* Facade Pattern
* 面向对象有三个要素：封装（Encapsulation）、继承（Inheritance）和多态（Polymorphism）


## Interview Oriented
1. [39 Best Object Oriented JavaScript Interview Questions and Answers](https://www.code-sample.com/2015/04/javascript-interview-questions-answers.html)
2. [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
3. [The Modern Javascript Tutorial](https://javascript.info/)