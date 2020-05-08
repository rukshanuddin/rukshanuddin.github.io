---
layout: post
title:      "Hoisting in JS"
date:       2020-05-08 20:18:32 +0000
permalink:  hoisting_in_js
---


What is *hoisting*? Hoisting is a relatively new concept, which you will not find anything about in documentation prior to ES6. According to [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting):

>Hoisting is a term you will not find used in any normative specification prose prior to ECMAScript® 2015 Language Specification. Hoisting was thought up as a general way of thinking about how execution contexts (specifically the creation and execution phases) work in JavaScript. However, the concept can be a little confusing at first.

Ok, so that is a little confusing. To put it simply, hoisting is a mechanism, that since ES5 will move variables to the top of its scope during "compilation" (_placed in quotes, since Javascript in considered a compiled language_) before code execution. Changes in how hoisting works were made with the ES6 release of Javascript as well, and I will document how it works.

No matter where functions and variables are declared, they are moved to the top of their scope (_global or local_).

This will let a few things occur in Javascript, including allowing you to call functions before they've been written in your code.

## How it works

### ES5

#### Global Variables

In ES5 thanks to the magic of hoisting, we could do this:

```javascript
console.log(myVar); // Output: undefined

var myVar = "ES5 hoisted me!";
```

Instead of getting **ReferenceError: myVar is undefined** for the **console.log(myVar)** we got ***undefined***.  

**Why did it return undefined?  
_Why didn't it return "ES5 hoisted me!"?_**  
_Did it not hoist myVar?_

All valid questions at this point. The interpreter will view the code as follows:

```javascript
var myVar; // myVar gets hoisted to top of scope

console.log(myVar); // myVar is not yet defined

var myVar = "ES5 hoisted me!"; // value not seen by the previous console.log()
```

While ES5 has hoisted **myVar** to the top of the scope, it has not been _initialized_ yet. The reason it is returning **undefined** is because only **var myVar** gets hoisted, not the initialization value of **"ES5 hoisted me!"** during the compilation phase. We need to make sure to _initialize_ our variables before we call them:

```javascript
var myVar = "ES5 hoisted me!";
console.log(myVar); // Output: ES5 hoisted me!
```

#### Function Scoped Variables

```javascript
function myFunction() {
    console.log(myVar);
    var myVar = "Hoisting is the bees knees!";
}

myFunction();
```

Based on our knowledge of _hoisting_ thus far, it is safe to assume that the scope of **myVar** is **myFunction()** and that function would return ***undefined***. The interpreter sees it as:

```javascript
function myFunction() {
    var myVar; // myVar gets hoisted to top of function
    console.log(myVar); // myVar is undefined at this point
    var myVar = "Hoisting is the bees knees!"; // console.log() does not see this value of myVar
}

myFunction(); // Output: undefined
```

Like the previous example, to prevent this all we need to do is _initialize_ the variable before calling it.

```javascript
function myFunction() {
    var myVar = "Hoisting is the bees knees!";
    console.log(myVar);
}

myFunction(); // Output: Hoisting is the bees knees!
```

#### Strict Mode

What is strict mode? With ES5 we have been given a way to be more mindful of how we declare variables. From MDN()

>JavaScript's **strict mode** is a way to opt in to a restricted variant of JavaScript, thereby implicitly opting-out of "[sloppy mode](https://developer.mozilla.org/en-US/docs/Glossary/Sloppy_mode)". Strict mode isn't just a subset: it intentionally has different semantics from normal code.
>
>Strict mode for an entire script is invoked by including the statement "use strict"; before any other statements.

We can get a lot of benefits from using **strict mode**. Strict mode will:

- Change _errors_ to __*throw errors*__.
  - Helps to remove "silent" Javascript errors.
- Strict mode repairs mistakes
  - Makes it easier for JavaScript engines to perform optimizations.
  - Identical code will sometimes execute faster in strict mode.
- Strict mode forbids certain JavaScript syntax.
- When relatively “unsafe” actions are taken, it prevents or throws errors.
  - (Such as gaining access to the global object).
- It restricts features that are unclear or poorly thought out.
- Strict mode makes it easier to write “secure” JavaScript code.

### ES6

#### let

```javascript
console.log(myVar);

let myVar = "ES6 hoisted me!";
```

Let's take the first example but use **let** instead of **var**. If we were going by the logic we had learned in the previous examples, we would assume that the **console.log()** would be ***undefined***. Instead, we get something else:

```javascript
console.log(myVar);

let myVar = "ES6 hoisted me!";

//Uncaught ReferenceError: myVar is not defined
```

What ES6 does is gives us a **ReferenceError**. ES6 will not let us use an undeclared variable. The **let** keyword means the variable's scope is bound to the block in which is it declared and not the function in which it is declared. It also means we need to explicitly declare **let** keyword variables before they are called.

```javascript
let myVar;

console.log(myVar); // Output: undefined

myVar = "ES6 hoisted me!";
```

This time we won't get a **ReferenceError**, but our output will be ***undefined***.

#### const

The **const** keyword is very much like let except one key difference. [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) defines it as the following:

>Constants are block-scoped, much like variables defined using the _let_ keyword. The value of a constant can't be changed through reassignment, and it can't be re-declared.

Just like with _let_, __const__ keyword variables are hoisted to the top of the block.

```javascript
console.log(myVar);

const myVar = "ES6 hoisted me!";

//ReferenceError: Cannot access 'myVar' before initialization
```

We will get ReferenceError just as we did with the **let** keyword. It works the same with **const** in functions:

```javascript
function hoistMe(user){
    console.log(result)
    const result = `${user}` + `has been hoisted`
}

hoistMe("Jonny")
```

We will ge an error:

```javascript
function hoistMe(user){
    console.log(result)
    const result = `${user}` + `has been hoisted`
}

hoistMe("Jonny") //Uncaught ReferenceError: Cannot access 'result' before initialization
```

We will also get errors for using a constant before declaring and initializing it.

## The differences between var, let, const

***The key difference between var, let and const as it relates to hoisting is that*** **let** ***and*** **const** ***remain uninitialized at execution while*** **var** ***declared variables are initialized with a value of undefined***
