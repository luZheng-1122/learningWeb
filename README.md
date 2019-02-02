# learningWeb

## Javascript/ES6, DOM

### ECMAScript 6
ECMAScript 6 is also known as ES6 and ECMAScript 2015. Some people like to call it JavaScript 6.

New features in ES6:

#### Block-Scoped Declarations

let and const

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


#### Default parameter values

   ```
   function myFunction(x, y = 10) 
   {
      // y is 10 if not passed or undefined
      return x + y;
   }
   myFunction(5); // will return 15

   myFunction(5,undefined); // undefined equal to not pass, will return 15
   myFunction(5, null); // null equal to pass y = 0, will return 5

   myFunction(5,1); // will return 6
   
   ```


#### Arrow functions
   ```
  const x = (x, y) => { return x * y };
   ```

   Arrow functions are not hoisted. They must be defined before they are used.

   Using const is safer than using var, because a function expression is always constant value.

   `this`: in arrow function, `this` is not change.
   ```
   var controller = {
	makeRequest: function(..){
		var self = this;

		btn.addEventListener( "click", function(){
			// ..
			   self.makeRequest(..);
		   }, false );
	   }
   };
   ```
   ```
   var controller = {
	makeRequest: function(..){
		btn.addEventListener( "click", () => {
			   // ..
            this.makeRequest(..);
         }, false );
      }
   };
   ```


   arrow functions is not a method for saving keystrokes, it is not appropriate for every situations:
   So now we can conclude a more nuanced set of rules for when => is appropriate and not:

   * If you have a short, single-statement inline function expression, where the only statement is a return of some computed value, and that function doesn't already make a this reference inside it, and there's no self-reference (recursion, event binding/unbinding), and you don't reasonably expect the function to ever be that way, you can probably safely refactor it to be an => arrow function.
   * If you have an inner function expression that's relying on a var self = this hack or a .bind(this) call on it in the enclosing function to ensure proper this binding, that inner function expression can probably safely become an => arrow function.
   * If you have an inner function expression that's relying on something like var args = Array.prototype.slice.call(arguments) in the enclosing function to make a lexical copy of arguments, that inner function expression can probably safely become an => arrow function.
   * For everything else -- normal function declarations, longer multistatement function expressions, functions that need a lexical name identifier self-reference (recursion, etc.), and any other function that doesn't fit the previous characteristics -- you should probably avoid => function syntax.


#### Rest and spread operator
   ```
   function foo(x,y,z) {
      console.log( x, y, z );
   }

   foo( ...[1,2,3] );				// 1 2 3
   ```

#### Destructuring
   In `pre-ES6`:  Manually assigning indexed values from an array or properties from an object can be thought of as structured assignment. 
   ```
   array assignment:
   function foo() {
	   return [1,2,3];
   }
   var tmp = foo(),
      a = tmp[0], b = tmp[1], c = tmp[2];
   

   object assignment:
   function bar() {
      return {
         x: 4,
         y: 5,
         z: 6
         };
   }
   var tmp = bar(),
	x = tmp.x, y = tmp.y, z = tmp.z;
   ```

   In `ES6`: ES6 adds a dedicated syntax for destructuring, specifically array destructuring and object destructuring. This syntax eliminates the need for the tmp variable in the previous snippets, making them much cleaner.
   ```
   const [ a, b, c ] = foo();
   const { x, y, z } = bar();

   console.log( a, b, c );				// 1 2 3
   console.log( x, y, z );				// 4 5 6
   ```

#### For..of Loops
   in `pre-ES6`, we can use `forEach` to iterate an array:
   ```
   myArray.forEach(function (value) {
      console.log(value);
   });
   ```
   But the issue is that you can’t break out of this loop using a break statement or return from the enclosing function using a return statement.

   We can also use `for-in` loop to iterate an array:
   ```
   for (var index in myArray) {    // don't actually do this
      console.log(myArray[index]);
   }
   ```
   But the drawbacks are: The values assigned to index in this code are the strings "0", "1", "2" and so on, not actual numbers.

   In `ES6`, we can use `for-of` loop to iterate:
   ```
   for (var value of myArray) {
      console.log(value);
   }
   // also work for a string
   for (var c of "hello") {
      console.log( c );
   }
   // "h" "e" "l" "l" "o"
   ```

