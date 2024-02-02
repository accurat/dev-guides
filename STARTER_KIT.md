# ACCURAT – Newbie Starter Kit

## Immutability

**Reassignment**: reassignment consists of assigning a new value for a variable using the = operator

https://dev.to/boywithsilverwings/reassignment-vs-mutability-21ob
 
```js
var immutableString = "Hello";

// In the above code, a new object with string value is created.

immutableString = immutableString + "World";

// We are now appending "World" to the existing value.
```
On appending the "immutableString" with a string value, following events occur:

1. Existing value of "immutableString" is retrieved
2. "World" is appended to the existing value of "immutableString"
3. The resultant value is then allocated to a new block of memory
4. "immutableString" object now points to the newly created memory space
5. Previously created memory space is now available for garbage collection.

A **mutable object** is an object whose state can be modified after it is created.
**Immutables** are the objects whose state cannot be changed once the object is created.
Strings and Numbers are Immutable.

Objects and arrays, on the other hand, allow mutation, meaning the data structure can be changed. 

Say we have a user object like this:
```js
let user = { name: "James Doe", location: "Lagos" }
```
Next, let’s attempt to create a newUser object using those properties:
```js
let newUser = user
```
Now let’s imagine the first user changes location. It will directly mutate the user object and affect the newUser as well:
```js
user.location = "Abia"
console.log(newUser.location) // "Abia"
```
_[Questo era un esempio di mutation. Vale anche al contrario: creo un newUser e gli assegno l’oggetto user. Cambio poi una proprietà di newUser: anche in user risulterà cambiata perché puntano alla stessa area di memoria. Questo funziona anche se user e newUser sono delle const.]_

### Preventing mutation

We want to make sure that our object isn’t mutated. If we’re going to make use of a method, it has to return a new object. In essence, we need something called a **pure function.**

A pure function has two properties that make it unique:
* The value it returns is dependent on the input passed. 
* The returned value will not change as long as the inputs do not change.
It does not change things outside of its scope.

By using ```Object.assign()```, we can create a function that does not mutate the object passed to it. This will generate a new object instead by copying the second and third parameters into the empty object passed as the first parameter. Then the new object is returned.

_[The Object.assign() method copies all enumerable own properties from one or more source objects to a target object. It returns the modified target object.]_
```js
const updateLocation = (data, newLocation) => {
  return Object.assign({}, data, {
    location: newLocation
  })
}
```
_[Object.assign ha sempre bisogno del primo parametro, che sarà il nuovo oggetto risultante, mentre tutti gli altri parametri vengono usati in ordine (da sx a dx) per riempire il primo oggetto. In questo caso la chiave location è già presente in data quindi verrà sovrascritta da location: newLocation.]_
```js
const updateLocation = (data, newLocation) => {
  return {
    ...data,
    location: newLocation
  }
}
```

## Variable scoping (var, let, const)
https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript#variables

_[Qui invece stiamo ritornando un oggetto, in cui inseriamo le coppie chiave-valore presenti in data (grazie allo spread operator), e sovrascriviamo location. Le graffe dopo il return sono quelle dell’oggetto risultante.]_
An important difference between JavaScript and other languages like Java is that in JavaScript, **blocks do not have scope; only functions have a scope**. So if a variable is defined using var in a compound statement (for example inside an if control structure), it will be visible to the entire function. However, starting with ECMAScript 2015, let and const declarations allow you to create block-scoped variables.

## Arrow functions
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions?retiredLocale=it

## Parameter handling
### Default
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters

Default function parameters allow named parameters to be initialized with default values if no value or undefined is passed.
```js
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5, 2));
// expected output: 10

console.log(multiply(5));
// expected output: 5
```

### Rest
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters

The rest parameter syntax allows a function to accept an indefinite number of arguments as an array

_[Note: only the **last** parameter in a function definition can be a rest parameter]_

```js
function myFun(a,  b, ...manyMoreArgs) {
  console.log("a", a)
  console.log("b", b)
  console.log("manyMoreArgs", manyMoreArgs)
}

myFun("one", "two", "three", "four", "five", "six")

// Console Output:
// a, one
// b, two
// manyMoreArgs, ["three", "four", "five", "six"]
```
### Spread
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax

