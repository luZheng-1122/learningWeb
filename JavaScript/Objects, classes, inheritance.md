# Objects, classes, inheritance

A property is a “key: value” pair, where key is a `string` or a `symbol type`, and value can be anything.

We can only use strings or symbols as keys in objects. Other types are converted to strings. For instance, a number `0` becomes a string `"0"` when used as a property key.

## Objects

An empty object (“empty cabinet”) can be created using one of two syntaxes:
```
let user = new Object(); // "object constructor" syntax
let user = {};  // "object literal" syntax
```

### Literals
To remove a property:
```
delete user.age
```

Using multiword property name:
```
let user = {
   "likes birds": true
}
```

access multiword property with square brackets:
```
user["likes birds"] = false;
```

computed property:
```
const key = name
let user = {
   [key]: lu,
   [key + 'Computers']: 5 // user.nameComputers = 5
}

user[key] = 'an'
```

* So most of the time, when property names are known and simple, the dot is used. And if we need something more complex, then we switch to square brackets.
### Existence check
Existence check, a special operator `in` to check for the existence of a property.
```
let user = { name: "John", age: 30 };
alert( "age" in user ); // true, user.age exists
alert( "blabla" in user ); // false, user.blabla doesn't exist
```

Using "in" for properties that store `undefined`:
```
let obj = {
   test: undefined
};

alert( obj.test ); // it's undefined, so - no such property?

alert( "test" in obj ); // true, the property does exist!
```

### for...in
object is iterable, so we can walk over all keys of an object:
```
for(let key in object) {
   // executes the body for each key among object properties
}
```

### Copying by reference
copying an object is copying its reference. 
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

### Comparison by reference
`==` convert the type if different and compare the value, `===` compare the types and the value, the value for an object variable is a reference. So:
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

### Const object
Const object: An object declared as const can be changed.
```
const user = {
   name: "John"
};

user.age = 25; // no problem

user = {
   name: "Pete" // Error can't reassign user
}
```
What if we want to make constant object properties? So that user.age = 25 would give an error. 
* give a `false` to its `writable` property

### Clone and merging, Object.assign
Create an independent copy, a clone
1. if all the properties are primitive type: Object.assign() can clone and merge
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

There are many other kinds of objects in JavaScript:
* `Array` to store ordered data collections,
* `Date` to store the information about the date and time,
* `Error` to store the information about an error.
* …And so on.


## Object to primitive conversion

The object-to-primitive conversion is called automatically by many built-in functions and operators that expect a primitive as a value.

There are 3 types (hints) of it:

1. "string" (for alert and other string conversions)
2. "number" (for maths)
3. "default" (few operators)

The specification describes explicitly which operator uses which hint. There are very few operators that “don’t know what to expect” and use the "default" hint. Usually for built-in objects "default" hint is handled the same way as "number", so in practice the last two are often merged together.

The conversion algorithm is:

1. Call obj[Symbol.toPrimitive](hint) if the method exists,
2. Otherwise if hint is "string"
* try obj.toString() and obj.valueOf(), whatever exists.
3. Otherwise if hint is "number" or "default"
* try obj.valueOf() and obj.toString(), whatever exists.

In practice, it’s often enough to implement only obj.toString() as a “catch-all” method for all conversions that return a “human-readable” representation of an object, for logging or debugging purposes.


## Constructor, operator "new"
### Constructor function
This type of function is used to create an object.

```
function User(name) {
   this.name = name;
   this.isAdmin = false;
}

let user = new User("Jack");
```

When a function is executed as new User(...), it does the following steps:
1. A new empty object is created and assigned to this.
2. The function body executes. Usually it modifies this, adds new properties to it.
3. The value of this is returned.

```
function User(name) {
   // this = {};  (implicitly)

   // add properties to this
   this.name = name;
   this.isAdmin = false;

   // return this;  (implicitly)
}
```

Let’s note once again – technically, any function can be used as a constructor. That is: any function can be run with new, and it will execute the algorithm above. The “capital letter first” is a common agreement, to make it clear that a function is to be run with new.

### Return from constructors
Usually, constructors do not have a return statement. Their task is to write all necessary stuff into this, and it automatically becomes the result.

But if there is a return statement, then the rule is simple:
* If return is called with object, then it is returned instead of this.
* If return is called with a primitive, it’s ignored.

### Methods in constructor
Of course, we can add to this not only properties, but methods as well.
```
function User(name) {
   this.name = name;

   this.sayHi = function() {
      alert( "My name is: " + this.name );
   };
}
```

## Property flags and descriptors
Object properties, besides a value, have three special attributes (so-called “flags”):

* writable – if true, can be changed, otherwise it’s read-only.
* enumerable – if true, then listed in loops, otherwise not listed.
* configurable – if true, the property can be deleted and these attributes can be modified, otherwise not.

two functions:
* Object.getOwnPropertyDescriptor(obj, propertyName)
* Object.defineProperty(obj, propertyName, descriptor)

copy all properties:
```
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(user));
```

## Property getters and setters

There are two kinds of properties. The first kind is data properties. 

The second type of properties is something new. It’s accessor properties. They are essentially functions that work on getting and setting a value, but look like regular properties to an external code.

```
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
};

// set fullName is executed with the given value.
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper
```

## Prototypal inheritance
* In JavaScript, all objects have a `hidden [[Prototype]] property` that’s either another object or null.
* We can use obj.__proto__ to access it (there are other ways too, to be covered soon). That’s a getter/setter for [[Prototype]].
* The object referenced by [[Prototype]] is called a “prototype”.
* If we want to read a property of obj or call a method, and it doesn’t exist, then JavaScript tries to find it in the prototype. `Write/delete` operations work directly on the object, they don’t use the prototype (unless the property is actually a setter).
* If we call obj.method(), and the method is taken from the prototype, this still references obj. So methods always work with the current object even if they are inherited.