#### Object Literal Extensions
   Concise Properties:
   ```
   var x = 2, y = 3,
	o = {
		x,
		y
	};
   ```
   Concise Methods:
   ```
   var o = {
      x() {
         // ..
      },
      y() {
         // ..
      }
   }
   ```

#### Template Literals
   ```
   var name = "Kyle";

   var greeting = `Hello ${name}!`;

   console.log( greeting );			// "Hello Kyle!"
   console.log( typeof greeting );		// "string"
   ```

#### Symbols
   With ES6, for the first time in quite a while, a new primitive type has been added to JavaScript: the symbol. Unlike the other primitive types, however, symbols don't have a literal form.

   Here's how you create a symbol:
   ```
   var sym = Symbol( "some optional description" );

   typeof sym;		// "symbol"
   ```

   Symbols could be a good replacement for strings or integers as class/module constants:
   ```
   class Application {
      constructor(mode) {
         switch (mode) {
            case Application.DEV:
            // Set up app for development environment
            break;
            case Application.PROD:
            // Set up app for production environment
            break;
            case default:
            throw new Error('Invalid application mode: ' + mode);
         }
      }
   }

   Application.DEV = Symbol('dev');
   Application.PROD = Symbol('prod');

   // Example use
   const app = new Application(Application.DEV);
   ```



#### Iterator and Generators
A generator can pause itself in mid-execution, and can be resumed either right away or at a later time.
??
```
function *foo() {
	// ..
   yield 10;
}
const it = foo();
it.next();
```

#### Modules
```
function foo(..) {
	// ..
}

export default foo; 
//  you are exporting a binding to the function expression value at that moment, not to the identifier foo. 
//If you later assign foo to a different value inside your module, the module import still reveals the function originally exported, not the new value.

export { foo as default }; 
// In this version of the module export, the default export binding is actually to the foo identifier rather than its value, 
// so you get the previously described binding behavior (i.e., if you later change foo's value, the value seen on the import side will also be updated).
```
```
import foo from "foo";

// or:
import { default as foo } from "foo";
```

#### Classes
At the heart of the new ES6 class mechanism is the class keyword, which identifies a block where the contents define the members of a function's prototype. Consider:
```
class Foo {
	constructor(a,b) {
		this.x = a;
		this.y = b;
	}

	gimmeXY() {
		return this.x * this.y;
	}
}
```
```
class Bar extends Foo {
	constructor(a,b,c) {
		super( a, b );
		this.z = c;
	}

	gimmeXYZ() {
		return super.gimmeXY() * this.z;
	}
}

var b = new Bar( 5, 15, 25 );

b.x;						// 5
b.y;						// 15
b.z;						// 25
b.gimmeXYZ();				// 1875
```
A significant new addition is super, which is actually something not directly possible pre-ES6 (without some unfortunate hack trade-offs). In the constructor, super automatically refers to the "parent constructor," which in the previous example is Foo(..). In a method, it refers to the "parent object," such that you can then make a property/method access off it, such as super.gimmeXY().

#### Array.find(), Array.findIndex()

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



### Javascript

#### Introduction
* ECMAScript: a specification standardized by Ecma international. It was created to standardize Javascript.

* Javascript Engine: Today, JavaScript can execute not only in the browser, but also on the server, or actually on any device that has a special program called the JavaScript engine. Different browser has different engines: e.g. V8 (Chrome and Opera), SpiderMonkey (Firefox).