Any argument in the argument list can use spread syntax, and the spread syntax can be used multiple times.
```js
function myFunction(v, w, x, y, z) { }
let args = [0, 1];
myFunction(-1, ...args, 2, ...[3]);
```
**Copy an array**
```js
let arr = [1, 2, 3];
let arr2 = [...arr]; // like arr.slice()

arr2.push(4);
//  arr2 becomes [1, 2, 3, 4]
//  arr remains unaffected
```
**Concat arrays**
```js
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];

arr1 = [...arr1, ...arr2];
//  arr1 is now [0, 1, 2, 3, 4, 5]
```
**With object literals**
```js
let obj1 = { foo: 'bar', x: 42 };
let obj2 = { foo: 'baz', y: 13 };

let clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

let mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```

_[Note: **Rest syntax looks exactly like spread syntax.** In a way, rest syntax is the opposite of spread syntax. Spread syntax "expands" an array into its elements, while rest syntax collects multiple elements and "condenses" them into a single element.]_

## Template literals (template strings)

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals

**Untagged** template literals result in strings, which makes them useful for string interpolation (and multiline strings, since unescaped newlines are allowed).

**Tagged** template literals call a function (the tag function) with an array of any text segments from the literal followed by arguments with the values of any substitutions.

### Multi-line strings
```js
console.log(`string text line 1
string text line 2`);
// "string text line 1
// string text line 2"
```
### Expression interpolation
```js
let a = 5;
let b = 10;
console.log(`Fifteen is ${a + b} and
not ${2 * a + b}.`);
// "Fifteen is 15 and
// not 20."
```

### Tagged templates
A more advanced form of template literals are tagged templates.

Tags allow you to parse template literals with a function. The first argument of a tag function contains an array of string values. The remaining arguments are related to the expressions.

The tag function can then perform whatever operations on these arguments you wish, and return the manipulated string. (Alternatively, it can return something completely different, as described in one of the following examples.)

The name of the function used for the tag can be whatever you want.
```js
let person = 'Mike';
let age = 28;

function myTag(strings, personExp, ageExp) {
  let str0 = strings[0]; // "That "
  let str1 = strings[1]; // " is a "
  let str2 = strings[2]; // "."

  let ageStr;
  if (ageExp > 99){
    ageStr = 'centenarian';
  } else {
    ageStr = 'youngster';
  }

  // We can even return a string built using a template literal
  return `${str0}${personExp}${str1}${ageStr}${str2}`;
}

let output = myTag`That ${ person } is a ${ age }.`;

console.log(output);
// That Mike is a youngster.
```

## Destructuring assignment
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

The **destructuring assignment** syntax is a JavaScript expression that makes it possible to **unpack** values from arrays, or properties from objects, into **distinct variables.**

### Syntax
```js
let a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

({ a, b } = { a: 10, b: 20 });
console.log(a); // 10
console.log(b); // 20

({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```
The left-hand side of the assignment defines what values to unpack from the sourced variable.
```js
const [firstElement, secondElement] = list;
// is equivalent to:
// const firstElement = list[0];
// const secondElement = list[1];
```
For example:
```js
const x = [1, 2, 3, 4, 5];
const [y, z] = x;
console.log(y); // 1
console.log(z); // 2
```
### Assignmment separate from declaration
A variable can be assigned its value via destructuring, separate from the variable's declaration. You can first declare the variable and then assign its value via destructuring.
```js
let a, b;

[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

In an array destructuring from an array of length N specified on the right-hand side of the assignment, if the number of variables specified on the left-hand side of the assignment is **greater than N**, only the first N variables are assigned values. The values of the remaining variables will be **undefined**.

### Swapping variables
Two variable values can be swapped in one destructuring expression.
```js
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1

//swapping values inside an array
const arr = [1,2,3];
[arr[2], arr[1]] = [arr[1], arr[2]];
console.log(arr); // [1,3,2]
```
### Assigning the rest of an array to a variable
When destructuring an array, you can unpack and assign the remaining part of it to a variable using the **rest pattern**:
```js
const [a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]
```
### Object destructuring
```js
const user = {
    id: 42,
    isVerified: true
};

const {id, isVerified} = user;

console.log(id); // 42
console.log(isVerified); // true
```
_[Note: in the basic assignement the **object properties** must **match the new variable names**, else the assignment will be **undefined**]_
### Assignment separate from declaration
Like we have seen with arrays, a variable can be assigned its value with destructuring separate from its declaration.
```js
let a, b;

({a, b} = {a: 1, b: 2});
```
_[Note: The parentheses ( ... ) around the assignment statement are required when using object literal destructuring assignment without a declaration.]_
### Assigning to new variable names
A property can be unpacked from an object and assigned to a variable with a different name than the object property.
```js
const o = {p: 42, q: true};
const {p: foo, q: bar} = o;

