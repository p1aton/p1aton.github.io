---
layout: post
title:  "Javascript Cheatsheet"
crawlertitle: "Javascript Cheatsheet"
summary: "Javascript Cheatsheet"
date:   2022-05-31 23:09:47 +0700
categories: posts
tags: ['Веб аналитика']
#author: Felipe
---
This document is a cheatsheet for JavaScript you will frequently encounter in modern projects and most contemporary sample code.

## Modern ES2015, ES2016, ES2017 and Beyond...

### Index
* [Comments](#comments)
* [Variables](#variables)
* [Data Types](*data-types)
* [Operators](#operators)
* [Functions](#functions)
* [Promise](#promise)
* [Class](#class)
* [Import](#import)
* [Export](#export)
* [Strings](#strings)
* [Numbers](#numbers)
* [Arrays](#arrays)
* [Date](#date)
* [Maths](#maths)
* [Conditional](#conditional)
* [Switch](#switch)
* [Boolean](#boolean)
* [Loops](#loops)
* [Break and Continue](#break-and-continue)
* [Error Handling](#error-handling)
* [JSON - JavaScript Object Notation](#json)
* [Fetch (POST,GET,PUT,PATCH,DELETE)](#fetch)
* [LocalStorage](#localstorage)
* [DOM - Document Object Model](#dom-document-object-model)

## Comments
```js
// This is a single line comment.

/*
This is a
multi-line
comment.
*/

// var message = "hello"; // Comment out line of code.
```
[Back to Top](#javascript-cheatsheet)

## Variables
```js
var message = "hello";    // Set Variable (Can be updated).
let message = "hello";     // Set Fixed Variable (Can be change outside scope, eg within a function).
const message = "hello";   // Set Constant (Can't be redeclared or reassigned).

var output = `${message} world`; // Call variable within a String (Need back ticks).
var output = message + "world"; // Attach variable to a String (Using plus sign).

var car = {type: "Fiat", model: "500", color: "white"}; // Create Variable JSON
car.type // Output: Fiat
car.type = "Ford"; // Updates type to "Ford"
car.type // Output: Ford
car["type"] // Output: Ford

var person = {
    firstName: "John",
    lastName : "Doe",
    id       : 5566,
    fullName : function() {
        return this.firstName + " " + this.lastName;
    }
};
person.fullName // Output: John Doe

var x, y, z;    // Declare multiple variables at once.
x = 5;          // Set x value.
y = 6;          // Set y value.
z = x + y;      // Set z value with operation.

var person = "John Doe",
age = "24",
sex = "male"; // Span variables over multiple lines.

var carName; // Equals undefined.
```
[Back to Top](#javascript-cheatsheet)

## Data Types
```js
var x;                                         // undefined
var locked = true;                             // Boolean
var length = 16;                               // Number
var lastName = "Johnson";                      // String
var x = {firstName:"John", lastName:"Doe"};    // Object
var y = function() {return "result"};          // Function

typeof "John"              // Returns "string"
typeof 3.14                // Returns "number"
typeof true                // Returns "boolean"
typeof false               // Returns "boolean"
typeof x                   // Returns "undefined" (if x has no value)
typeof {name:'John', age:34} // Returns "object"
typeof [1,2,3,4]             // Returns "object" (not "array", see note below)
typeof null                  // Returns "object"
typeof function myFunc(){}   // Returns "function"
```
[Back to Top](#javascript-cheatsheet)

## Operators
### Arithmetic Operators
```js
+	  // Addition
-	  // Subtraction
*	  // Multiplication
/	  // Division
%	  // Modulus (Division Remainder)
++	// Increment
--  // Decrement
```
### Assignment Operators
```js
=	    // Example: x = y     // Same As: x = y
+=    // Example: x += y    // Same As: x = x + y
-=    // Example: x -= y    // Same As: x = x - y
*=    // Example: x *= y    // Same As: x = x * y
/=    // Example: x /= y    // Same As: x = x / y
%=    // Example: x %= y    // Same As: x = x % y
<<=	  // Example: x <<= y	  // Same As:x = x << y
>>=	  // Example: x >>= y	  // Same As:x = x >> y
>>>=	// Example: x >>>= y	// Same As:x = x >>> y
&=	  // Example: x &= y	  // Same As:x = x & y
^=	  // Example: x ^= y	  // Same As:x = x ^ y
|=	  // Example: x |= y	  // Same As:x = x | y
**=	  // Example: x **= y	  // Same As:x = x ** y
```
[Back to Top](#javascript-cheatsheet)

## Functions
### Basic Function
```js
// Create Function
function sum(x, y) {
  return x + y;
}
// Call Function
sum(x, y);
```

### Function with default
```js
function myFunction(x, y = 10) {
    // y is 10 if not passed or undefined
    return x + y;
}
myFunction(5); // will return 15
```

### Function Object
```js
var greeting = new function () {
  this.hello = "Hello from this code";

  this.bye = function () {
    return 'Bye from this code';
  };

}

console.log(greeting.hello) // Output: Hello from this code
console.log(greeting.bye()) // Output: Bye from this code
```

### Advance Function
```js
var counter = (function(){
  var i = 0;

  return {
    get: function(){
      return i;
    },
    set: function( val ){
      i = val;
    },
    increment: function() {
      return ++i;
    }
  };
}());

// 'counter' is an object with properties, which in this case happen to be
// methods.

counter.get(); // 0
counter.set( 3 );
counter.increment(); // 4
counter.increment(); // 5
```

### Function Constructor (Class like)
```js
function Person(first, last, age, eye) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eye;
}

var myFather = new Person("John", "Doe", 50, "blue");
var myMother = new Person("Sally", "Rally", 48, "green");
```

### Self Initialising Function
```js
(function() { ... })();

(() => { ... })();
```

### Function with Callback
```js
// A Callback Function
function myFunction(name, callback) {
  var response = "My successful response.";
  var error = "My error response.";
  
  console.log(name); // Random Variable passed in Function
  
  callback(response, error);
}

// Function with Callback - Option 1
myFunction('Joe Blogs', function(response, error) {
  console.log(response); // Successful Response
  console.log(response); // Error Response
});

// Function with Callback - Option 2
myFunction('Joe Blogs', (response, error) => {
  console.log(response); // Successful Response
  console.log(response); // Error Response
});
```
[Back to Top](#javascript-cheatsheet)

## Promise
### Creating a Promise Function
```js
/*
Promise (ES6 >)
The creation of a Promise object is done via the Promise constructor by calling “new Promise()”. It takes a function as an argument and that function gets passed two callbacks: one for notifying when the operation is successful (resolve) and one for notifying when the operation has failed (reject).
*/

///// Sample 1 - Using Timer /////
function getAsyncData() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('Here is the resolved data');
    }, 2000);
  });
}

///// Sample 2 - Using MySQL /////
function getAsyncData() {
  return new Promise ((resolve, reject) => {
    db.query("SELECT * FROM post", (error, response) => {
      if (error) { reject(error); }
      else { resolve(response); }
    });
  });
}
```

### Using Promise Function
```js
// Option 1
getAsyncData()
.then((result) => {
    // Do stuff
})
.catch((error) => {
    // Handle error
})
.finally(() => {
    // Finally always executed
});

// Option 2
try {
  getAsyncData().then((result) => console.log(result));
} catch (error) {
  console.error(error);
}
```

### Using Promise Function with Async and Await
```js
// (ES7 >) Async Function Loads in Order or Awaits calling Promises...
async function asyncFunction() {
  console.log('Getting Database Results Please Wait...');
  var result = await getAsyncData(); // Wait for Function with a Promise in it.
  console.log(result); // Results from getAsyncData
  return result
}

// Option 1 for Calling Async Function
asyncFunction();

// Option 2 for Calling Async Function
asyncFunction()
.then((result) => {
    console.log(`The Results: ${result}`)
})
.catch((error) =>{
    // Handle error
})
.finally(() => {
    // Finally always executed
});

// Option 3 for Calling Async Function
try {
  asyncFunction().then((result) => console.log(result));
} catch (error) {
  console.error(error);
}
```

[Back to Top](#javascript-cheatsheet)


## Class
### Basic Class
```js
class User {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    alert(this.name);
  }
}

let user = new User("John");
user.sayHi();
```

### Extend a Class
```js
class Person {
    sayHello() {
        alert('hello');
    }
    walk() {
        alert('I am walking!');
    }
}

class Student extends Person {
    sayGoodBye() {
        alert('goodBye');
    }
    sayHello() {
        alert('hi, I am a student');
    }
}

var student1 = new Student();
student1.sayHello();
student1.walk();
student1.sayGoodBye();

// check inheritance
alert(student1 instanceof Person); // true
alert(student1 instanceof Student); // true
```

### Class Get and Set
```js
class User {
  constructor(name) {
    // invokes the setter
    this.name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 4) {
      alert("Name is too short.");
      return;
    }
    this._name = value;
  }
}

let user = new User("John");
alert(user.name); // John

user = new User(""); // Name too short.
```

### Class Static
```js
class User {
  static staticMethod() {
    alert(this === User);
  }
}

User.staticMethod(); // true
```

### Class Constructor used in Array
```js
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }
}

// usage
let articles = [
  new Article("Mind", new Date(2016, 1, 1)),
  new Article("Body", new Date(2016, 0, 1)),
  new Article("JavaScript", new Date(2016, 11, 1))
];
alert( articles[0].title ); // Body
```
[Back to Top](#javascript-cheatsheet)

## Import
```js
import 'helpers'
// aka: require('···')

import Express from 'express'
// aka: const Express = require('···').default || require('···')

import { indent } from 'helpers'
// aka: const indent = require('···').indent

import * as Helpers from 'helpers'
// aka: const Helpers = require('···')

import { indentSpaces as indent } from 'helpers'
// aka: const indent = require('···').indentSpaces
```
[Back to Top](#javascript-cheatsheet)

## Export
```js
export default function () { ··· }
// aka: module.exports.default = ···

export function mymethod () { ··· }
// aka: module.exports.mymethod = ···

export const pi = 3.14159
// aka: module.exports.pi = ···
```
[Back to Top](#javascript-cheatsheet)

## Strings
### Find / Search in a String
```js
// The indexOf() method returns the index of (the position of) the first occurrence of a specified text in a string ...
var str = "Please locate where 'locate' occurs!";
var pos = str.indexOf("locate");

// The lastIndexOf() method returns the index of the last occurrence of a specified text in a string ...
var str = "Please locate where 'locate' occurs!";
var pos = str.lastIndexOf("locate");

// Both methods accept a second parameter as the starting position for the search ...
var str = "Please locate where 'locate' occurs!";
var pos = str.indexOf("locate",15);

// The search() method searches a string for a specified value and returns the position of the match ...
var str = "Please locate where 'locate' occurs!";
var pos = str.search("locate");

/*
The two methods are NOT equal. These are the differences:
- The search() method cannot take a second start position argument.
- The indexOf() method cannot take powerful search values (regular expressions).
*/

// If not found search and find returns -1
```

### Extracting String Parts
```js
var str = "Apple, Banana, Kiwi";
var res = str.slice(7, 13);
// Returns Banana

var str = "Apple, Banana, Kiwi";
var res = str.slice(-12, -6);
// Returns Banana

var str = "Apple, Banana, Kiwi";
var res = str.slice(7);
// Returns Banana, Kiwi

var str = "Apple, Banana, Kiwi";
var res = str.substr(-4);
// Returns Kiwi
```
### Replace in Strings
```js
str = "Please visit Microsoft!";
var n = str.replace("Microsoft", "W3Schools");
// Replaces first match.

str = "Please visit Microsoft!";
var n = str.replace(/MICROSOFT/i, "W3Schools");
// Replaces first match insensitive to case.

str = "Please visit Microsoft and Microsoft!";
var n = str.replace(/Microsoft/g, "W3Schools");
// Replaces all matches.

var str = "       Hello World!        ";
str.trim();
// Removes whitespace from both sides of a string.

var str = "       Hello World!        ";
str.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
// Remove with regular expressions.
```
### String to Upper and Lower Case
```js
var text1 = "Hello World!";       // String
var text2 = text1.toUpperCase();  // text2 is text1 converted to upper

var text1 = "Hello World!";       // String
var text2 = text1.toLowerCase();  // text2 is text1 converted to lower
```

### Character at index position
```js
var str = "HELLO WORLD";
str.charAt(0);
// Returns H

str[0];
// Returns H
```

### String to Array
```js
var txt = "a,b,c,d,e";   // String
txt.split(",");          // Split on commas
txt.split(" ");          // Split on spaces
txt.split("|");          // Split on pipe
txt.split("");            // Split all Characters into a Array
```

### Convert to String
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.toString() // Returns: Banana,Orange,Apple,Mango

var num = 12
num.toString() // Returns: "12" as string.
```
[Back to Top](#javascript-cheatsheet)

## Numbers
### Checks
```js
Number.isInteger(10);        // returns true
Number.isInteger(10.5);      // returns false

Number.isSafeInteger(10);    // returns true
Number.isSafeInteger(12345678901234567890);  // returns false

isNaN("Hello");   // returns true (Not-A-Number)
isNaN(123);       // returns false (Not-A-Number)
```

### Decimals
```js
var x = 9.656;
x.toFixed(0);           // returns 10
x.toFixed(2);           // returns 9.66
x.toFixed(4);           // returns 9.6560
x.toFixed(6);           // returns 9.656000
```
### Specified length
```js
var x = 9.656;
x.toPrecision();        // returns 9.656
x.toPrecision(2);       // returns 9.7
x.toPrecision(4);       // returns 9.656
x.toPrecision(6);       // returns 9.65600
```

### Number Check / Convert
```js
Number(true);          // returns 1
Number(false);         // returns 0
Number("10");          // returns 10
Number("  10");        // returns 10
Number("10  ");        // returns 10
Number(" 10  ");       // returns 10
Number("10.33");       // returns 10.33
Number("10,33");       // returns NaN
Number("10 33");       // returns NaN
Number("John");        // returns NaN
Number(new Date("2017-09-30"));    // returns 1506729600000
```

### Number Parse into Int
```js
parseInt("10");         // returns 10
parseInt("10.33");      // returns 10
parseInt("10 20 30");   // returns 10
parseInt("10 years");   // returns 10
parseInt("years 10");   // returns NaN

parseFloat("10");        // returns 10
parseFloat("10.33");     // returns 10.33
parseFloat("10 20 30");  // returns 10
parseFloat("10 years");  // returns 10
parseFloat("years 10");  // returns NaN
```
[Back to Top](#javascript-cheatsheet)


## Arrays
```js
var cars = []; // Create Blank Array
var cars = ["Saab", "Volvo", "BMW"]; // Create Array
var cars = [
    "Saab",
    "Volvo",
    "BMW"
]; // Create Array

var name = cars[0]; // Call first array which begins with 0 into variable.
cars[0] = "Opel"; // Change first array.

cars.length; // Returns 4
cars[cars.length - 1]; // Returns BMW
cars.sort();   // The sort() method sorts arrays.
```

### Array Loop
```js
var cars = ["Saab", "Volvo", "BMW"];
var length = cars.length;
var index;
for (index = 0; index < length; index++) {
    console.log(cars[index]);
}
```

### Array Loop FOREACH
```js
const posts = [{"title": "This is a post", "content": "This is the contents of the post."};

await posts.forEach(async (user, index) => {
	// user.title or user[index].title
});
```

### Add (Push) new element to end of array
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Kiwi"); // Adds a new element ("Kiwi") to the end of fruits
```

### Add (unshift) new element to beginning of array
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.unshift("Lemon"); // Adds a new element "Lemon" to the beginning of fruits
```

### Add (Splice) element to position in array
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 0, "Lemon", "Kiwi");

// The first parameter (2) defines the position where new elements should be added (spliced in).

// The second parameter (0) defines how many elements should be removed.

// The rest of the parameters ("Lemon" , "Kiwi") define the new elements to be added.
```

### Remove (Pop) last array element
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.pop();  // Removes the last element ("Mango") from fruits
```

### Remove (Shift) first array element
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.shift(); // Removes the first element "Banana" from fruits
```

### Remove (Delete) array element
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
delete fruits[0]; // Changes the element in fruits to undefined

// ! Using delete may leave undefined holes in the array. Use pop() or shift() instead. !
```

### Remove (Splice) element from position in array
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(0, 1);  // Removes the first element of fruits

// The first parameter (0) defines the position where new elements should be added (spliced in).

// The second parameter (1) defines how many elements should be removed.

// The rest of the parameters are omitted. No new elements will be added.
```

### Get Result (Splice) from Array
```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1); // Returns: Orange
```

### Sort Array
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();
```

### Sort and Reverse Array
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();        // First sort the elements of fruits
fruits.reverse();     // Then reverse the order of the elements
```

### Sort Array (By Numbers) Trick
```js
// Ascending
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return a - b});

// Descending
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return b - a});

// Random Order
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return 0.5 - Math.random()});

/*
By default, the sort() function sorts values as strings.
This works well for strings ("Apple" comes before "Banana").
However, if numbers are sorted as strings, "25" is bigger than "100", because "2" is bigger than "1".
Because of this, the sort() method will produce incorrect result when sorting numbers.
*/
```
### For Each Array Function
```js
var numbers = [45, 4, 9, 16, 25];
numbers.forEach(myFunction);

function myFunction(value, index, array) {
    console.log(value);
}
```

### Array Filter Function
```js
var numbers = [45, 4, 9, 16, 25];
var over18 = numbers.filter(myFunction);

function myFunction(value, index, array) {
    return value > 18;
}
// Returns higher than 18
```

### Array find the indexOf element
```js
var fruits = ["Apple", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");
// a = 1

var fruits = ["Apple", "Orange", "Apple", "Mango"];
var a = fruits.lastIndexOf("Apple");
// a = 3
```

### Array FIND
```js
cont values = [
    { name: 'someName1' },
    { name: 'someName2' },
    { name: 'someName3' },
    { name: 'someName4' } 
];

const find = myArray.find(item => item.name === 'someName2');
// { name: 'someName2' }
```

### Array MAP
```js
const myArray = [
  { id: 20, name: 'Captain Piett' },
  { id: 24, name: 'General Veers' },
  { id: 56, name: 'Admiral Ozzel' },
  { id: 88, name: 'Commander Jerjerrod' }
];

const result = myArray.map((item) => { return item.id });
// [20, 24, 56, 88]
```

### Merge Arrays
```js
var arr1 = ["Cecilie", "Lone"];
var arr2 = ["Emil", "Tobias", "Linus"];
var arr3 = ["Robin", "Morgan"];
var myChildren = arr1.concat(arr2, arr3); // Concatenates arr1 with arr2 and arr3
```

### Change Array Join
```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.join(" * "); // Returns: Banana * Orange * Apple * Mango
```
[Back to Top](#javascript-cheatsheet)

## Date
```js
var date = Date.now()
var date = new Date(); // Now
var date = new Date(2018, 11, 24, 10, 33, 30); // Or Set Date
var date = new Date("2015-03-25T12:00:00Z"); // Or Set Date

new Date()
new Date(year, month, day, hours, minutes, seconds, milliseconds)
new Date(milliseconds)
new Date(date string)
```
[Back to Top](#javascript-cheatsheet)

## Maths
```js
Math.round(4.7);    // returns 5
Math.round(4.4);    // returns 4
Math.ceil(4.4);     // returns 5 (Round up)
Math.floor(4.7);    // returns 4 (Round down)

Math.min(0, 150, 30, 20, -8, -200);  // returns -200 (Min)
Math.max(0, 150, 30, 20, -8, -200);  // returns 150 (Max
Math.random();     // returns a random number
Math.floor(Math.random() * 10);     // returns a random integer from 0 to 9
Math.floor(Math.random() * 11);      // returns a random integer from 0 to 10
Math.floor(Math.random() * 100);     // returns a random integer from 0 to 99
Math.floor(Math.random() * 100) + 1; // returns a random integer from 1 to 100

Math.PI;            // returns 3.141592653589793
Math.pow(8, 2);     // returns 64 (Power of)
Math.sqrt(64);      // returns 8 (Square Root)
Math.abs(-4.7);     // returns 4.7 (Positive)

Math.sin(90 * Math.PI / 180);     // returns 1 (the sine of 90 degrees)
Math.cos(0 * Math.PI / 180);     // returns 1 (the cos of 0 degrees)
```
[Back to Top](#javascript-cheatsheet)

## Boolean
```js
Boolean(10 > 9)       // returns true
(10 > 9)              // also returns true
10 > 9                // also returns true

var x = 0;
Boolean(x);       // returns false

var x = -0;
Boolean(x);       // returns false

var x = "";
Boolean(x);       // returns false

var x;
Boolean(x);       // returns false

var x = null;
Boolean(x);       // returns false

var x = false;
Boolean(x);       // returns false
```
[Back to Top](#javascript-cheatsheet)

## Comparison Operators
```js
// Comparison Operators
==    (equal to)
===   (equal value and equal type)
!=    (not equal)
!==   (not equal value or not equal type)
>     (greater than)
<     (less than)
>=    (greater than or equal to)
<=    (less than or equal to)

Logical Operators
&&    (and)
||    (or)
!     (not)
```
[Back to Top](#javascript-cheatsheet)

## Conditional
### IF / Conditional Value
```js
var voteable = (age < 18) ? "Too young" : "Old enough";
// Conditional operator that assigns a value to a variable based on some condition.
```
### IF / Conditional Statement
```js
if (condition) { ... }

if (hour < 18) {
    greeting = "Good day";
} else {
    greeting = "Good evening";
}

if (time < 10) {
    greeting = "Good morning";
} else if (time < 20) {
    greeting = "Good day";
} else {
    greeting = "Good evening";
}
```
[Back to Top](#javascript-cheatsheet)

## Switch
```js
switch (new Date().getDay()) {
    case 0:
        day = "Sunday";
        break;
    case 1:
        day = "Monday";
        break;
    case 2:
        day = "Tuesday";
        break;
    case 3:
        day = "Wednesday";
        break;
    case 4:
        day = "Thursday";
        break;
    case 5:
        day = "Friday";
        break;
    case 6:
        day = "Saturday";
}
// Returns: Current Day of the Week

var x = "0";
switch (x) {
    case 0:
        text = "Off";
        break;
    case 1:
        text = "On";
        break;
    default:
        text = "No value found";
}
```
[Back to Top](#javascript-cheatsheet)

## Loops
### Loop (for) by number
```js
var length = 5;
for (index = 0; index < length; index++) {
    text += "The number is " + index + "<br>";
}
```
### Loop (for/in) properties of object
```js
var person = {fname:"John", lname:"Doe", age:25};

var text = "";
var x;
for (x in person) {
    text += person[x];
}
```

### Loop (while) condition
```js
while (i < 10) {
    text += "The number is " + i;
    i++;
}
```

### Loop (do/while) condition
```js
do {
    text += "The number is " + i;
    i++;
}
while (i < 10);
```

### Loop with Await/Async within
ForEach loops with async's and await's have some strange behaviour resulting in not waiting a running tasks out of order, which can be fixed using es6 .map and promise all functions as follows:

```js
const myArray = ["One", "Two", "Three"]

const myPromises = myArray.map(async (item) => {
  await myFunc('doSomeThing');
});

await Promise.all(myPromises);
```
[Back to Top](#javascript-cheatsheet)

## Break and Continue
```js
break;
// The break statement can also be used to jump out of a loop.  
// The break statement breaks the loop and continues executing the code after the loop.

continue;
// The continue statement breaks one iteration (in the loop),
// if a specified condition occurs, and continues with the next iteration in the loop.
```
[Back to Top](#javascript-cheatsheet)

## Error Handling
```js
try {
  // Block of code to try
}
catch(err) {
  // Block of code to handle errors
}
finally {
  // Block of code to be executed regardless of the try / catch result
}

throw "Too big";    // throw a text
throw 500;          // throw a number
// The throw statement allows you to create a custom error.

try {
  if(x == "") throw "empty";
  if(x > 10) throw "too high";
}
catch(err) {
  console.log(err);
  console.log(err.message);
}
```
[Back to Top](#javascript-cheatsheet)

## JSON
###  JSON as String
```js
var string = '[{ "firstName":"John" , "lastName":"Doe" }, { "firstName":"Anna" , "lastName":"Smith" }, { "firstName":"Peter" , "lastName":"Jones" }]'
```

###  String Parsed to usable JSON
```js
JSON.parse(string)
```

###  JSON convert to String
```js
JSON.stringify(json)
```

### Insert into JSON Array
```js
json[json.length] = { "firstName": "Sarah", "lastName": "Blogs" }

// or by Array index
json[2] = { "firstName": "Sarah", "lastName": "Blogs" }
```

### Update JSON in Array
```js
json[0].firstName = "Bob"
json[0].lastName = "Thomas"
```

### Delete JSON in Array
```js
// Delete 1 record a index [3]
json.splice(3, 1)

// Delete (But can cause issues  - Slice, Pop, Shift is better)
delete json[0]; // Changes the element in JSON to undefined
```

### View single JSON in Array by index
```js
// The object[index].fieldName
json[0].firstName
```

### View All JSON in Array via loop
```js
for (index in json) {
    console.log(json[index].firstName + " " + json[index].lastName)
}
```
[Back to Top](#javascript-cheatsheet)

## Fetch
### Fetch (GET) JSON
```js
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(JSON.stringify(myJson));
  });
```

### Fetch (POST) Extended header
```js
fetch('http://example.com/movies.json', {
        method: "POST", // *GET, POST, PUT, DELETE, etc.
        mode: "cors", // no-cors, cors, *same-origin
        cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
        credentials: "same-origin", // include, same-origin, *omit
        headers: {
            "Content-Type": "application/json; charset=utf-8",
            // "Content-Type": "application/x-www-form-urlencoded",
        },
        redirect: "follow", // manual, *follow, error
        referrer: "no-referrer", // no-referrer, *client
        body: JSON.stringify(data), // body data type must match "Content-Type" header
    })
    .then(response => response.json()); // parses response to JSON
```

### Fetch (POST / PUT) JSON
```js
var url = 'https://example.com/profile';
var data = {username: 'example'};

fetch(url, {
  method: 'POST', // or 'PUT'
  body: JSON.stringify(data), // data can be `string` or {object}!
  headers:{
    'Content-Type': 'application/json'
  }
}).then(res => res.json())
.then(response => console.log('Success:', JSON.stringify(response)))
.catch(error => console.error('Error:', error));
```

### Fetch (POST) Multiple Files
```js
var formData = new FormData();
var photos = document.querySelector("input[type='file'][multiple]");

formData.append('title', 'My Vegas Vacation');
formData.append('photos', photos.files);

fetch('https://example.com/posts', {
  method: 'POST',
  body: formData
})
.then(response => response.json())
.then(response => console.log('Success:', JSON.stringify(response)))
.catch(error => console.error('Error:', error));
```
[Back to Top](#javascript-cheatsheet)

## LocalStorage
```js
var storage = window.localStorage; // Set Storage (5MB LIMIT !!!)

storage.setItem('Name', 'Tom'); // Add Data to a field/keys
storage.getItem('Name'); // Returns Tom
storage.removeItem('Name'); // Remove Data & Field / Key
storage.clear(); // Clear all items in localStorage
```

## DOM Document Object Model
### DOM Find the Element
```js
document.getElementById(id);
document.getElementsByTagName(name);
document.getElementsByClassName(name);
document.querySelectorAll("p.intro"); // Returns all p with class intro.
document.forms["id"];
document.body;
document.head;
document.images;
document.links;
document.scripts;
document.title;

```

### DOM Change the Element
```js
var element = document.getElementById(id);

element.innerHTML =  "new html content"; // Change the inner HTML of an element
element.attribute = "new value"; // Change the attribute value of an HTML element
element.setAttribute(attribute, value); // Change the attribute value of an HTML element
element.style.property = "new style"; // Change the style of an HTML element
element.style.color = 'red'; // Add Colour to Style
```

### DOM Add and Delete Elements
```js
document.write("Hello"); // Write to Page.
document.createElement(element);	// Create an HTML element
document.removeChild(element);	// Remove an HTML element
document.appendChild(element);	// Add an HTML element
document.replaceChild(element);	// Replace an HTML element
document.write(text);  // Write into the HTML output stream
```

### DOM Node Tree
```js
// You can use the following node properties to navigate between nodes with JavaScript:
.parentNode
.childNodes[nodenumber]
.firstChild
.lastChild
.nextSibling
.previousSibling

var myTitle = document.getElementById("demo").firstChild.nodeValue;
var myTitle = document.getElementById("demo").childNodes[0].nodeValue;
```

### DOM Event Handlers
```js
var element = document.getElementById(id);

element.onclick = function(){ ... }
// Adding event handler code to an onclick event

element.addEventListener("click", function(){ alert("Hello World!"); });
element.addEventListener("click", myFunction);
element.addEventListener("mouseover", myFunction);
element.addEventListener("click", mySecondFunction);
element.addEventListener("mouseout", myThirdFunction);

window.addEventListener("resize", resizeFuction); // Window Resize Listen

element.removeEventListener("mousemove", myFunction); // Remove Listener

<button onclick="this.innerHTML = Date()">The time is?</button>
```



### DOM Alert / Popup
```js
window.alert("Hello World");
alert("Hello World");

window.confirm("sometext");
confirm("sometext");

confirm("sometext");

if (confirm("Press a button!")) {
    txt = "You pressed OK!";
} else {
    txt = "You pressed Cancel!";
}

window.prompt("sometext","defaultText");
prompt("sometext","defaultText");

var person = prompt("Please enter your name", "Harry Potter");
if (person == null || person == "") {
    txt = "User cancelled the prompt.";
} else {
    txt = "Hello " + person + "! How are you today?";
}

```

### DOM (BOM) Window / Screen Methods
```js
window.innerHeight; // Browser Height in pixels
window.innerWidth; // Browser Width in pixels

window.open(); // open a new window
window.close(); // close the current window
window.moveTo(); // move the current window
window.resizeTo(); // resize the current window

screen.width
screen.height
screen.availWidth
screen.availHeight
screen.colorDepth
screen.pixelDepth
```

### DOM (BOM) Locations and History
```js
window.location.href // property returns the URL of the current page
window.location.hostname // property returns the name of the internet host
window.location.pathname // property returns the pathname of the current page
window.location.protocol // property returns the web protocol of the page
window.location.port // property returns the number of the internet host port
window.location.assign("https://URL") // method loads a new document

window.history.back() // method loads the previous URL in the history list
window.history.forward() // method loads the next URL in the history list
```

### DOM Window / Browser Navigator
```js
navigator.appName // Returns the application name of the browser
navigator.appCodeName // Returns the application code name of the browser
navigator.appVersion // Returns version information about the browser
navigator.platform // Returns the browser platform (operating system)
navigator.cookieEnabled // Returns true if cookies are enabled, otherwise false
navigator.product // Returns Browser Engine
navigator.userAgent // Returns the user-agent header sent by the browser to the server
navigator.language // Returns the browser's language

navigator.onLine // onLine property returns true if the browser is online
navigator.offLine // offLine property returns true if the browser is offline

navigator.javaEnabled() // Returns true if Java is enabled
```
[Back to Top](#javascript-cheatsheet)

### DOM Events
<table>
	<tbody>
		<tr>
			<th>Event</th>
			<th>Description</th>
			<th>Belongs To</th>
		</tr>
		<tr>
			<td>abort</td>
			<td>The event occurs when the loading of a media is aborted</td>
			<td>UiEvent, Event</td>
		</tr>
		<tr>
			<td>afterprint</td>
			<td>The event occurs when a page has started printing, or if the print dialogue box has been closed</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>animationend</td>
			<td>The event occurs when a CSS animation has completed</td>
			<td>AnimationEvent</td>
		</tr>
		<tr>
			<td>animationiteration</td>
			<td>The event occurs when a CSS animation is repeated</td>
			<td>AnimationEvent</td>
		</tr>
		<tr>
			<td>animationstart</td>
			<td>The event occurs when a CSS animation has started</td>
			<td>AnimationEvent</td>
		</tr>
		<tr>
			<td>beforeprint</td>
			<td>The event occurs when a page is about to be printed</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>beforeunload</td>
			<td>The event occurs before the document is about to be unloaded</td>
			<td>UiEvent, Event</td>
		</tr>
		<tr>
			<td>blur</td>
			<td>The event occurs when an element loses focus</td>
			<td>FocusEvent</td>
		</tr>
		<tr>
			<td>canplay</td>
			<td>The event occurs when the browser can start playing the media (when it has buffered enough to begin)</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>canplaythrough</td>
			<td>The event occurs when the browser can play through the media without stopping for buffering</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>change</td>
			<td>The event occurs when the content of a form element, the selection, or the checked state have changed (for , , and textarea)
			<td>Event</td>
		</tr>
		<tr>
			<td>click</td>
			<td>The event occurs when the user clicks on an element</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>contextmenu</td>
			<td>The event occurs when the user right-clicks on an element to open a context menu</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>copy</td>
			<td>The event occurs when the user copies the content of an element</td>
			<td>ClipboardEvent</td>
		</tr>
		<tr>
			<td>cut</td>
			<td>The event occurs when the user cuts the content of an element</td>
			<td>ClipboardEvent</td>
		</tr>
		<tr>
			<td>dblclick</td>
			<td>The event occurs when the user double-clicks on an element</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>drag</td>
			<td>The event occurs when an element is being dragged</td>
			<td>DragEvent</td>
		</tr>
		<tr>
			<td>dragend</td>
			<td>The event occurs when the user has finished dragging an element</td>
			<td>DragEvent</td>
		</tr>
		<tr>
			<td>dragenter</td>
			<td>The event occurs when the dragged element enters the drop target</td>
			<td>DragEvent</td>
		</tr>
		<tr>
			<td>dragleave</td>
			<td>The event occurs when the dragged element leaves the drop target</td>
			<td>DragEvent</td>
		</tr>
		<tr>
			<td>dragover</td>
			<td>The event occurs when the dragged element is over the drop target</td>
			<td>DragEvent</td>
		</tr>
		<tr>
			<td>dragstart</td>
			<td>The event occurs when the user starts to drag an element</td>
			<td>DragEvent</td>
		</tr>
		<tr>
			<td>drop</td>
			<td>The event occurs when the dragged element is dropped on the drop target</td>
			<td>DragEvent</td>
		</tr>
		<tr>
			<td>durationchange</td>
			<td>The event occurs when the duration of the media is changed</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>ended</td>
			<td>The event occurs when the media has reach the end (useful for messages like "thanks for listening")</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>error</td>
			<td>The event occurs when an error occurs while loading an external file </td>
			<td>ProgressEvent, UiEvent, Event</td>
		</tr>
		<tr>
			<td>focus</td>
			<td>The event occurs when an element gets focus</td>
			<td>FocusEvent</td>
		</tr>
		<tr>
			<td>focusin</td>
			<td>The event occurs when an element is about to get focus</td>
			<td>FocusEvent</td>
		</tr>
		<tr>
			<td>focusout</td>
			<td>The event occurs when an element is about to lose focus</td>
			<td>FocusEvent</td>
		</tr>
		<tr>
			<td>fullscreenchange</td>
			<td>The event occurs when an element is displayed in fullscreen mode</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>fullscreenerror</td>
			<td>The event occurs when an element can not be displayed in fullscreen mode</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>hashchange</td>
			<td>The event occurs when there has been changes to the anchor part of a URL</td>
			<td>HashChangeEvent</td>
		</tr>
		<tr>
			<td>input</td>
			<td>The event occurs when an element gets user input</td>
			<td>InputEvent, Event</td>
		</tr>
		<tr>
			<td>invalid</td>
			<td>The event occurs when an element is invalid</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>keydown</td>
			<td>The event occurs when the user is pressing a key</td>
			<td>KeyboardEvent</td>
		</tr>
		<tr>
			<td>keypress</td>
			<td>The event occurs when the user presses a key</td>
			<td>KeyboardEvent</td>
		</tr>
		<tr>
			<td>keyup</td>
			<td>The event occurs when the user releases a key</td>
			<td>KeyboardEvent</td>
		</tr>
		<tr>
			<td>load</td>
			<td>The event occurs when an object has loaded</td>
			<td>UiEvent, Event</td>
		</tr>
		<tr>
			<td>loadeddata</td>
			<td>The event occurs when media data is loaded</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>loadedmetadata</td>
			<td>The event occurs when meta data (like dimensions and duration) are loaded</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>loadstart</td>
			<td>The event occurs when the browser starts looking for the specified media</td>
			<td>ProgressEvent</td>
		</tr>
		<tr>
			<td>message</td>
			<td>The event occurs when a message is received through the event source</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>mousedown</td>
			<td>The event occurs when the user presses a mouse button over an element</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>mouseenter</td>
			<td>The event occurs when the pointer is moved onto an element</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>mouseleave</td>
			<td>The event occurs when the pointer is moved out of an element</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>mousemove</td>
			<td>The event occurs when the pointer is moving while it is over an element</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>mouseover</td>
			<td>The event occurs when the pointer is moved onto an element, or onto one of its children</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>mouseout</td>
			<td>The event occurs when a user moves the mouse pointer out of an element, or out of one of its children</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>mouseup</td>
			<td>The event occurs when a user releases a mouse button over an element</td>
			<td>MouseEvent</td>
		</tr>
		<tr>
			<td>mousewheel</td>
			<td>Deprecated. Use the wheel event instead</td>
			<td>WheelEvent</td>
		</tr>
		<tr>
			<td>offline</td>
			<td>The event occurs when the browser starts to work offline</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>online</td>
			<td>The event occurs when the browser starts to work online</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>open</td>
			<td>The event occurs when a connection with the event source is opened</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>pagehide</td>
			<td>The event occurs when the user navigates away from a webpage</td>
			<td>PageTransitionEvent</td>
		</tr>
		<tr>
			<td>pageshow</td>
			<td>The event occurs when the user navigates to a webpage</td>
			<td>PageTransitionEvent</td>
		</tr>
		<tr>
			<td>paste</td>
			<td>The event occurs when the user pastes some content in an element</td>
			<td>ClipboardEvent</td>
		</tr>
		<tr>
			<td>pause</td>
			<td>The event occurs when the media is paused either by the user or programmatically</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>play</td>
			<td>The event occurs when the media has been started or is no longer paused</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>playing</td>
			<td>The event occurs when the media is playing after having been paused or stopped for buffering</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>popstate</td>
			<td>The event occurs when the window's history changes</td>
			<td>PopStateEvent</td>
		</tr>
		<tr>
			<td>progress</td>
			<td>The event occurs when the browser is in the process of getting the mediadata (downloading the media)</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>ratechange</td>
			<td>The event occurs when the playing speed of the media is changed</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>resize</td>
			<td>The event occurs when the document view is resized</td>
			<td>UiEvent, Event</td>
		</tr>
		<tr>
			<td>reset</td>
			<td>The event occurs when a form is reset</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>scroll</td>
			<td>The event occurs when an element's scrollbar is being scrolled</td>
			<td>UiEvent, Event</td>
		</tr>
		<tr>
			<td>search</td>
			<td>The event occurs when the user writes something in a search field input="search"</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>seeked</td>
			<td>The event occurs when the user is finished moving/skipping to a new position in the media</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>seeking</td>
			<td>The event occurs when the user starts moving/skipping to a new position in the media</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>select</td>
			<td>The event occurs after the user selects sometext (for input and textarea)</td>
			<td>UiEvent, Event</td>
		</tr>
		<tr>
			<td>show</td>
			<td>The event occurs when a menu element is shown as a context menu</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>stalled</td>
			<td>The event occurs when the browser is trying to get media data, but data is not available</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>storage</td>
			<td>The event occurs when a Web Storage area is updated</td>
			<td>StorageEvent</td>
		</tr>
		<tr>
			<td>submit</td>
			<td>The event occurs when a form is submitted</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>suspend</td>
			<td>The event occurs when the browser is intentionally not getting media data</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>timeupdate</td>
			<td>The event occurs when the playing position has changed (like when the user fast forwards to a different point in the media)</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>toggle</td>
			<td>The event occurs when the user opens or closes the details element</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>touchcancel</td>
			<td>The event occurs when the touch is interrupted</td>
			<td>TouchEvent</td>
		</tr>
		<tr>
			<td>touchend</td>
			<td>The event occurs when a finger is removed from a touch screen</td>
			<td>TouchEvent</td>
		</tr>
		<tr>
			<td>touchmove</td>
			<td>The event occurs when a finger is dragged across the screen</td>
			<td>TouchEvent</td>
		</tr>
		<tr>
			<td>touchstart</td>
			<td>The event occurs when a finger is placed on a touch screen</td>
			<td>TouchEvent</td>
		</tr>
		<tr>
			<td>transitionend</td>
			<td>The event occurs when a CSS transition has completed</td>
			<td>TransitionEvent</td>
		</tr>
		<tr>
			<td>unload</td>
			<td>The event occurs once a page has unloaded (for <body>)</td>
			<td>UiEvent, Event</td>
		</tr>
		<tr>
			<td>volumechange</td>
			<td>The event occurs when the volume of the media has changed (includes setting the volume to "mute")</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>waiting</td>
			<td>The event occurs when the media has paused but is expected to resume (like when the media pauses to buffer more data)</td>
			<td>Event</td>
		</tr>
		<tr>
			<td>wheel</td>
			<td>The event occurs when the mouse wheel rolls up or down over an element</td>
			<td>WheelEvent</td>
		</tr>
	</tbody>
</table>

