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