console.log(foo); // 42
console.log(bar); // true
```
Here, for example, const {p: foo} = o takes from the object o the property named p and assigns it to a local variable named foo.

### Nested objects (exercise)
```js
const obj = {
  a: 1,
  b:{
    c:2
  }
}
```
#### Case 1
```js
const {a, b} = obj;
console.log(b) //{ c: 2 }
```
#### Case 2
```js
const {a, b: c} = obj;
console.log(c) //{ c: 2 }
```
#### Case 3
```js
const {a, b: {c}} = obj;
console.log(c) // 2
```
#### Case 4
```js
const {a, b:{c:second}} = obj;
console.log(second) // 2
```
### Unpacking fields from objects passed as a function parameter
```js
const user = {
  id: 2,
  fullName: {
    firstName: 'Pippo'
  }
};
function whois({id, fullName}) {
  const {firstName} = fullName;
  return `${id} is ${firstName}`;
}
console.log(whois(user)) // '2 is Pippo'

// To understand this, think of it as if we were doing
// const {id, fullName:{firstName}} = user
// console.log(id) // 2
// console.log(firstName) // 'Pippo'
```
### Nested object and array destructuring
```js
const metadata = {
  title: 'Scratchpad',
  translations: [
    {
      language: 'de',
      title: 'JavaScript-Umgebung'
    }
  ],
  url: '/en-US/docs/Tools/Scratchpad'
};

let {
  title: englishTitle, // rename
  translations: [
    {
       title: localeTitle, // rename
    },
  ],
} = metadata;

console.log(englishTitle); // "Scratchpad"
console.log(localeTitle);  // "JavaScript-Umgebung"
```
### Computed object property names and destructuring
Computed Property Names is an ES6 feature which allows the names of object properties in JavaScript object literal notation to be determined dynamically.
Example:
```js
const myPropertyName = 'c'

const myObject = {
  a: 5,
  b: 10,
  [myPropertyName]: 15
} 

console.log(myObject.c) // prints 15 
```
Destructuring example:
```js
let key = 'z';
const obj = {z: 'bar'};
let {[key]: foo} = obj; // [key] becomes z, then is renamed to foo

console.log(foo); // "bar"
```
## Modules
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

A module is just a file. One script is one module. As simple as that.

Modules can load each other and use special directives `export` and `import` to interchange functionality, call functions of one module from another one:

- `export` keyword labels variables and functions that should be accessible from outside the current module.
- `import` allows the import of functionality from other modules.

You can only use import and export statements inside modules, not regular scripts.
### Exporting module features
The first thing you do to get access to module features is export them. This is done using the `export` statement.

You can export functions, var, let, const, and classes. They need to be top-level items; you can't use export inside a function, for example.

The easiest way to use it is to place it **in front of any items you want exported** out of the module, for example:
```js
export const name = 'square';

