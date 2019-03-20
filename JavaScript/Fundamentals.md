# ECMAScript 6
ECMAScript 6 is also known as ES6 and ECMAScript 2015. Some people like to call it JavaScript 6.

ECMA compatibility table: https://kangax.github.io/compat-table/es6/

New features in ES6:

## Block-Scoped Declarations

let and const: [w3c let and const](https://www.w3schools.com/js/js_let.asp)

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

   **Redeclaring a variable using the let keyword can solve this problem.**
   
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

   **let and const can solve the issues. const is read-only after its initial value is set.**
   
   this works:
   ```
   const a = {};
   a.foo = foo;
   const b = [];
   b.push(1);
   ```


## Default parameter values

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


## Arrow functions
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

   * If you have a short, single-statement inline function expression, where the only statement is a return of some computed value, and that function doesn't already make a this reference inside it, and there's no self-reference (recursion, event binding/unbinding), and you don't reasonably expect the function to ever be that way, you can probably safely refactor it to be an => arrow function.
   * If you have an inner function expression that's relying on a var self = this hack or a .bind(this) call on it in the enclosing function to ensure proper this binding, that inner function expression can probably safely become an => arrow function.
   * If you have an inner function expression that's relying on something like var args = Array.prototype.slice.call(arguments) in the enclosing function to make a lexical copy of arguments, that inner function expression can probably safely become an => arrow function.
   * For everything else -- normal function declarations, longer multistatement function expressions, functions that need a lexical name identifier self-reference (recursion, etc.), and any other function that doesn't fit the previous characteristics -- you should probably avoid => function syntax.


## Rest and spread operator
```
function foo(x,y,z) {
   console.log( x, y, z );
}

foo( ...[1,2,3] );				// 1 2 3
```

## Destructuring
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

## For..of Loops
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

## Object Literal Extensions
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

## Template Literals
```
var name = "Kyle";

var greeting = `Hello ${name}!`;

console.log( greeting );			// "Hello Kyle!"
console.log( typeof greeting );		// "string"
```

## Symbols
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

In an object, using symbol we can create more than one key:value pair with the same key name. they are hidden from each other.
```
let user = { name: "John" };
let id = Symbol("id");

user[id] = "ID Value";

// Imagine that another script wants to have its own “id” property inside user, for its own purposes. That may be another JavaScript library, so the scripts are completely unaware of each other.

let id = Symbol("id");
user[id] = "Their id value";
```
TODO:
QA: what is the case of more than one script share with a same object with same name symbol property?

Symbols have two main use cases:

1. “Hidden” object properties. If we want to add a property into an object that “belongs” to another script or a library, we can create a symbol and use it as a property key. A symbolic property does not appear in for..in, so it won’t be occasionally listed. Also it won’t be accessed directly, because another script does not have our symbol, so it will not occasionally intervene into its actions.

So we can “covertly” hide something into objects that we need, but others should not see, using symbolic properties.

2. There are many system symbols used by JavaScript which are accessible as Symbol.*. We can use them to alter some built-in behaviors. For instance, later in the tutorial we’ll use Symbol.iterator for iterables, Symbol.toPrimitive to setup object-to-primitive conversion and so on.

Technically, symbols are not 100% hidden. There is a built-in method Object.getOwnPropertySymbols(obj) that allows us to get all symbols. Also there is a method named Reflect.ownKeys(obj) that returns all keys of an object including symbolic ones. So they are not really hidden. But most libraries, built-in methods and syntax constructs adhere to a common agreement that they are. And the one who explicitly calls the aforementioned methods probably understands well what he’s doing.

## Iterator and Generators
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

## Modules
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

## Classes
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

## Array.find(), Array.findIndex()

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



# Javascript Language

## Introduction
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

## Fundamentals

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
   * `==` regular equality check has a problem, it cannot differentiate 0 from false:
      ```
         alert( 0 == false ); // true, `false` is converted to number `0`
      ```
      The same thing happens with an empty string:
      ```
         alert( '' == false ); // true, `false` and `''` are both converted to number `0`
      ```
      This happens because operands of different types are converted to numbers by the equality operator ==. An empty string, just like false, becomes a zero. 

      **A strict equality operator === checks the equality without type conversion.**

      `==` converts the operands if they are not of the same type, then applies strict comparison. If both operands are objects, then JavaScript compares internal references which are equal when operands refer to the same object in memory:
      ```
         var object1 = {'key': 'value'}, object2 = {'key': 'value'}; 
         object1 == object2 //false, since the internal reference is different
         object1 === object2 //false

         // Use `Object.is()` to determines whether two values are the same value.
         Object.is(object1,object2) // false
         Object.is(object1,object1) // true
      ```

      **Compare two objects**
      1. use `_.isEqual()` in [lodash](https://lodash.com/docs#isEqual).
      2. reinvent the wheel: https://gomakethings.com/check-if-two-arrays-or-objects-are-equal-with-javascript/



* browser-specific functions to interact with visitors: `alert`, `prompt` and `confirm`.
   
   The mini-window with the message is called a `modal window`. The word `modal` means that the visitor can’t interact with the rest of the page, press other buttons, etc. until they have dealt with the window.

* logical operators:
   * `||` OR
      OR finds the `first truthy` value:
      ```
         result = value1 || value2 || value3;
         
         // The OR || operator does the following:
         // 1. Evaluates operands from left to right.
         // 2. For each operand, converts it to boolean. If the result is true, stops and returns the original value of that operand.
         // 3. If all operands have been evaluated (i.e. all were false), returns the last operand.
      ```
      usage:
      1. Getting the first truthy value from a list of variables or expressions.
      ```
         let currentUser = null;
         let defaultUser = "John";

         let name = currentUser || defaultUser || "unnamed";

         alert( name ); // selects "John" – the first truthy value
      ```
      2. Short-circuit evaluation.
      ```
         let x;
         false || (x = 1);
         alert(x); // 1

         // in React
         <dialog title={isFailed || <FormattedMessage id="ADD_SUCCESS" defaultMessage="Added" />} />
      ```
   * `&&` AND
      AND finds the `first falsy` value.
      ```
         result = value1 && value2 && value3;

         // The AND && operator does the following:
         // 1. Evaluates operands from left to right.
         // 2. For each operand, converts it to a boolean. If the result is false, stops and returns the original value of that operand.
         // 3. If all operands have been evaluated (i.e. all were truthy), returns the last operand.
      ```
      usage:
      1. Short-circuit evaluation.
      ```
         let x = 1;
         (x > 0) && alert( 'Greater than zero!' );

         // if url exist, href equals to the later variable
         href = url && `${process.env.REACT_APP_BIGCHAINDB_URL}${url}`

         // if data contains keys and href exist, button equals to the FlatButton.
         // if any of the two values are false, the button equals to false, meaning not showing on the page
         button = (Object.keys(data).length && href && <FlatButton />
      ```

* Loop: 
   A single execution of the loop body is called an `iteration`.
   creating an infinite loop:
   ```
      for (;;) {
         // repeats without limits
      }
   ```
   Labels for break/continue:
   ```
      outer: for (let i = 0; i < 3; i++) {

      for (let j = 0; j < 3; j++) {

         let input = prompt(`Value at coords (${i},${j})`, '');

         // if an empty string or canceled, then break out of both loops
         if (!input) break outer; // (*) looks upwards for the label named outer and breaks out of that loop.

         // do something with the value...
      }
      }
      alert('Done!');
   ```

* switch:
   Let’s emphasize that the equality check is always strict. The values must be of the same type to match.

* Functions
   * Parameters
      ```
         function showMessage(from, text) { // arguments: from, text
            alert(from + ': ' + text);
         }

         showMessage('Ann', 'Hello!'); // Ann: Hello! (*)
         showMessage('Ann', "What's up?"); // Ann: What's up? (**)
      ```
      When the function is called in lines (*) and (**), the given values are copied to local variables from and text. Then the function uses them. When the function changes the from and text, it only changes the local copy value, the change is not seen outside.
   * Default values:
      default value can be a string or a more complex expression, which is only evaluated and assigned if the parameter is missing:
      ```
         function showMessage(from, text = anotherFunction()) {
            // anotherFunction() only executed if no text given
            // its result becomes the value of text
         }
         function anotherFunction(){
            return 'default text';
         }
      ```
   * Naming a function
      For instance:
      ```
         "get…" – return a value,
         "calc…" – calculate something,
         "create…" – create something,
         "check…" – check something and return a boolean, etc.
      ```
      **one function, one action**
   * Function Expression VS Function Declaration
      1. Function Declaration:
      ```
         function sum(a, b) {
            return a + b;
         }
      ```
      **A Function Declaration is usable in the whole script/code block.**
      In other words, when JavaScript prepares to run the script or a code block, it first looks for Function Declarations in it and creates the functions. We can think of it as an “initialization stage”.

      And after all of the Function Declarations are processed, the execution goes on.

      As a result, a function declared as a Function Declaration can be called earlier than it is defined.
      2. Function Expression
      ```
         let sum = function(a, b) {
            return a + b;
         };
      ```
      **A Function Expression is created when the execution reaches it and is usable from then on.**

      When we need to create a function, the first to consider is Function Declaration syntax. But if a Function Declaration does not suit us for some reason (we’ve seen an example above), then Function Expression should be used.

   * Arrow functions
      Arrow function can simplyfy Function Expression:
      ```
         let sum = (a, b) => a + b; // one line function, two argument
         let double = n => n * 2; // one line function, one argument
         let sayHi = () => alert("Hello!"); // one line function, zero argument
      ```

## Code quality

### Debugger
[Debugging in Chrome](https://javascript.info/debugging-chrome)

Debugger command, we can pause the code by using the `debugger` command:
```
   function hello(name) {
      let phrase = `Hello, ${name}!`;

      debugger;  // <-- the debugger stops here

      say(phrase);
   }
```
### Coding style
1. [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
2. [StandardJS](https://standardjs.com/)

### Good comments
1. Avoid descibing what is does, since the good code should be self-descriptive
2. Describe the architecture
3. Document a function usage: [JSDoc](https://github.com/jsdoc3/jsdoc)
4. Why is the task solved this way?
5. Any subtle features of the code? Where they are used?

### Testing
* Behaviour Driven Development(BDD): the spec goes first, followed by implementation. At the end we have both the spec and the code. The spec can be used in three ways:

1. `Tests` guarantee that the code works correctly.
2. `Docs` – the titles of describe and it tell what the function does.
3. `Examples` – the tests are actually working examples showing how a function can be used.

* Testing tools:
1. [Mocha](https://mochajs.org/) – the core framework: it provides common testing functions including describe and it and the main function that runs tests.
2. [Chai](https://www.chaijs.com/) – the library with many assertions. It allows to use a lot of different assertions, for now we need only assert.equal.

### Polyfills
When we use modern features of the language, some engines may fail to support such code. Here Babel comes to the rescue. Babel is a transpiler. It rewrites modern JavaScript code into the previous standard.

Actually, there are two parts in Babel:
1. the transpiler program, which rewrites the code into the older standard. And then the code is delivered to the website for users. Modern project build system like `webpack` or `brunch` provide means to run transpiler automatically on every code change, so that doesn’t involve any time loss from our side.
2. the polyfill. There’s a term “polyfill” for scripts that “fill in” the gap and add missing implementations. 
```
// Polyfill provided by MDN for Object.is
if (!Object.is) {
   Object.is = function(x, y) {
      // SameValue algorithm
      ...
   };
}
```


## Object
### Square brackets notation:
```
let user = {
   name: "John",
   age: 30
};

let key = prompt("What do you want to know about the user?", "name");

// access by variable
alert( user[key] ); // John (if enter "name")

// when the key is multiword preperty
user["likes birds"] = true;

// computed properties
let fruit = 'apple';
let bag = {
   [fruit + 'Computers']: 5 // bag.appleComputers = 5
};
```

### remove a property:
```
delete user.age;
```

### Existence check, a special operator `in` to check for the existence of a property.
```
let user = { name: "John", age: 30 };
alert( "age" in user ); // true, user.age exists
alert( "blabla" in user ); // false, user.blabla doesn't exist

// using a variable as the tested key
let user = { age: 30 };
let key = "age";
alert( key in user ); // true, takes the name from key and checks for such property
```

### copying an object is copying its reference. 
```
let user = { name: 'John' };
```
The value of `user` is the reference (address) of the real object. So copying an object is copying its reference. If change any variable will change the object:
```
let user = { name: 'John' };

let admin = user; // copy the reference

admin.name = 'Pete'; // changed by the "admin" reference

alert(user.name); // 'Pete', changes are seen from the "user" reference
```

### Duplicate object: 
Create an independent copy, a clone
1. if all the properties are primitive type: Object.assign()
```
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// copies all properties from permissions1 and permissions2 into user
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
```
2. if some properties are objects: _.cloneDeep()
```
const objects = [{ 'a': 1 }, { 'b': 2 }];

const deep = _.cloneDeep(objects);
console.log(deep[0] === objects[0]);
// => false
```

###`==` convert the type if different and compare the value, `===` compare the types and the value, the value for an object variable is a reference. So:
```
admin == user // true
admin === user // true
```
Two independent objects are not equal, even though both are empty:
```
let a = {};
let b = {}; // two independent objects

alert( a == b ); // false
```

### Const object: An object declared as const can be changed.
```
const user = {
   name: "John"
};

user.age = 25; // no problem

user = {
   name: "Pete" // Error can't reassign user
}
```
What if we want to make constant object properties? So that user.age = 25 would give an error. [Property flags and descriptors](https://javascript.info/property-descriptors)

### this
`this`: It’s common that an object method needs to access the information stored in the object to do its job. To access the object, a method can use the this keyword.
#### function defined in an object can use a shorthand method:
```
// these objects do the same

let user = {
sayHi: function() {
   alert("Hello");
}
};

// method shorthand looks better, right?
let user = {
sayHi() { // same as "sayHi: function()"
   alert("Hello");
}
};
```

#### why we need to have `this`.
The following code will error
```
let user = {
   name: "John",
   age: 30,

   sayHi() {
      alert( user.name ); // leads to an error
   }

};

let admin = user;
user = null; // overwrite to make things obvious

admin.sayHi(); // Whoops! inside sayHi(), the old name is used! error!
```
if we use `this` to replace the `user` above:
```
let user = {
   name: "John",
   age: 30,

   sayHi() {
      alert(this.name);
   }
};

user.sayHi(); // John
```

#### `this` is not bound, the value of `this` is evaluated during the run-time.
```
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
   alert( this.name );
}

// use the same functions in two objects
user.f = sayHi;
admin.f = sayHi;

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)
``` 
* Reference Type: 
To make user.hi() calls work, JavaScript uses a trick – the dot '.' returns not a function, but a value of the special Reference Type.

The value of Reference Type is a three-value combination `(base, name, strict)`, where:
   * `base` is the object.
   * `name` is the property.
   * `strict` is true if use strict is in effect.

The result of a property access user.hi is not a function, but a value of Reference Type. For user.hi in strict mode it is: 
```
// Reference Type value
(user, "hi", true)
```
When parentheses () are called on the Reference Type, they receive the full information about the object and its method, and can set the right `this` (=user in this case).

Any other operation like assignment `hi = user.hi` discards the reference type as a whole, takes the value of `user.hi` (a function) and passes it on. So any further operation “loses” `this`:
```
// split getting and calling the method in two lines
let hi = user.hi;
hi(); // Error, because this is undefined
```
So, as the result, the value of `this` is only passed the right way if the function is called directly using a dot `obj.method()` or square brackets `obj['method']()` syntax (they do the same here). Later in this tutorial, we will learn various ways to solve this problem such as `func.bind()`.

#### Arrow functions have no `this`
Arrow functions are special: they don’t have their “own” this. If we reference this from such a function, it’s taken from the outer “normal” function.
* Chaining: to make functions chainable
```
let ladder = {
   step: 0,
   up() {
      this.step++;
      return this;
   },
   down() {
      this.step--;
      return this;
   },
   showStep() {
      alert( this.step );
      return this;
   }
}

ladder.up().up().down().up().down().showStep(); // 1
```

#### Constructor, operator "new"
Constructor functions technically are regular functions. 
```
function User(name) {
   this.name = name;
   this.isAdmin = false;
}

let user = new User("Jack");

alert(user.name); // Jack
alert(user.isAdmin); // false
```

* pass an object as a parameter to a function, the local parameter will copy the reference so if the function changes the object, the outer object will change accordingly. Is it a safe manner?
* how to check empty array?


## Data Type
### Methods of primitives: primitives can provide methods, but they still remain lightweight.
```
let str = "Hello";
alert( str.toUpperCase() ); // HELLO
```
in the above example, when call a function of the primitive `string`, a special “object wrapper” is created that provides the extra functionality, and then is destroyed.
**null/undefined have no methods**
**primitive cannot store data**

### Number
* `e` to write a number:
   ```
   let billion = 1000000000;
   let billion = 1e9;  // 1 billion, literally: 1 and 9 zeroes

   let ms = 0.000001;
   let ms = 1e-6; // six zeroes to the left from 1
   ```
* toString(base): The base can vary from 2 to 36. By default it’s 10.
* parseInt and parseFloat: They “read” a number from a string until they can’t. In case of an error, the gathered number is returned. if the first character is not a number, then return `NaN`
   ```
   alert( parseInt('100.5px') ); // 100
   alert( parseFloat('12.5em') ); // 12.5
   ```
* [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

### String
* Accessing characters
   ```
   let str = `Hello`;
   // the first character
   alert( str[0] ); // H, modern way
   alert( str.charAt(0) ); // H, historical use

   // can also iterate over characters
   for (let char of "Hello") {
      alert(char); // H,e,l,l,o (char becomes "H", then "e", then "l" etc)
   }
   ```
* searching for a substring
   `str.indexOf`: 
   ```
   let str = 'Widget with id';

   alert( str.indexOf('Widget') ); // 0, because 'Widget' is found at the beginning
   alert( str.indexOf('widget') ); // -1, not found, the search is case-sensitive

   alert( str.indexOf("id") ); // 1, "id" is found at the position 1 (..idget with id)
   ```
   `includes, startsWith, endsWith`
   ```
   alert( "Widget with id".includes("Widget") ); // true
   alert( "Hello".includes("Bye") ); // false

   alert( "Widget".startsWith("Wid") ); // true, "Widget" starts with "Wid"
   alert( "Widget".endsWith("get") );   // true, "Widget" ends with "get"
   ```
* Getting a substring
   `substring, substr and slice.`
   The author finds themself using `slice` almost all the time.

### Array
* Methods that work with the end of the array:
   ```
   let fruits = ["Apple", "Orange", "Pear"];
   alert( fruits.pop() ); // remove "Pear" and alert it
   // Apple, Orange

   fruits.push("Pear");
   // Apple, Orange, Pear
   ```
* Methods that work with the beginning of the array:
   ```
   alert( fruits.shift() ); // remove Apple and alert it
   // Orange, Pear

   fruits.unshift('Apple');
   // Apple, Orange, Pear
   ```
* loop:
   ```
   let fruits = ["Apple", "Orange", "Plum"];

   // iterates over array elements
   for (let fruit of fruits) {
      alert( fruit );
   }
   ```
* the simplest way to clear the array is: arr.length = 0;.
* array methods:
   * `splice`: The `arr.splice(str)` method is a swiss army knife for arrays. It can do everything: add, remove and insert elements. `arr.splice(index[, deleteCount, elem1, ..., elemN])`. Negative indexes allowed: 
   ```
   let arr = [1, 2, 5];

   // from index -1 (one step from the end)
   // delete 0 elements,
   // then insert 3 and 4
   arr.splice(-1, 0, 3, 4);

   alert( arr ); // 1,2,3,4,5
   ```
   * `slice`: It returns a new array containing all items from index "start" to "end" (not including "end"). Both start and end can be negative, in that case position from array end is assumed. `arr.slice(start, end)`
   * `concat`:  joins the array with other arrays and/or items. `arr.concat(arg1, arg2...)`
   * `forEach`: allows to run a function for every element of the array.
   ```
   ["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
      alert(`${item} is at index ${index} in ${array}`);
   });
   ```
   * `indexOf` and `include`
   * `find` and `findIndex`: find `an element` with the specific condition.
   ```
   let users = [
      {id: 1, name: "John"},
      {id: 2, name: "Pete"},
      {id: 3, name: "Mary"}
   ];

   let user = users.find(item => item.id == 1);
   // if true is returned, item is returned and iteration is stopped, so only find the first one which is true
   // for falsy returns undefiend
   alert(user.name); // John
   ``` 
   * `filter`: The `find` method looks for a single (first) element that makes the function return true. If there may be many, we can use `arr.filter(fn)`.
   ```
   let users = [
      {id: 1, name: "John"},
      {id: 2, name: "Pete"},
      {id: 3, name: "Mary"}
   ];

   // returns array of the first two users
   let someUsers = users.filter(item => item.id < 3);

   alert(someUsers.length); // 2
   ``` 

   **Transform an array:**
   * `map`: use it when you want to transform the elements in old array to new elements and return with a new array:
   ```
   var array1 = [1, 4, 9, 16];

   // pass a function to map
   const map1 = array1.map(x => x * 2);

   console.log(map1);
   // expected output: Array [2, 8, 18, 32]
   ``` 
   * `arr.sort(fn)`: we can compare any type of element in the `arr`, all we need is implement the `fn` as the ordering function that knows how to compare the elements. The default is a **string order**.
   * `split`: split the string into an array by the given delimiter.
   ```
   let names = 'Bilbo, Gandalf, Nazgul';
   let arr = names.split(', ');
   for (let name of arr) {
      alert( `A message to ${name}.` ); // A message to Bilbo  (and other names)
   }
   ``` 
   * `join`: does the reverse to `split`. creates a string of `arr` items glued by `separator` between them.
   * `reduce/reduceRight`: 
   When we need to iterate over an array – we can use `forEach, for or for..of`.

   When we need to iterate and return the data for each element – we can use `map`.

   The methods `arr.reduce` and `arr.reduceRight` also belong to that breed, but are a little bit more intricate. They are used to calculate a single value based on the array. 
   ```
   let arr = [1, 2, 3, 4, 5];

   let result = arr.reduce((accumulator, current) => accumulator + current, 0); // 0 is initial value, always indicate the intitial value for safety

   alert(result); // 15
   ```
* Iterables: to make an uniterable object iterable, implement `iterator` function to this object.
   * Iterables and array-likes:
   * Iterables are objects that implement the Symbol.iterator method, as described above.
   * Array-likes are objects that have indexes and length, so they look like arrays.
   * Array.from:
   ```
   let arrayLike = {
      0: "Hello",
      1: "World",
      length: 2
   };

   let arr = Array.from(arrayLike); // (*)
   alert(arr.pop()); // World (method works)
   ``` 
### Map, Set, weakMap and weakSet:
   
* Map – is a collection of keyed values
   The differences from a regular Object:
      * Any keys, objects can be keys.
      * Iterates in the insertion order.
      * Additional convenient methods, the size property.

* Set – is a collection of unique values.
      * Unlike an array, does not allow to reorder elements.
      * Keeps the insertion order.

Collections that allow garbage-collection:

* WeakMap – a variant of Map that allows only objects as keys and removes them once they become inaccessible by other means.
   It does not support operations on the structure as a whole: no size, no clear(), no iterations.

* WeakSet – is a variant of Set that only stores objects and removes them once they become inaccessible by other means.
   Also does not support size/clear() and iterations.

WeakMap and WeakSet are used as “secondary” data structures in addition to the “main” object storage. Once the object is removed from the main storage, if it is only found in the WeakMap/WeakSet, it will be cleaned up automatically.

* Object.keys(), Object.values(), Object.entries()

* Destructuring assignment
  * array destructuring
  ```
   let arr = ["Ilya", "Kantor"]

   // destructuring assignment with array
   let [firstName, surname] = arr;

   // work for any iterables:
   let [a, b, c] = "abc"; // ["a", "b", "c"]
   let [one, two, three] = new Set([1, 2, 3]);

   // default values
   let [name = "Guest", surname = "Anonymous"] = ["Julius"];
  ```
  * object destructuring
  ```
   let options = {
      title: "Menu",
      width: 100,
      height: 200
   };
   // name needs to match, order doesn't matter
   let {title, width, height} = options;

   // change name and default value:
   let {width: w = 100, height: h = 200, title} = options;
  ``` 
  * smart function parameters
   ```
   // we pass object to function
   let options = {
      title: "My menu",
      items: ["Item1", "Item2"]
   };

   // ...and it immediately expands it to variables
   function showMenu({title = "Untitled", width = 200, height = 100, items = []}) {}
   showMenu(options);
   ``` 



### Date and Time
* `UTC`: Coordinated Universal Time
   `GMT`: Greenwich Mean Time
   they are equal

* The number of milliseconds that has passed since the beginning of 1970 is called a **timestamp**.

`new Date()`: return local time zone.
`new Date("2019-02-16")`: it assume the time you input `2019-02-16` is midnight GMT time, then it converts the UTC time to your local time zone and returns.
`new Date(2019,1,1,14,30,02,666)`: `new Date(year, month, date, hours, minutes, seconds, ms)`, returns the exact time that you input and assume it is the local time zone: `Fri Feb 01 2019 14:30:02 GMT+1100 (Australian Eastern Daylight Time)`. **Note that month starts from 0(Jan) to 11(Dec).**
`localdate.getHours` and `localdate.getUTCHours`.
`localdate.getTime`: returns the timestamp for the date.
`localdate.getTimezoneOffset`: returns the difference between the local time zone and UTC, in minutes.

* Autocorrection: The autocorrection is a very handy feature of Date objects. We can set out-of-range values, and it will auto-adjust itself. for example:
```
   let date = new Date(2013, 0, 32); // 32 Jan 2013 ?!?
   alert(date); // ...is 1st Feb 2013!

   let date = new Date(2016, 1, 28);
   date.setDate(date.getDate() + 2);

   alert( date ); // 1 Mar 2016
``` 

* Date to number:
```
   let start = new Date(); // start counting

   // do the job
   for (let i = 0; i < 100000; i++) {
   let doSomething = i * i * i;
   }

   let end = new Date(); // done

   alert( `The loop took ${end - start} ms` ); // same as end.getTime() - start.getTime(). But slower than getTime since need type conversion first
``` 
* Date.now(): good for measure the time difference, It is semantically equivalent to new Date().getTime(), but it doesn’t create an intermediate Date object. So it’s faster and doesn’t put pressure on garbage collection.
```
   let start = Date.now(); // milliseconds count from 1 Jan 1970

   // do the job
   for (let i = 0; i < 100000; i++) {
   let doSomething = i * i * i;
   }

   let end = Date.now(); // done

   alert( `The loop took ${end - start} ms` ); // subtract numbers, not dates
``` 
* Date.parse(str): The string format should be: `YYYY-MM-DDTHH:mm:ss.sssZ`, where:
   `T`: a delimiter
   `Z`: it is optional, denotes the time zone in the format +-hh:mm. A single letter `Z` that would mean UTC+0. `-07:00` means toward 7 more hours, `+07:00` means backward 7 hours.



### JSON (JavaScript Object Notation): 
initially was made for JavaScript, but many other languages have libraries to handle it as well.
* replacer: 
The full syntax of `JSON.stringify` is: JSON.stringify(value[, replacer, space]). replacer is an Array of properties to encode or a mapping function function(key, value).

1. encode all keys in an array and pass as replacer, so that the JSON.stringify will only return the keys you want.
```
let room = {
   number: 23
};

let meetup = {
   title: "Conference",
   participants: [{name: "John"}, {name: "Alice"}],
   place: room // meetup references room
};

room.occupiedBy = meetup; // room references meetup

alert( JSON.stringify(meetup, ['title', 'participants', 'place', 'name', 'number']) );
/*
{
   "title":"Conference",
   "participants":[{"name":"John"},{"name":"Alice"}],
   "place":{"number":23}
}
*/
``` 
3. you can also pass a mapping function as replacer to convert any value and stringify/parse the converted new value.
```
let schedule = `{
"meetups": [
   {"title":"Conference","date":"2017-11-30T12:00:00.000Z"},
   {"title":"Birthday","date":"2017-04-18T12:00:00.000Z"}
]
}`;

schedule = JSON.parse(schedule, function(key, value) {
   if (key == 'date') return new Date(value);
   return value;
});

alert( schedule.meetups[1].date.getDate() ); // works!
```