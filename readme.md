# Front End Masters JavaScript Foundations Course
My work and learning from the JS foundations course that can be found on frontend masters.
## Scopes and Closures
* Nested Scope
* Hoisting - doesn't actually exist... just a metaphor.
* Closure
* Modules

### Scope - Where to look for things
What are we looking for? Any variable! Where that variable 'shows up' is where its scope is.
Javascript is a compiled language. The compilation occurs just before run time. The Javascript process is a minimum of 2 passes of the code.
For example:
* First pass - compiles an intermediary file (IF). **scope is made here**
* Second pass - executes the IF.

**JavaScript has function scope only - ish**
consider the following:
```javascript
var foo = "bar";

function bar() {
    var foo = "baz";
}

function baz(foo) {
    foo = "bam";
    bam = "yay"; // error!
}
```
[see example 1](section1/script.js)
### Compiling of Function Scope

```javascript
var foo = "bar";
```
A **formal declaration** as there is the 'var' keyword.

```javascript
function bar() {
```
A **formal function declaration** due to the 'function' keyword.

The scope manager is interrogated to find if any of the formal declarations have already been declared. Informal declaration and assigned values are ignored for now.
#### Meta Information
Where a variable is defined is important as it affects what happens at runtime. There are two states:
Either the source or the target of an assignment.
Whether the variable is the target or the source of an assignment is stored for runtime.
### Code Execution - runtime
If a variable has not been declared explicitly by runtime then one will be implicitly created. Doing this in a global sense is never the right way to declare a variable. Using strict mode actually displays **reference errors** when this happens.
'Reference error variable is not defined'. This is not to be confused with 'variable is undefined' which is when a variable has been declared but has no value yet.
### Strict Mode
Should be used with all JavaScript now. You can force it into your code with the following at the top of the file:
```javascript
"use strict";
```
The reason for strict mode is to make you write code that runs in the fastest way possible. We want the code to be optimised.
Each file is in its own separate program. The global scope is shared between them but on the code that has "use strict" at the top of it will be processed in strict mode. Beware when gulping code together however.

Use strict can also be used on a function-by-function basis.
### Named Function Expressions
Most commonly you see:
```javascript
var clickHandler = function() {
```
This is referred to as an **anonymous function expression**. Less common is the **named function declaration** which is accessible inside of itself but not in the enclosing scope.
```javascript
var keyHandler = function keyHandler() {
```
#### What's the difference between the two?
The named function declaration is better. Why?
* Provides a reliable self-reference to the function - great for recursion.
* Easier to debug when it has a name. No more stack traces without function names.
* More self-documenting code.
## IIFE Pattern - Immediately Invoked Function Expression
consider this code:
```javascript
var foo = "foo";

// other code here

var foo = "foo2";
console.log(foo); // foo2

// other code here

console.log(foo); // foo2 -- oops!
```
In this instance, having a second left hand reference (var foo) is not a problem. But the value will be changed. A function declaration can easily fix this:
```javascript
var foo = "foo";

// other code here

function bob(){
    var foo = "foo2";
    console.log(foo); // foo2
}

bob();

// other code here

console.log(foo); // foo2 -- oops!
```
This is not the best however as the function bob is now in the enclosing scope, we may need this name later! We can use a pattern called an *IIFE* to solve this:
```javascript
var foo = "foo";

( function bob(){
    var foo = "foo2";
    console.log(foo);
} )();

console.log(foo); // foo
```
having the '()' after the function expressions immediately invokes the function. Variables can also be passed into the function here.
The IIFE pattern is also useful in loops:
```javascript
for (var i = 0; i < 5; i++) {
    (function IIFE(){
        var j = i;
        console.log(j);
    })();
    }
```
var j is recreated in each run of the for loop.
## Block Scoping
The IIFE pattern does have some downsides such as the meaning of the 'return' keyword being changed so a return statement will talk to the IIFE rather than the parent function.
Consider the following:
```javascript
function diff(x,y) {
    if (x > y) {
        var tmp = x;
        x = y;
        y = tmp;
    }
    return y - x;
}
```
The tmp variable is supposed to only be accessible within the if. We are stylistically trying to do this but we have no enforcement. We can use the 'let' keyword for enforcing in this way. The 'let' keyword attaches a variable to the scope of this if rather than the scope of a function. The if statement now has scope in the same way as the function, it didn't without this.
```javascript
function diff(x,y) {
    if (x > y) {
        let tmp = x;
        x = y;
        y = tmp;
    }
    return y - x;
}
```
### Explicit Let Block
Let does not replace var. It just gives us a way of enforcing what we stylistically choose in our code.
Let can also be used with just curly braces to provide scope in any part of JS code.
```javascript
function formatStr(str) {
    { let prefix, rest;
        prefix = str.slice( 0, 3);
        rest = str.slice( 3 );
        str = prefix.toUpperCase() + rest;
    }
    
    if (/^FOO:/.test( str )) {
        return str;
    }
    
    return str.slice( 4 );
}
```
* Let keywords can be repeated. An error will be thrown if a let keyword is used twice on a variable *
**See exercise2 here**
## Hoisting
variable and function declarations are "Hoisted". Function expressions are not hoisted however. This allows functions to be declared further down in the code and used at any point.
let variables do hoist however they do not initialise.
**See exercise3 here**
## Closure
Closure is the logical conclusion of lexical scope.
A closure can be removed by removing its references. Unbinding event listeners etc.
**See exercise4 here**
## Module Pattern
What isn't a module?
This isn't:
```javascript
var foo = {
    o: { bar: "bar" },
    bar() {
        console.log(this.o.bar);
    }
}

foo.bar();  // "bar"
```
We need to hide things that are not necessary to be seen to the outside world. This is **encapsulation**.
## Module Pattern ##
### Classic Module Pattern ###
```javascript
var foo = (function(){
    
    var o = { bar: "bar" };
    
    return {
        bar: function() {
            console.log(o.bar);
        }
    };
    
})();

foo.bar();  // "bar"
```
In the example above, it is not possible to reference ```foo.o``` because it is not in the return statement. Anything we want to be publicly accessible goes in the return statement. So (as shown) only ```foo.bar();``` will be accessible.
**The point of the classic module pattern** is to force you to think about the responsibility requirements of the code you write.

**Note** The ```return``` object is anonymous. Only an external reference to foo can reference it. This means no internal access to the public/external function is possible.
### Classic Module Pattern: Modified ###
```javascript
var foo = (function(){
    var publicAPI = {
        bar: function(){
            publicAPI.baz();
        },
        baz: function(){
            console.log("baz");
        }
    };
    return publicAPI;
})();

foo.bar();  // "baz"
```
Creating the publicAPI variable allows us to reference the function internally as well as externally.

There are two implicit characteristics for using the module pattern:
* There must be an outer enclosing function/scope that runs at least once (see the ending '''();'').
* There must be at least one internal function which is returned in the public api with closure over the internal state.
### ES6+ Module Pattern - dedicated syntax for the module pattern
foo.js
```javascript
var o = { bar: "bar" };

export function bar() {
    return o.bar;
};
```
then
```javascript
import { bar } from "bar.js";

bar();  // "bar"

import * as foo from "foo.js";

foo.bar();  // "bar"
```
**import** and **export** allows us to express what is public and what isn't.
Here we consider the file to be the actual model.

This means this pattern is suggesting that we load lots of individual js files into the browser instead of one. **HTTP2** allows us to achieve this with great performance.

It is better with HTTP2 to send lots of smaller resources concurrently. This means there is no good reason to adopt ES6  until you are on HTTP2.