export function draw(ctx, length, x, y, color) {
  ctx.fillStyle = color;
  ctx.fillRect(x, y, length, length);

  return {
    length: length,
    x: x,
    y: y,
    color: color
  };
}
```
A more convenient way of exporting all the items you want to export is to use a **single export statement** at the end of your module file, followed by a comma-separated list of the features you want to export wrapped in curly braces. For example:
```js
export { name, draw, reportArea, reportPerimeter };
```
### Importing module features
Once you've exported some features out of your module, you need to import them into your script to be able to use them. The simplest way to do this is as follows:
```js
import { name, draw, reportArea, reportPerimeter } from './modules/square.js';
```
_[Note: In some module systems, you can omit the file extension and the leading /, ./, or ../ (e.g. 'modules/square'). This doesn't work in native JavaScript modules.]_

Once you've imported the features into your script, you can use them just **like they were defined inside the same file**.

_[Note: Although imported features are available in the file, they are read only views of the feature that was exported. You cannot change the variable that was imported, but you can still modify properties similar to const._

_Additionally, these features are imported as live bindings, meaning that they can change in value even if you cannot modify the binding unlike const.]_

### Applying a module to an HTML page
This is very similar to how we apply a regular script to a page, with a few notable differences.

First of all, you need to include type="module" in the `<script>` element, to declare this script as a module. To import the main.js script, we use this:
```js
<script type="module" src="main.js"></script>
```
### Default exports and imports
The imports/exports we used until now are called **named** imports/exports, since each item (be it a function, const, etc.) has been referred to by its name upon export, and that name has been used to refer to it on import as well.


There is a type of export called the **default export**. Let's look at an example as we explain how it works. In our basic-modules _(see link at the start of the section)_ square.js you can find a function called `randomSquare()` that creates a square with a random color, size, and position. We want to export this as our default, so at the bottom of the file we write this:
```js
export default randomSquare;
```
Note the lack of curly braces.

We could instead prepend export default onto the function and define it as an anonymous function, like this:
```js
export default function(ctx) {
  ...
}
```
Over in our `main.js` file, we import the default function using this line:
```js
import randomSquare from './modules/square.js';
```
Again, note the lack of curly braces. This is because **there is only one default export allowed per module**, and we know that randomSquare is it. The above line is basically **shorthand** for:
```js
import {default as randomSquare} from './modules/square.js';
```
### Renaming imports and exports
Let's suppose we add other modules to draw shapes like triangles or circles. These shapes would probably have associated functions like draw(), reportArea(), etc.

If we tried to import different functions of the same name into the same top-level module file, we'd end up with conflicts and errors.

We've added circle.js and triangle.js modules to draw and report on circles and triangles.

Inside each of these modules, we've got features with the same names being exported, and therefore each module has the same export statement at the bottom:
```js
export { name, draw, reportArea, reportPerimeter };
```
When importing these into main.js, if we tried to use
```js
import { name, draw, reportArea, reportPerimeter } from './modules/square.js';
import { name, draw, reportArea, reportPerimeter } from './modules/circle.js';
import { name, draw, reportArea, reportPerimeter } from './modules/triangle.js';
```
we would get an error. 

Fortunately, inside your import and export statement's curly braces, you can use the keyword `as` along with a new feature name, to change the identifying name you will use for a feature inside the top-level module.

So, we can fix the problem by either renaming the imports or the exports.

#### Renaming imports
```js
// in square.js
export { name, draw, reportArea, reportPerimeter };

// in main.js
import { name as squareName,
         draw as drawSquare,
         reportArea as reportSquareArea,
         reportPerimeter as reportSquarePerimeter } from './modules/square.js';
```
#### Renaming exports
```js
// in square.js
export { name as squareName,
         draw as drawSquare,
         reportArea as reportSquareArea,
         reportPerimeter as reportSquarePerimeter };

// in main.js
import { squareName, drawSquare, reportSquareArea, reportSquarePerimeter } from './modules/square.js';
```
 Both methods would produce the same result, however it arguably makes more sense to **make the changes in the imports**, avoiding to edit the module. This especially makes sense when you are importing from third party modules that you don't have any control over.

### Module objects
An even better solution is to import each module's features inside a module object. The following syntax form does that:
```js
import * as Module from './modules/module.js';
```
This grabs all the exports available inside module.js, and makes them available as members of an object Module.

In the modules, the exports are all in the following simple form:
```js
export { name, draw, reportArea, reportPerimeter };
```
The imports on the other hand look like this:
```js
import * as Canvas from './modules/canvas.js';
import * as Square from './modules/square.js';
import * as Circle from './modules/circle.js';
import * as Triangle from './modules/triangle.js';
```
In each case, you can now access the module's imports underneath the specified object name, for example:
```js
let square1 = Square.draw(myCanvas.ctx, 50, 50, 100, 'blue');
Square.reportArea(square1.length, reportList);
Square.reportPerimeter(square1.length, reportList);
```
So you can now write the code just the same as before (as long as you include the object names where needed), and the imports are much neater.
## Classes
https://exploringjs.com/es6/ch_classes.html#sec_overview-classes

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes

## Promises
### Using promises
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

A Promise is an **object** representing the eventual completion or failure of an **asynchronous operation**.

Essentially, a promise is a returned object to which you attach callbacks, instead of passing callbacks into a function.

_[A callback is a function passed as an argument to another function. 
This technique allows a function to call another function]_
#### Example
Imagine a function, `createAudioFileAsync()`, which asynchronously generates a sound file given a configuration record and two callback functions, one called if the audio file is successfully created, and the other called if an error occurs.

Here's some code that uses createAudioFileAsync():
```js
function successCallback(result) {
  console.log("Audio file ready at URL: " + result);
}

function failureCallback(error) {
  console.error("Error generating audio file: " + error);
}

