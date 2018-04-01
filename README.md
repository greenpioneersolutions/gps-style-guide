
<h1 align="center">
  <a href="http://standardjs.com"><img src="https://avatars1.githubusercontent.com/u/17859884?v=3&s=200" alt="JavaScript Style Guide - JavaScript Standard Style" width="200"></a>
  <br>
  GPS Style Guide
  <br>
  <br>
</h1>

<h4 align="center">Green Pioneer Solutions Style Guide to all things we develop</h4>

<br>


[![GitHub version](https://badge.fury.io/gh/emiloberg%2Fmarkdown-styleguide-generator.svg)](http://badge.fury.io//gre%2Fmarkdown-styleguide-generator)







## Table of Contents

- [Intro](#intro)
	- [Variables](#variables-speech_balloon)
	- [Constants](#constants-triangular_flag_on_post)
    - [Globals](#globals-earth_africa)
- [Style](#style)
- [Common Mistakes](#common-mistakes)
- [Tools](#tools)
- [Versions](#versions)
- [Blogs](#blogs)
- [Github](#github)
- [More Resources](#resources)
- [Bootcamps](#bootcamps)
- [Courses](#courses)
- [Mentee](#mentee)
- [Contribute](#contribute)
- [Future](#Future)
- [License](#License)


## Intro

Welcome to all of those who are here reading this. This is the Green Pioneer Solutions style guide to code. We will be write about many bigger than just code and syntax. We will use this guide to help you in any way possible. For example were going to take a dive into other things that make us successful than just style like blogs, mentors, other style guides, mistakes and tools. Thats just to name a few.

Why: We are building this guide to help you figure  

How: we plan to accomplish this by write how style our code and showing you lots

What: you will have a guide to come follow to help you along your journey as a developer

This guide is not a one size fits all. the goal

## Approach

Maybe talk about my approach

## Installs

Installs

## Style

- [Javascript Style](#javascript-style)
- [Mean Stack Style](#mean-stack-style)
- [Angular Style](#angular-style)
- [Express Style](#express-style)

### Javascript Style

Description of the JS

#### SubPoint

Description of the section

```javascript
// Style
```



#### Control Flow

Description of the section

```javascript
// Style
```

#### Lodash

Description of the section

```javascript
// Style
```

#### Be Async

Description of the section

```javascript
// Style
```

#### When to Be Sync

Description of the section

```javascript
// Style
```

#### Promises

Description of the section

```javascript
// Style
```

#### Bind Call Apply

Description of the section

```javascript
// Style
```

#### Avoid polluting the global scope

Declaring variables is a lot of fun. Sometimes, you may declare global variables even if you don't want to. In today's browsers, the global variables are stored in the window object. So, since there's a lot of stuff going on there, you may override default values.

Let's suppose you have an HTML file which contains a `<script>` tag containing (or which loads a JavaScript file containing):

```javascript
var foo = 42;
console.log(foo);
```

This will obviously output 42 in the console. But since the code is not executed in a function, the context will be the global one. So, the variables are attached to the window object. This means that window.foo is 42 as well.

This is dangerous since you can override existing global variables:

```javascript
function print () {
   // do something
}
print();
```

When executing window.print (or just print), it won't open the print popup since we have overriden the native print popup.

The solution is quite simple; we need a wrapping function that is called immediately, like below:

```javascript
// Declare an anonymous function
(function () {
   var foo = 42;
   console.log(window.foo);
   // → undefined
   console.log(foo);
   // → 42
})();
//^ and call it immediately
```

Alternatively, you could choose to send the window and other globals (e.g. document) as arguments to that function (this will probably improve the performance):

```javascript
(function (global, doc) {
  global.setTimeout(function () {
     doc.body.innerHTML = "Hello!";
  }, 1000);
})(window, document);
```

So, do use the wrapping function to prevent creating globals. Note that I'm not going to use the wrapping function in the code snippets below since we want to focus on the code itself.

💡 Tip: browserify is another way to prevent creating globals. It uses the require function the same way you do it in Node.js.

By the way, Node.js does wrap your files in functions automatically. They look like this:

```javascript
(function (exports, require, module, __filename, __dirname) {
// ...
```

So if you thought the require function is a global one, well, it's not. It's nothing more than a function argument.

Did you know?
Since the window object contains the global variables and since it is a global itself, the window references itself inside:

```javascript
window.window.window
// => Window {...}
```
That's because the window object is a circular object. Here's how to create such an object:

```javascript
// Create an object
var foo = {};

// Point a key value to the object itself
foo.bar = foo;

// The `foo` object just became a circular one:
foo.bar.bar.bar.bar
// → foo
```

#### Use Strict

Strictly use `use strict`! This is nothing more than just adding string put in your code that adds more magic to your script.

```javascript
  // This is bad, since you do create a global without having anyone to tell you
  (function () {
     a = 42;
     console.log(a);
     // → 42
  })();
  console.log(a);
  // → 42
```
Using use strict, you can get quite a few more errors:

```javascript
  (function () {
     "use strict";
     a = 42;
     // Error: Uncaught ReferenceError: a is not defined
  })();
```

You could be wondering why you can't put the `use strict` outside of the wrapping function. Well, you can, but it will be applied globally. That's still not bad; but it will affect it if you have code which comes from other libraries, or if you bundle everything in one file.

#### Strict equal

This is short. If you compare a with b using == (like in other programming languages), in JavaScript you may find this works in a weird way: if you have a string and a number, they will be equal (==):

```javascript
"42" == 42
// → true
```

For obvious reasons (e.g. validations), it's better to use strict equal (===):

```javascript
"42" === 42
// → false
```

#### || and &&

Depending on what you need to do, you can make your code shorter using logic operators.

Defaults
```javascript
"" || "foo"
// → "foo"

undefined || 42
// → 42

// Note that if you want to handle 0 there, you need
// to check if a number was provided:
var a = 0;
a || 42
// → 42

// This is a ternary operator—works like an inline if-else statement
var b = typeof a === "number" ? a : 42;
// → 0
```

Instead of checking if something is truly using an if expression, you can simply do:

```javascript
expr && doSomething();

// Instead of:
if (expr) {
   doSomething();
}
```

That's even fancier if you need the result returned by doSomething():

```javascript
function doSomething () {
   return { foo: "bar" };
}
var expr = true;
var res = expr && doSomething();
res && console.log(res);
// → { foo: "bar" }
```

You may not agree with me here, but this is more ideal. If you don't want to uglify your code this way, this is what these JavaScript minifiers will actually do.

If you ask me, though the code is shorter, it is still human-readable.

#### Convert value types

There are several ways to convert these things depending on how you want to do it. The most common ways are:

```javascript
// From anything to a number

var foo = "42";
var myNumber = +foo; // shortcut for Number(foo)
// → 42

// Tip: you can convert it directly into a negative number
var negativeFoo = -foo; // or -Number(foo)
// → -42

// From object to array
// Tip: `arguments` is an object and in general you want to use it as array
var args = { 0: "foo", 1: "bar", length: 2 };
Array.prototype.slice.call(args)
// → [ 'foo', 'bar' ]

// Anything to boolean
/// Non non p is a boolean p
var t = 1;
var f = 0;
!!t
// → true
!!f
// → false

/// And non-p is a boolean non-p
!t
// → false
!f
// → true

// Anything to string
var foo = 42;
"" + foo // shortcut for String(foo)
// → "42"

foo = { hello: "world" };
JSON.stringify(foo);
// → '{ "hello":"world" }'

JSON.stringify(foo, null, 4); // beautify the things
// →
// '{
//    "hello": "world"
// }'

// Note you cannot JSON.stringify circular structures
JSON.stringify(window);
// ⚠ TypeError: JSON.stringify cannot serialize cyclic structures.
```

#### Variable declarations

- [Variables](#variables)
- [Constants](#constants)
- [Globals](#globals)


##### Variables

Using `var` in general or `let` when they should be accesible only in specific blocks (e.g. `if`).

```js
// One declaration
var foo = 1;

// Multiple declarations
var foo = 1
  , bar = "Hello World"
  , anotherOne = [{ foo: "bar" }]
  ;

if (...) {
   let baz = 42;
  /* do something with baz */
}
```
##### Constants

Using `const`. The constant names are written with UPPERCASE letters. I also use `const` when including libraries using `require` and when they should not be changed. In this case, the names will not be with caps.

```js
// Dependencies
const http = require("http")
   , fs = require("fs")
   , EventEmitter = require("events").EventEmitter
// Constants
const PI = Math.PI
    , MY_CONSTANT = 42
    ;
```
##### Globals

I define globals when there is no commonjs environment (this is actually handled by [`dist-it`](https://github.com/IonicaBizau/dist-it). When I manually define globals, I do that using `window.MyGlobal` (on the client) and `global.MyGlobal` (on the server).


#### Iterating object and arrays

For arrays, most of times, I use the `forEach` function:

```js
arr.forEach(c => {
    // do something
});
```

However, using `for` loops is fine too:

```js
for (var i = 0; i < arr.length; ++i) {
    for (var ii = 0; ii < arr[i].length; ++ii) {
        ...
    }
    ...
}
```

For objects, I use the following style:

```js
Object.keys(obj).forEach(k => {
    var cValue = obj[k];
    // do something
});
```

To simplify this, I created [`iterate-object`](https://github.com/IonicaBizau/iterate-object), which abstracts this functionality:

```js
const iterateObject = require("iterate-object");
iterateObject(obj, (value, key) => {
    // do something
});
```


#### Binary and Ternary operators

See examples.

```js
var foo = someObj
    .method()
    .method2()
    .method3()
    ;

var a = cond ? v1 : v2;

var b = long_condition_here
        ? v1 : v2
        ;

var c = another_long_condition_here
        ? with_some_long_value
        : or_another_some_long_value
        ;
```

#### Multiline strings :guitar:

I use backticks to create multiline strings:

```js
var multiLineStr = `Lorem ipsum dolor sit amet, consectetur adipisicing elit
sed do eiusmod tempor incididunt ut labore et dolore magna
aliqua. Ut enim ad minim veniam, quis nostrud exercitation
ullamco laboris nisi ut aliquip ex ea commodo consequat
New line again...`;
```
#### Modifying prototypes of built-in objects :shit:

Just don't, unless that's the scope of the library.

#### Naming things :thought_balloon:

Using camel case notation for variables, in general. For constructors I capitalize the variable name (e.g. `EventEmitter`).

```js
// Node.JS require
const fs = require("fs")
    , events = require("events")
    , EventEmitter = events.EventEmitter
    ;

// Local variables
var x = 1
  , twoWords = "Hello World"
  ;

// Functions
function fooBar () {...}

// Classes
class Person {
    constructor (name, age) {
        this.name = name;
        this.age = age;
    }
    getName () {
        return this.name;
    }
}
// Object fields
var obj = {
    full_name: "Johnny B."
  , age: 20
};
obj.methodA = function () {...};
```
## Curly braces :curly_loop:

Open the curly brace at the end of the line. Always put the instructions between curly braces, even there is only one instruction.

```js
if (expr) {
    instr;
} else {
    instr2;
    instr3;
}
```
## Array and Object Initializers :file_folder:

See examples.

```js
// Arrays
var arr = [1, 2, 3, 4];

var lotOfElms = [
    1, 2, 3, 4, 5, 6, 7
  , 8, 9, 10, 11, 12, 13
  , 14, 15, 16, 17, 18
];

var bigElms = [
    "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod."
  , "Tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim."
  , "Veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea."
  , "Commodo consequat. Duis aute irure dolor in reprehenderit in voluptate"
];

// Objects
var obj = { a: 1 };

var obj1 = {
    full_name: "Johnny B."
  , age: 20
};
```

#### Quotes

Double quotes, with some exceptions when single quotes are used.

```js
var foo = "\"Hello\", he said.";
var jQuerySelector = "div.myClass[data-foo='bar']";
```
#### Comments

##### Use slashes for comments

Use slashes for both single line and multi line comments. Try to write comments that explain higher level mechanisms or clarify difficult segments of your code. Don't use comments to restate trivial things.

Right:
```js
// 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE', 'SOMETHING', 'VALUE']
var matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// This function has a nasty side effect where a failure to increment a
// redis counter used for statistics will cause an exception. This needs
// to be fixed in a later iteration.
function loadUser(id, cb) {
  // ...
}

var isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
  // ...
}
```

Wrong:

```js
// Execute a regex
var matches = item.match(/ID_([^\n]+)=([^\n]+)/);

// Usage: loadUser(5, function() { ... })
function loadUser(id, cb) {
  // ...
}

// Check if the session is valid
var isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
  // ...
}
```

Put relevant comments. The comments start with uppercase letter.


```js
// Dependencies
const lib1 = require("lib1")
    , lib2 = require("lib2")
    ;

// Constants
const FOURTY_TWO = 42;
```
Single line comments start with `//`. For multi-line commands, you use `/* ... */`
```javascript
/* This is a single line comment */
// or
// This is a single line comment

/*
 And this is
 a multi-line
 comment
 */
 
 //or
 
// And this is
// a multi-line
// comment

 ```

#### If

Use `if (expression) { ... } else { ... }` to do something if expression is true or not.

```javascript
let foo = 42;

if (foo > 40) {
    // do something
} else {
    // do something else
}
```

#### Switch

Description of the section

```javascript
let planet = 'Earth';

switch (planet) {
  case "Mercury":
  case "Venus":
    console.log("Too hot here.");

  case 'Earth' :
    console.log("Welcome home!");
    break;

  case 'Mars' :
    console.log("Welcome to the other home!");
    break;

  case "Jupiter":
  case "Saturn":
  case "Uranus":
  case "Neptune":
  case "Pluto":
    console.log("You may get gold here.");
    break;

  default:
    console.log("Seems you found another planet.");
    break;
}
```

#### Primitive values

The following ones, are primitives:

* Booleans: false, true
* Numbers: 42, 3.14, 0b11010, 0x16, NaN (check out The magic of numbers)
* Strings: 'Earth', "Mars"
* Special values: undefined, null

Want to parse them ? try auto parse

#### Objects

The common way to declare objects is by using the curly braces:

```javascript
let myObj = { world: "Earth" };
```

Attention: Objects are compared by reference. That being said, we have this:

```javascript
let firstObj = {};
let secondObj = {};

// Check if they are equal
firstObj === secondObj
→ false

// Comparing an object with itself...
firstObj === firstObj
→ true

// Let's point the secondObj to firstObj
secondObj = firstObj

// Now they are equal
firstObj === secondObj
→ true
```
Attention: you have no guarantee that adding the object keys in a specific order will get them in the same order. Object keys are not ordered, even in general JavaScript interpreters which will iterate them in the order of adding them in the object (again, do not rely on this feature).



#### Arrays

In addition to objects, the array data is ordered by indexes. Arrays are actually objects, having the indexes (numbers from 0 to legnth - 1) as keys.

```javascript
let fruits = ["apples", "pears", "oranges"];

fruits[1]
→ "pears"
```

##### Access, add, remove, and update elements

```javascript
> let fruits = ["Apple"]

// Add to the end of the array
> fruits.push("Pear")
2 // < The new length of the array

 [ 'Apple', 'Pear' ]
 //         ^ This was just added

// Add to the start of the array
> fruits.unshift("Orange")
3 // < The new length of the array

 [ 'Orange', 'Apple', 'Pear' ]
 // ^ This was just added

// Access the element at index 1 (the second element)
> fruits[1]
'Apple'

// How many items do we have?
> fruits.length
3

// Turn the Apple into Lemon
> fruits[1] = "Lemon"
'Lemon'

 [ 'Orange', 'Lemon', 'Pear' ]
 //          ^ This was before an Apple

// Insert at index 1 a new element
> fruits.splice(1, 0, "Grapes")
[] // < Splice is supposed to delete things (see below)
   //   In this case we're deleting 0 elements and
   //   inserting one.

 [ 'Orange', 'Grapes', 'Lemon', 'Pear' ]
 //          ^ This was added.

// Get the Lemon's index
> fruits.indexOf("Lemon")
2

// Delete the Lemon
> fruits.splice(2, 1)
[ 'Lemon' ]

 [ 'Orange', 'Grapes', 'Pear' ]
 //                   ^ Lemon is gone

// Remove the last element
> fruits.pop()
'Pear'

 [ 'Orange', 'Grapes' ]
 //                  ^ Pear is gone

// Remove the first element
> fruits.shift()
'Orange'

   [ 'Grapes' ]
 // ^ Orange is gone
```

##### Iterating over Arrays

There are few ways to loop trough an array.

```javascript
// Using for-loop
var arr = [42, 7, -1]
for (var index = 0; index < arr.length; ++index) {
    var current = arr[index];
    /* Do something with `current` */
}

// Others prefer defining an additional `length` variable:
for (var index = 0, length = arr.length; index < length; ++index) {
    var current = arr[index];
    /* ... */
}

// Another way i using `forEach`:
arr.forEach((current, index, inputArray) => {
    /* Do something with `current` */
});

// Or using the for ... of:
for (let current of arr) {
    /* current will be the current element */
}
```

#### Functions

There are a couple of ways to define functions in JavaScript. The common uses are:

```javascript
function sum (a, b) {
    return a + b;
}

var sum = function (a, b) {
    return a + b;
}

// Using the ES6 arrow functions
let sum = (a, b) => a + b;
```

Then you can call the function like:

```javascript
let result = sum(40, 2);
// => 42
```

#### Constructors and Classes

Description of the section

SUGGEST DESIGN PATTERNS



There are a couple of ways you can obtain a class-like functionality in JavaScript.

Factory functions
Creating an object and returning it.

```javascript
function Person (name) {
   var self = {};

   // Public field
   self.name = name;

   // Public method
   self.getName = function () {
     // `this` is the `self`
     return this.name;
   };

   return self;
}

var p = Person("Alice");
console.log(p.getName());
// => "Alice"
```

Using `prototype`s
By adding a method in the prototype object of a function, you're adding that method as a public method to all the instances of that class.
```javascript
function Person (name) {
  this.name = name;
}

Person.prototype.getName = function () {
  // `this` is the `self`
  return this.name;
};

var p = new Person("Bob");
console.log(p.getName());
// => "Bob"
```

Using ES6 classes

```javascript
class Person {
  constructor (name) {
    this.name = name;
  }
  getName () {
    return this.name;
  }
}

var p = new Person("Carol");
console.log(p.getName());
// => "Bob"
```
It's also very easy to inherit classes in ES6:

```javascript
class Musician extends Person {
   constructor (name, instrument) {
     // Call the base class
     super(name);

     this.instrument = instrument;
   }

   play () {
     console.log(`${this.getName()} is playing ${this.instrument}`);
   }
}

var me = new Musician("Johnny B.", "piano");

// Get the name of the musician, who is also a person
console.log(me.getName());
// => "Johnny B."

me.play();
// => "Johnny B. is playing piano."
```


#### Async vs Sync

Sometimes you have to wait a little bit for things to be done: such as when making a cake, you have to work on it and you have to wait a while for it to be ready. Most familiar things in our lives are asynchronous.

Sync
Usually, synchronous operations send the response directly, using the return keyword:

// Synchronous sum
function sum (a, b) {
  return a + b;
}
var result = sum(40, 2);
// => 42
Async
To have an async function, you need a source of async stuff. In this example, we will use the setTimeout function. It accepts a function as first argument (which is called callback function) and a number as second argument—representing the time delay after the function is called:

function asyncSum (a, b, cb) {
  setTimeout(function () {
    cb(a + b); // -----------+ This is the place
  }, 1000);    //            | where we call then
}              //            V callback function
asyncSum(40, 2, function (result) {
  console.log(result);
  // => 42
});

```javascript
// Style
```

#### What are callbacks?

callbacks are functions which are sent as an argument to another function and are invoked when something happens.

```javascript
function Cake() { /* Just a dummy class-like constructor for now */ }

// We won't make a ckae if we're busy
var busy = false;

function makeCake ( callback) {
  // If we're busy making a cake, send back an error
  if (busy) {
    return callback(new Error("Already busy with creating a cake. Please wait a bit and try again later."));
  }
  // Well, if we weren't busy before, we are now
  busy = true;

  // Wait one second
  setTimeout(function () { // <- This is a callback function too. It is called after one second.

    // After one second we call the callback function
    callback(null, new Cake());

    // After sending the cake, we're not busy anymore
    busy = false;
  }, 1000);
}

makeCake(function (err, cake) {
  if (err) { console.error(err); }
  console.log("Made a cake.");
  // => "Made a cake."
});

// This will end with an error because we're busy already
makeCake(function (err, cake) {
  if (err) { console.error(err); }
  // !=> "Already busy with creating a cake. Please wait a bit and try again later."
  console.log("Made a cake.");
});

// Wait 2 seconds
setTimeout(function () {
  // This will work again
  makeCake(function (err, cake) {
    if (err) { console.error(err); }
    console.log("Made a cake.");
    // => "Made a cake."
  });
}, 2000);
```

#### Promises

There is a thing called Promise. Let's see an example:

```javascript
function sum (a, b) {
   return Promise(function (resolve, reject) {
     // Let's wait a second and only then send the response
     setTimeout(function () {
       if (typeof a !== "number" || typeof b !== "number") {
         return reject(new TypeError("Please provide two numbers to sum."));
       }
       resolve(a + b);
     }, 1000);
   });
}

var myPromise = sum(40, 2);

myPromsise.then(function (result) {
  // After one second we have the response here
  console.log("> Summed 40 + 2: ", result);

  // Let's pass some invalid data and return another promise
  return sum(null, "foo");
}).then(function () {
  // This won't be called because we have an error
}).catch(function (err) {
  // Instead, this `catch` handler is called: (after another second)
  console.error(err);
  // => Please provide two numbers to sum.
});
```

A promise can be in one of these three states:

* pending: the operation is pending
* fulfilled: the operation was finished
* rejected: an error appeared, so the promise is rejected

StackOverflow Documention has a good section about promises here.

#### Create and throw errors

To create an error, you have to use the Error constructor:

```javascript
let myError = new Error("Something went really wrong.");

// You can even append custom data here
myError.code = "MY_FANCY_ERROR_CODE";
```

To throw an error, you have to use the throw statement:

throw new Error("Something bad happened.");
There are few types of errors. For example, if you validate the arguments passed to a function, use TypeError:
```javascript
function sayHello(message) {
  if (typeof message !== "string") {
    throw new TypeError("The message should be a string.");
  }
  console.log(message);
}
```

##### Callbacks
When you have a callback interface it's friendlier to send the errors using the callback functions.

##### Promises
In the Promises (even on the interface) it's friendlier to send the errors using the reject functions.


#### Error handling

In general, it is a good practice to validate the data you're passing to function (especially when they are coming from the user input). This way you can avoid TypeErrors to be thrown.

If there's a function which throws an error by design, you can catch it using try - catch:

```javascript
function assert (truly) {
  if (!truly) {
    throw new Error("Not true.");
  }
}

try {
  // This will *not* throw an error
  assert(42 === 42);

  // This will throw an error, so that will go in the catch
  assert(42 === -1);

  // Everything put after the place where the error was thrown
  // is not executed
} catch (e) {
  console.error(e);
  // => Not true.
}
```

Note if you have an async function (either callback-based or which returns a promise), you'll do something like this:

```javascript
// Callback
fetchDataFromServer(function (err, data) {
  if (err) {
    /* do something if you have an error */
  } else {
    /* Do something with `data` */
  }
});

// Promise
fetchDataFromServer().then(function (data) {
    /* Do something with `data` */
}).catch(function (err) {
    /* do something if you have an error */
});
```

#### Try catch

Description of the section

```javascript
// Style
```

#### Incrementing/decrementing numbers

You can increment a variable name `x` using `++x` or `x++`. Similarly, you can decrement it by `--x` or `x--`.

The difference is that `++x` `(--x)` returns the incremented (decremented) value, while `x++` `(x--)` returns the previous value of x.
```javascript
// Let's declare x
let x = 42;

// Store in y, the result of ++x
let y = ++x;

// Let's see the result
console.log(`x is ${x} and y is ${y}`);
→ 'x is 43 and y is 43'

// Now, store in y the result of x++. Note, x is now 43.
y = x++;

// Let's see again
console.log(`x is ${x} and y is ${y}`);
→ 'x is 44 and y is 43'

// So, `y` is 43, because `x++` returned the pre-incremented value (which was 43)
```

#### Loops

This will print all the integers from 1 to 42.

##### `for`

```javascript
for (var i = 1; i <= 42; ++i) {
  console.log(i);
}
```
Using for ... in ... can be used to iterate object keys:

```javascript
var name = {
  first: "Johnny",
  last: "B."
};

for (var key in name) {
  if (name.hasOwnProperty(key)) {
    console.log(key, name[key]);
    // "first", "Johnny"
    // "last", "B."
  }
}
```

In ES6 there is a for ... of ... as well. It's pretty neat since it iterates any iterable objects (arrays, strings etc).

```javascript
let numbers = [-1, 7, 42, 64];
for (let num of numbers) {
  console.log(num);
}
// -1
// 7
// 42
// 64
while

var i = 1;
while (i <= 42) {
    console.log(i);
    ++i;
}
do - while

var i = 0;
do {
    ++i;
    console.log(i);
} while (i < 42);
```

#### Regular expressions

Regular expressions are a lot of fun. They are delimited by slashes:

```javascript
/^[0-9]+$/gm.test("a")
// => false

/^[0-9]+$/gm.test("42")
// => true
```
A regular expression has a pattern and flags. The pattern in this example is ^[0-9]+$—meaning if the input is one or more digits (from 0 to 9), we validate it—while the flags are g (global) and m (multiline).

regex101.com is a good resource for visualizing and sharing regular expressions with others:

javascript cheat sheet

Regular expressions can be used to match things in strings, such as numbers:

```javascript
// Get all the numbers from this input (note you'll get an array of strings)
> "Alice was born in 1974, so she's 42 years old in 2016.".match(/[0-9]+/g)
[ '1974', '42', '2016' ]

// Want to get numbers? Map them using the `Number` function
> "Alice was born in 1974, so she's 42 years old in 2016.".match(/[0-9]+/g).map(Number)
[ 1974, 42, 2016 ]

// BE CAREFUL that if there's no match `match` will return `null`
> "Bob is seven years old.".match(/[0-9]+/g)
null
```
Also, they can be used to replace parts of the strings:

// Mask all the digits of a credit card number, but the last 4
> "1234234534564567".replace(/\d(?=\d{4})/g, "*");
'************4567'


```javascript
// Style
```

#### Useful Math functions and constants

```javascript
// Get the PI number
> Math.PI
3.141592653589793

// Get the E number
> Math.E
2.718281828459045

// Maximum between two numbers
> Math.max(7, 42)
42

// Minimum between numbers (arguments)
> Math.min(7, 42, 255, 264)
7

// Power
> Math.pow(2, 4)
16

// Random number between 0 (inclusive) and 1 (exclusive)
> Math.random()
0.2114640628617317

// Round
> Math.round(41.8)
42
```

There are trigonometric functions as well: sin, cos, tan, atan etc. Note these require radians. One radian is 180 degrees.


#### Dates

Use the Date constructor to create date objects.

```javascript
var now = new Date();
now.getFullYear();
// => 2016
```
If you want to have fancy stuff like formatting dates, etc., use libraries like `moment`.



#### Naming Conventions


##### Use lowerCamelCase for variables, properties and function names

Variables, properties and function names should use lowerCamelCase. They should also be descriptive. Single character variables and uncommon abbreviations should generally be avoided.

```javascript
Right:

var adminUser = db.query('SELECT * FROM users ...');
Wrong:

var admin_user = db.query('SELECT * FROM users ...');
```

##### Use UpperCamelCase for class names

Class names should be capitalized using UpperCamelCase.

```javascript
Right:

function BankAccount() {
}
Wrong:

function bank_Account() {
}
```

###### Use UPPERCASE for Constants

Constants should be declared as regular variables or static class properties, using all uppercase letters.

```javascript
Right:

var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
Wrong:

const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```


#### Functions

##### Write small functions

Keep your functions short. A good function fits on a slide that the people in the last row of a big room can comfortably read. So don't count on them having perfect vision and limit yourself to ~15 lines of code per function.

##### Return early from functions

To avoid deep nesting of if-statements, always return a function's value as early as possible.

```javascript
Right:

function isPercentage(val) {
  if (val < 0) {
    return false;
  }

  if (val > 100) {
    return false;
  }

  return true;
}
Wrong:

function isPercentage(val) {
  if (val >= 0) {
    if (val < 100) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}
```

Or for this particular example it may also be fine to shorten things even further:

```javascript
function isPercentage(val) {
  var isInRange = (val >= 0 && val <= 100);
  return isInRange;
}
```

##### Name your closures

Feel free to give your closures a name. It shows that you care about them, and will produce better stack traces, heap and cpu profiles.

```javascript
Right:

req.on('end', function onEnd() {
  console.log('winning');
});
Wrong:

req.on('end', function() {
  console.log('losing');
});
```

##### No nested closures

Use closures, but don't nest them. Otherwise your code will become a mess.

```javascript
Right:

setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
Wrong:

setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```

##### Method chaining

One method per line should be used if you want to chain methods.

You should also indent these methods so it's easier to tell they are part of the same chain.

```javascript
Right:

User
  .findOne({ name: 'foo' })
  .populate('bar')
  .exec(function(err, user) {
    return true;
  });
Wrong:

User
.findOne({ name: 'foo' })
.populate('bar')
.exec(function(err, user) {
  return true;
});

User.findOne({ name: 'foo' })
  .populate('bar')
  .exec(function(err, user) {
    return true;
  });

User.findOne({ name: 'foo' }).populate('bar')
.exec(function(err, user) {
  return true;
});

User.findOne({ name: 'foo' }).populate('bar')
  .exec(function(err, user) {
    return true;
  });
```

#### Stay Away from

##### Object.freeze, Object.preventExtensions, Object.seal, with, eval

Crazy shit that you will probably never need. Stay away from it.

##### Requires At Top

Always put requires at top of file to clearly illustrate a file's dependencies. Besides giving an overview for others at a quick glance of dependencies and possible memory impact, it allows one to determine if they need a package.json file should they choose to use the file elsewhere.

##### Getters and setters

Do not use setters, they cause more problems for people who try to use your software than they can solve.

Feel free to use getters that are free from side effects, like providing a length property for a collection class.

##### Do not extend built-in prototypes

Do not extend the prototype of native JavaScript objects. Your future self will be forever grateful.

```javascript
Right:

var a = [];
if (!a.length) {
  console.log('winning');
}
Wrong:

Array.prototype.empty = function() {
  return !this.length;
}

var a = [];
if (a.empty()) {
  console.log('losing');
}

```


#### Standard js

##### Rules

* **Use 2 spaces** for indentation.

  ```js
  function hello (name) {
    console.log('hi', name)
  }
  ```

* **Use single quotes for strings** except to avoid escaping.

  ```js
  console.log('hello there')
  $("<div class='box'>")
  ```

* **No unused variables.**

  ```js
  function myFunction () {
    var result = something()   // ✗ avoid
  }
  ```

* **Add a space after keywords.**

  ```js
  if (condition) { ... }   // ✓ ok
  if(condition) { ... }    // ✗ avoid
  ```

* **Add a space before a function declaration's parentheses.**

  ```js
  function name (arg) { ... }   // ✓ ok
  function name(arg) { ... }    // ✗ avoid

  run(function () { ... })      // ✓ ok
  run(function() { ... })       // ✗ avoid
  ```

* **Always use** `===` instead of `==`.<br>
  Exception: `obj == null` is allowed to check for `null || undefined`.

  ```js
  if (name === 'John')   // ✓ ok
  if (name == 'John')    // ✗ avoid
  ```

  ```js
  if (name !== 'John')   // ✓ ok
  if (name != 'John')    // ✗ avoid
  ```

* **Infix operators** must be spaced.

  ```js
  // ✓ ok
  var x = 2
  var message = 'hello, ' + name + '!'
  ```

  ```js
  // ✗ avoid
  var x=2
  var message = 'hello, '+name+'!'
  ```

* **Commas should have a space** after them.

  ```js
  // ✓ ok
  var list = [1, 2, 3, 4]
  function greet (name, options) { ... }
  ```

  ```js
  // ✗ avoid
  var list = [1,2,3,4]
  function greet (name,options) { ... }
  ```

* **Keep else statements** on the same line as their curly braces.

  ```js
  // ✓ ok
  if (condition) {
    // ...
  } else {
    // ...
  }
  ```

  ```js
  // ✗ avoid
  if (condition) {
    // ...
  }
  else {
    // ...
  }
  ```

* **For multi-line if statements,** use curly braces.

  ```js
  // ✓ ok
  if (options.quiet !== true) console.log('done')
  ```

  ```js
  // ✓ ok
  if (options.quiet !== true) {
    console.log('done')
  }
  ```

  ```js
  // ✗ avoid
  if (options.quiet !== true)
    console.log('done')
  ```

* **Always handle the** `err` function parameter.

  ```js
  // ✓ ok
  run(function (err) {
    if (err) throw err
    window.alert('done')
  })
  ```

  ```js
  // ✗ avoid
  run(function (err) {
    window.alert('done')
  })
  ```

* **Always prefix browser globals** with `window.`.<br>
  Exceptions are: `document`, `console` and `navigator`.

  ```js
  window.alert('hi')   // ✓ ok
  ```

* **Multiple blank lines not allowed.**

  ```js
  // ✓ ok
  var value = 'hello world'
  console.log(value)
  ```

  ```js
  // ✗ avoid
  var value = 'hello world'


  console.log(value)
  ```

* **For the ternary operator** in a multi-line setting, place `?` and `:` on their own lines.

  ```js
  // ✓ ok
  var location = env.development ? 'localhost' : 'www.api.com'

  // ✓ ok
  var location = env.development
    ? 'localhost'
    : 'www.api.com'

  // ✗ avoid
  var location = env.development ?
    'localhost' :
    'www.api.com'
  ```

* **For var declarations,** write each declaration in its own statement.

  ```js
  // ✓ ok
  var silent = true
  var verbose = true

  // ✗ avoid
  var silent = true, verbose = true

  // ✗ avoid
  var silent = true,
      verbose = true
  ```

* **Wrap conditional assignments** with additional parentheses. This makes it clear that the expression is intentionally an assignment (`=`) rather than a typo for equality (`===`).

  ```js
  // ✓ ok
  while ((m = text.match(expr))) {
    // ...
  }

  // ✗ avoid
  while (m = text.match(expr)) {
    // ...
  }
  ```

##### Semicolons

* No semicolons. (see: [1](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding), [2](http://inimino.org/%7Einimino/blog/javascript_semicolons), [3](https://www.youtube.com/watch?v=gsfbh17Ax9I))

  ```js
  window.alert('hi')   // ✓ ok
  window.alert('hi');  // ✗ avoid
  ```

* Never start a line with `(`, `[`, or `` ` ``. This is the only gotcha with omitting semicolons, and standard protects you from this potential issue.

  ```js
  // ✓ ok
  ;(function () {
    window.alert('ok')
  }())

  // ✗ avoid
  (function () {
    window.alert('ok')
  }())
  ```

  ```js
  // ✓ ok
  ;[1, 2, 3].forEach(bar)

  // ✗ avoid
  [1, 2, 3].forEach(bar)
  ```

  ```js
  // ✓ ok
  ;`hello`.indexOf('o')

  // ✗ avoid
  `hello`.indexOf('o')
  ```

  Note: If you're often writing code like this, you may be trying to be too clever.

  Clever short-hands are discouraged, in favor of clear and readable expressions, whenever
  possible.

  Instead of this:

  ```js
  ;[1, 2, 3].forEach(bar)
  ```

  This is much preferred:

  ```js
  var nums = [1, 2, 3]
  nums.forEach(bar)
  ```


```javascript
// Style
```

#### JS Fiddle Examples

Description of the section

```javascript
// Style
```

### Mean Stack Style
### Angular Style
### Express Style



## Common Mistakes
## Tools
## Installs & Versions
## Blogs
## Github
## More Resources
## Bootcamps
## Courses
## Mentee
## Contribute
## Future
## License




-
-
-
--
-
-
-
-
-
-
-


## Table of Contents

- [Control Flow](#control-flow)
- [Mongodb](#mongodb)
- [Mongoose](#mongoose)
- [ExpressJS Style](#expressjs-style)
- [Versions](#versions)
- [Text Editor](#text-editor)
- [Install Tools](#install-tools)
- [Javascript Style](#javascript-style)
- [Angular Style](#angular-style)

## Control Flow

###[run-auto](https://www.npmjs.com/package/run-auto)

#### Installation
```
npm install run-auto --save
```
##### Usage


```js
var auto = require('run-auto')

auto({
  getData: function (callback) {
    console.log('in getData')
    // async code to get some data
    callback(null, 'data', 'converted to array')
  },
  makeFolder: function (callback) {
    console.log('in makeFolder')
    // async code to create a directory to store a file in
    // this is run at the same time as getting the data
    callback(null, 'folder')
  },
  writeFile: ['getData', 'makeFolder', function (results, callback) {
    console.log('in writeFile', JSON.stringify(results))
    // once there is some data and the directory exists,
    // write the data to a file in the directory
    callback(null, 'filename')
  }],
  emailLink: ['writeFile', function (results, callback) {
    console.log('in emailLink', JSON.stringify(results))
    // once the file is written let's email a link to it...
    // results.writeFile contains the filename returned by writeFile.
    callback(null, { file: results.writeFile, email: 'user@example.com' })
  }]
}, function(err, results) {
  console.log('err = ', err)
  console.log('results = ', results)
})
// With Multiple Mongo Calls
auto({
    blogs: function (cb) {
      blogs
        .find()
        .populate('user')
        .exec(cb)
    },
    users: function (cb) {
      users
        .find()
        .populate('user')
        .exec(cb)
    },
    tester: function (cb) {
      tester
        .find()
        .populate('user')
        .exec(cb)
    },
    todos: function (cb) {
      todos
        .find()
        .populate('user')
        .exec(cb)
    }
  }, function (err, results) {
    if (err) return next(err)
    return res.status(200).send(results)
  })
```


## Mongodb

### Installation
- <img src="https://www.mongodb.com/assets/global/favicon-bf23af61025ab0705dc84c3315c67e402d30ed0cba66caff15de0d57974d58ff.ico" height="17">&nbsp; [Download](https://www.mongodb.org/downloads) and Install mongodb - <a href="https://docs.mongodb.org/manual/">Checkout their manual</a> if you're just starting.
  - <img src="http://deluge-torrent.org/images/apple-logo.gif" height="17">&nbsp; [OSX MongoDB](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/)
  - <img src="http://dc942d419843af05523b-ff74ae13537a01be6cfec5927837dcfe.r14.cf1.rackcdn.com/wp-content/uploads/windows-8-50x50.jpg" height="17">&nbsp; [Windows Mongodb](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-windows/)
  - <img src="https://lh5.googleusercontent.com/-2YS1ceHWyys/AAAAAAAAAAI/AAAAAAAAAAc/0LCb_tsTvmU/s46-c-k/photo.jpg" height="17">&nbsp; [Linux Mongodb](https://docs.mongodb.org/manual/administration/install-on-linux/)
  
### Database-as-a-Service
[Mlab](https://mlab.com)

## Mongoose

###[mongoose-validator](https://www.npmjs.com/package/mongoose-validator)

#### Installation

```bash
$ npm install mongoose-validator --save
```

#### Usage

```javascript
var mongoose = require('mongoose');
var validate = require('mongoose-validator');

var nameValidator = [
  validate({
    validator: 'isLength',
    arguments: [3, 50],
    message: 'Name should be between {ARGS[0]} and {ARGS[1]} characters'
  }),
  validate({
    validator: 'isAlphanumeric',
    passIfEmpty: true,
    message: 'Name should contain alpha-numeric characters only'
  })
];

var Schema = new mongoose.Schema({
  name: {type: String, required: true, validate: nameValidator}
});

```


## ExpressJS Style

### [express-validator](https://www.npmjs.com/package/express-validator)

#### Installation

```
npm install express-validator --save
```

#### Usage

```javascript
var util = require('util'),
    express = require('express'),
    expressValidator = require('express-validator'),
    app = express.createServer();

app.use(express.bodyParser());
app.use(expressValidator([options])); // this line must be immediately after express.bodyParser()!

app.post('/:urlparam', function(req, res) {

  // VALIDATION
  req.assert('postparam', 'Invalid postparam').notEmpty().isInt();
  req.assert('urlparam', 'Invalid urlparam').isAlpha();
  req.assert('getparam', 'Invalid getparam').isInt();

  // SANITIZATION
  // as with validation these will only validate the relevent param in all areas
  req.sanitize('postparam').toBoolean();

  var errors = req.validationErrors();
  if (errors) {
    res.send('There have been validation errors: ' + util.inspect(errors), 400);
    return;
  }
  res.json({
    urlparam: req.params.urlparam,
    getparam: req.params.getparam,
    postparam: req.params.postparam
  });
});

app.listen(8888);
```
#### Options
[Api Options](https://github.com/chriso/validator.js/blob/master/README.md)


### [validator](https://npmjs.org/package/validator)

#### Installation

```
npm install validator --save
```

#### Usage
```javascript
var validator = require('validator');

validator.isEmail('foo@bar.com'); //=> true
```
#### Options
[Api Options](https://github.com/chriso/validator.js/blob/master/README.md)

## Versions

| Framework/Library/Program  |Version  |
|---------------|----------------|
| Angular   |   1.X   |
| JQuery  |   3.X   |
| NodeJs  |   6.X   |
| Centos  |   7   |
| NodeJs  |   6.X   |

## Text Editor

[Sublime 3](https://www.sublimetext.com/3)
or
[Atom](https://atom.io/)

## Install Tools


### StandardJs

#### [Sublime Text](https://www.sublimetext.com/)

Using **[Package Control][sublime-1]**, install **[SublimeLinter][sublime-2]** and
**[SublimeLinter-contrib-standard][sublime-3]**.

For automatic formatting on save, install **[StandardFormat][sublime-4]**.

[sublime-1]: https://packagecontrol.io/
[sublime-2]: http://www.sublimelinter.com/en/latest/
[sublime-3]: https://packagecontrol.io/packages/SublimeLinter-contrib-standard
[sublime-4]: https://packagecontrol.io/packages/StandardFormat

#### [Atom](https://atom.io)

Install **[linter-js-standard][atom-1]**.

For automatic formatting, install **[standard-formatter][atom-2]**. For snippets,
install **[standardjs-snippets][atom-3]**.

[atom-1]: https://atom.io/packages/linter-js-standard
[atom-2]: https://atom.io/packages/standard-formatter
[atom-3]: https://atom.io/packages/standardjs-snippets

## Javascript Style

[Standard JS](https://github.com/feross/standard)

No decisions to make. No `.eslintrc`, `.jshintrc`, or `.jscsrc` files to manage. It just
works.

This module saves you (and others!) time in two ways:

- **No configuration.** The easiest way to enforce consistent style in your
  project. Just drop it in.
- **Catch style errors before they're submitted in PRs.** Saves precious code
  review time by eliminating back-and-forth between maintainer and contributor.

Install with:

```
npm install standard
```

### The Rules

- **2 spaces** – for indentation
- **Single quotes for strings** – except to avoid escaping
- **No unused variables** – this one catches *tons* of bugs!
- **No semicolons** – [It's][1] [fine.][2] [Really!][3]
- **Never start a line with `(`, `[`, or `` ` ``**
  - This is the **only** gotcha with omitting semicolons – *automatically checked for you!*
  - [More details][4]
- **Space after keywords** `if (condition) { ... }`
- **Space after function name** `function name (arg) { ... }`
- Always use `===` instead of `==` – but `obj == null` is allowed to check `null || undefined`.
- Always handle the node.js `err` function parameter
- Always prefix browser globals with `window` – except `document` and `navigator` are okay
  - Prevents accidental use of poorly-named browser globals like `open`, `length`,
    `event`, and `name`.
- **And [more goodness][5]** – *give `standard` a try today!*

[1]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[2]: http://inimino.org/~inimino/blog/javascript_semicolons
[3]: https://www.youtube.com/watch?v=gsfbh17Ax9I
[4]: RULES.md#semicolons
[5]: RULES.md#javascript-standard-style

To get a better idea, take a look at
[a sample file](https://github.com/feross/bittorrent-dht/blob/master/client.js) written
in JavaScript Standard Style, or check out some of
[the repositories](https://github.com/feross/standard-packages/blob/master/all.json) that use
`standard`.

The easiest way to use JavaScript Standard Style to check your code is to install
it globally as a Node command line program. To do so, simply run the following
command in your terminal (flag `-g` installs `standard` globally on your system,
omit it if you want to install in the current working directory):


```bash
npm install standard --g
```

## Angular Style

[John Papa's Style Guide](https://github.com/johnpapa/angular-styleguide/tree/master/a1)


GPS Style Guide

Intro
About helping you learn what you like and dont like
Be flexible to best the best dev possible
You will like some o fwhat i say and not some of it
Make it yours 
Tips
Dont trust the computer trust what happens - not what it is always saying
Know what you dont know
Build on the shoulders of Gaints - mentors
Build , Learn , test, Refactor , repeat
Mentor
Common Mistakes
https://www.airpair.com/node.js/posts/top-10-mistakes-node-developers-make#8-not-using-static-analysis-tools
Blocking the event loop
Executing a callback multiple times
Callback Hell
Creating big monolithic applications
Poor logging
No tests
Not using static analysis tools
Zero monitoring or profiling
Debugging with console.log

Tools
Ref Setup Guide
Ref Debug Guide
Text Editor
Javascript Style
Control Flow
Lodash
Be Async
When to Be Sync
Promises
Bind Call Apply
Avoid polluting the global scope
https://www.codementor.io/johnnyb/tutorials/javascript-best-practices-du107mvud
Use Strict
|| and &&
Convert value types
Variable declarations
How to name properly
Avoid global
Good name example  - isLegalAge() not isoverEightteen
https://github.com/IonicaBizau/code-style
Let const var - https://www.codementor.io/johnnyb/tutorials/javascript-cheatsheet-fb54lz08k
Iterating object and arrays
Binary and Ternary operators
Comments
Use Slashes for comments - felexge
https://www.codementor.io/johnnyb/tutorials/javascript-cheatsheet-fb54lz08k
If
Switch
Primitive values
Objects
Arrays
Access, add, remove, and update elements
Iterating over Arrays
Functions
Constructors and Classes
Async vs Sync
What are callbacks?
Promises
Create and throw errors
Error handling
Try catch
Incrementing/decrementing numbers
Loops
Regular expressions
Useful Math functions and constants
Dates
Naming Conventions
https://github.com/felixge/node-style-guide#use-slashes-for-comments
Use lowerCamelCase for variables, properties and function names
Use UpperCamelCase for class names & mongoose Models
Use UPPERCASE for Constants

Conditionals
===
Single Ternary
descriptive conditions
Functions
Write small functions
Return early from functions
Name your closures
No nested closures
Method chaining
Stay Away from
object.freeze, Object.preventExtensions, Object.seal, with, eval
Do not extend built-in prototypes
Standard js
Node JS Style
Require dependencies up front
Mongoose
ExpressJS Style
How to intregrate NPM modules
Mean Stack Style
Mongodb
Mean Stack JS
Competitors
Meanstack.net
Io
Js
Production
Pm2
Helmet
--max-old-space-size  ~15gb, that flags seems to raise the limit
Development
Nodemon

Other Npm Packages
Test 
E2e
Unit

Angular Style
John Papa
Versions
Blogs
https://blog.risingstack.com/
https://www.codementor.io
https://toddmotto.com/
https://scotch.io/
https://davidwalsh.name/
http://dailyjs.com/
https://www.sitepoint.com/javascript/
https://www.javascript.com/
http://blog.teamtreehouse.com/
http://www.nearform.com/nodecrunch/
Github
Commands
Users
Markdown -https://www.codementor.io/ianwang/tutorials/codementor-writing-tutorials-markdown-cheat-sheet-lmpdjdriw?utm_campaign=user-db&utm_source=tutorial&utm_medium=announcement&utm_term=lmpdjdriw&ref=user-db-tutorial-lmpdjdriw
Headers
Emphasis
Lists
Links
Images
Code
Tables
Block Quotes
Inline HTML
Horizontal Rules
Line Breaks
Video Embedding
Other Style Guides
Follow
https://github.com/johnpapa/angular-styleguide
http://standardjs.com/
Other Great Ones
https://github.com/airbnb/javascript
https://github.com/IonicaBizau/code-style
http://javascript.crockford.com/code.html
https://github.com/RisingStack/node-style-guide
http://caolanmcmahon.com/posts/nodejs_style_and_structure
https://docs.npmjs.com/misc/coding-style
http://anixir.com/minimal-node-express-style-guide/
https://make.wordpress.org/core/handbook/best-practices/coding-standards/javascript/
https://github.com/rwaldron/idiomatic.js
https://github.com/toddmotto/angular-styleguide
Code Quality Tools, Resources & References
JavaScript Plugin for Sonar
Plato
jsPerf
jsFiddle
Codepen
jsbin
JavaScript Lint (JSL)
jshint
jslint
eslint
jscs
jscodesniffer
Editorconfig
Hound

Contribute
Mentors
Code Mentor
Thinkful
Bootcamps
Thinkful
Courses
Tree House
Code Academy
Pluralsight
Blogs
Thinkful
License
Future
Lint  - something loadable
ES6-https://github.com/DrkSephy/es6-cheatsheet
IDEA project - https://github.com/verekia/js-stack-from-scratch

Buttons
Paypal , support me on patreon, codementor


