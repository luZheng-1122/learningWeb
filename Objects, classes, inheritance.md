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
* Array to store ordered data collections,
* Date to store the information about the date and time,
* Error to store the information about an error.
* …And so on.


## Object to primitive conversion

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