createAudioFileAsync(audioSettings, successCallback, failureCallback);
```
If createAudioFileAsync() were rewritten to return a promise, you would attach your callbacks to it instead:
```js
createAudioFileAsync(audioSettings).then(successCallback, failureCallback);
```
#### Guarantees

- Callbacks added with then() will never be invoked before the completion of the current run of the JavaScript event loop.
- These callbacks will be invoked even if they were added after the success or failure of the asynchronous operation that the promise represents.
- Multiple callbacks may be added by calling then() several times. They will be invoked one after another, in the order in which they were inserted.

#### Chaining
A common need is to execute two or more asynchronous operations back to back, where each subsequent operation starts when the previous operation succeeds, with the result from the previous step. We accomplish this by creating a **promise chain**.

Here's the magic: the `then()` function returns a new promise, different from the original:
```js
const promise = doSomething();
const promise2 = promise.then(successCallback, failureCallback);
```
or
```js
const promise2 = doSomething().then(successCallback, failureCallback);
```
Basically, each promise represents the completion of another asynchronous step in the chain.
```js
doSomething()
.then(result => doSomethingElse(result))
.then(newResult => doThirdThing(newResult))
.then(finalResult => {
  console.log(`Got the final result: ${finalResult}`);
})
.catch(failureCallback);
```
_Important_: **Always return results**, otherwise callbacks won't catch the result of a previous promise (with arrow functions () => x is short for () => { return x; }).

It is possible to chain after a failure, for example after a `catch`. This is useful to accomplish new actions even after an action failed in the chain.
```js
// previous code...

.catch(() => {
    console.error('Do that');
})
.then(() => {
    console.log('Do this, no matter what happened before');
});
```
### General info
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

https://web.dev/promises/

A Promise is a proxy for a value not necessarily known when the promise is created. It allows you to associate **handlers** with an asynchronous action's eventual **success** value or **failure** reason. 

This lets asynchronous methods return values like synchronous methods: instead of immediately returning the final value, **the asynchronous method returns a promise to supply the value at some point in the future**.

_[Synchronous methods immediately return their result, while asynchronous methods don't. So, we make them return a promise instead, and the final result will be returned after a certain amount of time if the promise is fulfilled.]_

A Promise is in one of these states:

- **pending**: initial state, neither fulfilled nor rejected.
- **fulfilled**: meaning that the operation was completed successfully.
- **rejected**: meaning that the operation failed.

A pending promise can either be **fulfilled with a value** or **rejected with a reason** (error). When either of these options happens, the associated handlers queued up by a promise's then method are called. 

Here's how you create a promise:
```js
var promise = new Promise(function(resolve, reject) {
  // do a thing, possibly async, then…

  if (/* everything turned out fine */) {
    resolve("Stuff worked!");
  }
  else {
    reject(Error("It broke"));
  }
});
```
The promise constructor takes one argument, a callback with two parameters, resolve and reject. Do something within the callback, perhaps async, then call resolve if everything worked, otherwise call reject.

Here's how you use that promise:
```js
promise.then(function(result) {
  console.log(result); // "Stuff worked!"
}, function(err) {
  console.log(err); // Error: "It broke"
});
```
`then()` takes two arguments, a callback for a success case, and another for the failure case. Both are optional, so you can add a callback for the success or failure case only.
#### Chained promises
The methods promise.then(), promise.catch(), and promise.finally() are used to associate further action with a promise that becomes settled.

As we mentioned before, the .then() method takes up to two arguments; the first argument is a callback function for the **resolved** case of the promise, and the second argument is a callback function for the **rejected** case. Each .then() returns a newly generated promise object, which can optionally be used for chaining.

Processing continues to the next link of the chain even when a .then() lacks a callback function that returns a Promise object. Therefore, a chain can safely omit every rejection callback function until the final .catch().

A `.catch()` is really just a .then() without a slot for a callback function for the case when the promise is resolved.


### async/await
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function

https://developers.google.com/web/fundamentals/primers/async-functions

An async function is a function declared with the async keyword, and the await keyword is permitted within them. The async and await keywords enable asynchronous, promise-based behavior to be written in a cleaner style, avoiding the need to explicitly configure promise chains.

#### Example: fetching an URL
Say we wanted to fetch a URL and log the response as text. Here's how it looks using promises:
```js
function logFetch(url) {
  return fetch(url)
    .then(response => response.text())
    .then(text => {
      console.log(text);
    }).catch(err => {
      console.error('fetch failed', err);
    });
}
```
And here's the same thing using async functions:
```js
async function logFetch(url) {
  try {
    const response = await fetch(url);
    console.log(await response.text());
  }
  catch (err) {
    console.log('fetch failed', err);
  }
}
```
Async functions **always** return a promise, whether you use await or not. That promise resolves with whatever the async function returns, or rejects with whatever the async function throws.

---
## Exercise: promises
https://www.youtube.com/watch?v=PoRJizFvM7s

### Synchronous vs asynchronous operation
We talk about an asynchronous operation when something  is happening but we don't want to wait until it's done to go on with other operations. An operation is instead synchronous when we wait until it is completed to move forward with other processing. 

For example, retrieving data from a server can take a couple of seconds, and we don't want to stall our program until that operation is complete.
### Simulating the fetching of posts from a server
#### Step 1
```js
const posts = [
  {title: "Post 1", body: "This is post 1"},
  {title: "Post 2", body: "This is post 2"}
];