* JS restrictions: JavaScript’s abilities in the browser are limited for the sake of the user’s safety. The aim is to prevent an evil webpage from accessing private information or harming the user’s data.
  * not read/write arbitrary files on the hard disk, copy them or execute programs. It has no direct access to OS system functions.

    Modern browsers allow it to work with files, but the access is limited and only provided if the user does certain actions, like “dropping” a file into a browser window or selecting it via an `input` tag.

    There are ways to interact with camera/microphone and other devices, but they require a user’s explicit permission. So a JavaScript-enabled page may not sneakily enable a web-camera, observe the surroundings and send the information to the NSA.

  * Different tabs/windows generally do not know about each other. Sometimes they do, for example when one window uses JavaScript to open the other one. But even in this case, JavaScript from one page may not access the other if they come from different sites (from a different domain, protocol or port).

    This is called the “Same Origin Policy”. To work around that, both pages must contain a special JavaScript code that handles data exchange.

    This limitation is, again, for the user’s safety. A page from http://anysite.com which a user has opened must not be able to access another browser tab with the URL http://gmail.com and steal information from there.

   * JavaScript can easily communicate over the net to the server where the current page came from. But its ability to receive data from other sites/domains is crippled. Though possible, it requires explicit agreement (expressed in HTTP headers) from the remote side. Once again, that’s a safety limitation.
   * Such limits do not exist if JavaScript is used outside of the browser, for example on a server. Modern browsers also allow plugin/extensions which may ask for extended permissions.

#### Fundamentals
* The benefit of a separate script file is that the browser will download it and store it in its cache. Other pages that reference the same script will take it from the cache instead of downloading it, so the file is actually downloaded only once. 
* We recommend putting semicolons between statements even if they are separated by newlines.
* always use strict:
   ```
      "use strict";

      // this code works the modern way
      ...
   ```
* There are 7 basic types in JavaScript.
  * `number`
  * `string`
  * `boolean`
  * `null`: for unknown values – a standalone type that has a single value null.
  * `undefined`: or unassigned values – a standalone type that has a single value undefined.
  * `object`
  * `symbol`: for unique identifiers. Enabled by ES6.
  
   The `typeof` operator allows us to see which type is stored in a variable.
   * Two forms: `typeof x` or `typeof(x)`.
   * For null returns `"object"` – this is an error in the language, it’s not actually an object.
   * The result of `typeof alert` is `"function"`, because `alert` is a function of the language. We’ll study functions in the next chapters where we’ll see that there’s no special “function” type in JavaScript. Functions belong to the object type. But `typeof` treats them differently. Formally, it’s incorrect, but very convenient in practice.

* Addition ‘+’ concatenates strings:
  Almost all mathematical operations convert values to numbers. A notable exception is addition `+`. If one of the added values is a string, the other one is also converted to a string. Then, it concatenates (joins) them:

   ```
      alert( 1 + '2' ); // '12' (string to the right)
      alert( '1' + 2 ); // '12' (string to the left)
   ```

   This only happens when at least one of the arguments is a string. Otherwise, values are converted to numbers.

* Operators:
  * String concatenation, binary +:
      ```
         let s = "my" + "string";
         alert(s); // mystring
      ```
   * Remainder %: 5 % 2 // 1
   * Exponentiation **: 
      2 ** 4 // result is 16. `a` multiplied by itself `b` times
      4 ** (1/2) // result is 2. power of 1/2 is the same as a square root
   * 


* map and forEach
* closure
  https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8
* shallow clone and deep clone
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
* Ant design

## Node.js, MVC, MongoDB, backend integration

## Performance, security, internet protocol

## Testing tool, test driven

## Dev tools
* React/redux dev tools

## Third-party library
* [Async]()
* [Moment.js]()

## SCSS, SASS, Webpack, env build, Git, CI/CD, AWS, Docker

## Agile experience

## Open source community exposure, open source freamwork code
* [HyperLedger Caliper](https://github.com/hyperledger/caliper)

## Algorithm

## Network protocol

## Design Pattern
* Facade Pattern
* 面向对象有三个要素：封装（Encapsulation）、继承（Inheritance）和多态（Polymorphism）


## Interview Oriented
1. [39 Best Object Oriented JavaScript Interview Questions and Answers](https://www.code-sample.com/2015/04/javascript-interview-questions-answers.html)
2. [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
3. [The Modern Javascript Tutorial](https://javascript.info/)