## F.prototype
When you want to create an object with a constructor function, you can use constructor function's `prototype` property. After giving an object to this property of a constructor function, every time you create an object with this function, the created object will automatically inheritance from the object that stores in constructor function's `prototype`.
```
let animal = {
   eats: true
};

function Rabbit(name) {
   this.name = name;
}

Rabbit.prototype = animal;
let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal
alert( rabbit.eats ); // true
```

On regular objects the prototype is nothing special:
```
let user = {
   name: "John",
   prototype: "Bla-bla" // no magic at all
};
```

By default all functions have F.prototype = { constructor: F }, so we can get the constructor of an object by accessing its "constructor" property.

## Native prototype
`obj = new Object()`, where `Object` is a built-in object constructor function, and there is a built-in `Object.prototype` which stores many built-in methods, such as toString(). So every created obj will has `obj.__proto__ == Object.prototype` and can use these built-in functions.

* All built-in objects follow the same pattern:
   * The methods are stored in the prototype (Array.prototype, Object.prototype, Date.prototype etc).
   * The object itself stores only the data (array items, object properties, the date).
* Primitives also store methods in prototypes of wrapper objects: Number.prototype, String.prototype, Boolean.prototype. There are no wrapper objects only for undefined and null.
* Built-in prototypes can be modified or populated with new methods. But it’s not recommended to change them. Probably the only allowable cause is when we add-in a new standard, but not yet supported by the engine JavaScript method.

## Methods for prototypes

The __proto__ is not a property of an object, but an accessor property of Object.prototype. So, if obj.__proto__ is read or assigned, the corresponding getter/setter is called from its prototype, and it gets/sets [[Prototype]]. As it was said in the beginning: __proto__ is a way to access [[Prototype]], it is not [[Prototype]] itself.

* Object.create(proto[, descriptors]) – creates an empty object with given proto as [[Prototype]] (can be null) and optional property descriptors.
* obj.hasOwnProperty(key): it returns true if obj has its own (not inherited) property named key.

## Class patterns
The term “class” comes from the object-oriented programming. In JavaScript it usually means the `functional class pattern` or `the prototypal pattern`. `The prototypal pattern` is more powerful and memory-efficient, so it’s recommended to stick to it.

prototyple pattern:
```
// Same Animal as before
function Animal(name) {
  this.name = name;
}

// All animals can eat, right?
Animal.prototype.eat = function() {
  alert(`${this.name} eats.`);
};

// Same Rabbit as before
function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype.jump = function() {
  alert(`${this.name} jumps!`);
};

// setup the inheritance chain
Rabbit.prototype.__proto__ = Animal.prototype; // (*)

let rabbit = new Rabbit("White Rabbit");
rabbit.eat(); // rabbits can eat too
rabbit.jump();
```

**`Rabbit.prototype` is a normal object, setup its inheritance should use `__proto__`**

## Class syntax
The basic class syntax:
```
class MyClass {
  constructor(...) {
    // ...
  }
  method1(...) {}
  method2(...) {}
  get something(...) {}
  set something(...) {}
  static staticMethod(..) {}
  // ...
}
```
### default constructor
The value of `MyClass` is a function provided as constructor. If there’s no constructor, then an empty function.

### static function
In any case, methods listed in the class declaration become members of its `prototype`, with the exception of `static methods` that are written into the function itself and callable as `MyClass.staticMethod()`. Static methods are used when we need a function bound to a class, but not to any object of that class.

Static function will not be added to MyClass.prototype. Meaning that the static function cannot be inheritanced by any instance created by this class. Only the class itself can use this static function.

### Private and protected properties and methods
* Protected fields start with _. That’s a well-known convention, not enforced at the language level. Programmers should only access a 
field starting with _ from its class and classes inheriting from it.
* Private fields start with #. Javascript makes sure we only can access those from inside the class.

## Class inheritance, super
### super

* super.method(...) to call a parent method.
* super(...) to call a parent constructor (inside our constructor only).

```
let animal = {
  name: "Animal",
  eat() {         // [[HomeObject]] == animal
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  name: "Rabbit",
  eat() {         // [[HomeObject]] == rabbit
    super.eat();
  }
};

let longEar = {
  __proto__: rabbit,
  name: "Long Ear",
  eat() {         // [[HomeObject]] == longEar
    super.eat();
  }
};

longEar.eat();  // Long Ear eats.
```
**`super` represent the [[HomeObject]].__proto__**

**`this` only represent the object before dot**

### Static methods and inheritance
```
Class Deer extends Animal{}
```
does two things:
1. Deer.__proto__ = Animal
2. Deer.prototype.__proto__ = Animal.prototype

So static function of Animal can be inheritanced by Deer.

## Class instanceof
The `instanceof` operator allows to check whether an object belongs to a certain class. It also takes inheritance into account.

|             | work for                                                      | returns    |
| ----------- | ------------------------------------------------------------- | ---------- |
| typeof      | primitives                                                    | string     |
| {}.toString | primitives, built-in objects, objects with Symbol.toStringTag | string     |
| instanceof  | objects                                                       | true/false |


## Mixins
a `mixin` is a class that contains methods for use by other classes without having to be the parent class of those other classes.

Some other languages like e.g. python allow to create mixins using multiple inheritance. JavaScript does not support multiple inheritance, but mixins can be implemented by copying them into the prototype.

We can use mixins as a way to augment a class by multiple behaviors, like event-handling as we have seen above.

Mixins may become a point of conflict if they occasionally overwrite native class methods. So generally one should think well about the naming for a mixin, to minimize such possibility.