function getPosts() {
  setTimeout(() => {
    let output ="";
    posts.forEach((post) => {
      output = output + `${post.body}
`;
    });
    console.log(output)
  }, 1000)
}

getPosts()

function addPost(post){
  setTimeout(() =>{
    posts.push(post)
  }, 2000)
}

addPost({title: "Post 3", body: "This is post 3"})
```
In this first step we are simulating the fetch of two posts from a server. We have a function that gets the posts, in which we use setTimeout to make the operation happen after one second. 

We also create a function to add a third post to the others. This operation takes two seconds. We call this function right after getPosts(): since addPost() takes two seconds to finish, the other one is faster so we won't see the third post printed.
```js
//output
'This is post 1
This is post 2
'
```
#### Step 2
Before promises, this kind of problem was handled thanks to **callbacks**. 
```js
const posts = [
  {title: "Post 1", body: "This is post 1"},
  {title: "Post 2", body: "This is post 2"}
];

function getPosts() {
  setTimeout(() => {
    let output ="";
    posts.forEach((post) => {
      output = output + `${post.body}
`;
    });
    console.log(output)
  }, 1000)
}

function addPost(post, callback){
  setTimeout(() =>{
    posts.push(post);
    callback();
  }, 2000)
}

addPost({title: "Post 3", body: "This is post 3"}, getPosts)
```

In this case we modify `addPost()` so that it takes a callback function as a parameter. Then we call that function after we pushed the new post to the posts array. 

When we call `addPost()`, we pass `getPosts` as the callback: after the new post is added, getPosts will be called and the full list will be printed correctly. The whole operation takes two seconds.
```js
//output
'This is post 1
This is post 2
This is post 3
'
```
#### Step 3
How do we obtain the same behaviour with **promises**?

We don't want to pass a callback function to `addPost()`, instead we want to make `getPosts()` return a Promise.

A Promise takes in a callback function with two parameters: **resolve** and **reject**. We want to do something in the promise, possibly an async operation, and then call resolve if that operation is successful or reject if it fails. 
```js
function addPost(post){
  return new Promise((resolve, reject) => {
    setTimeout(() =>{
      posts.push(post);

      //error checking, we assume there are none
      const error = false;

      if(!error){
        resolve();
      } else {
        reject('Error: Something went wrong!')
      }
    }, 2000)
  })

}

addPost({title: "Post 3", body: "This is post 3"})
  .then(getPosts)
  .catch(err => console.log(err))
```
In this example, the operation that happens within the promise is pushing the new post to the posts array. After that we resolve or reject. 

Now when we call `addPost()` it will return a promise. We use `then()` to do something with the promise's result after the promise is settled (either fulfilled or rejected). The action we want to do is `getPosts()`: if the promise was resolved we will get the three posts printed in the console, otherwise an error will be thrown. 

If we want that error to be handled correctly, we add a `catch()` after the then(), instructing it to handle errors that happened in the promise chain, if any (you can make the error occur by setting `const error = true;`)


#### Step 4
Now let's rewrite this code in a cleaner way using **async/await** instead of then().

Let's create an async function `init()` in which we will add the new post and, when that operation is done, print the post list. 
```js
async function init() {
  await addPost({title: "Post 3", body: "This is post 3"});
  getPosts();
}
init();
```

Calling `init()` does exactly the same thing as calling `addPost` the way we did earlier, using then().

#### Another example of then vs async/await using fetch()
Let's fetch a list of users from a REST API. We want to see the list in the console.

Using the then() chain:
```js
const fetchUsers = fetch('https://jsonplaceholder.typicode.com/users')
.then(res => res.json())
.then(data => console.log(data));
```
Using async/await:
```js
async function fetchUsers(){
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await res.json();
  console.log(data);
}
fetchUsers()
```
