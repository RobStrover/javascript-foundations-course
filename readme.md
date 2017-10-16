#Front End Masters JavaScript Foundations Course
My work and learning from the JS foundations course that can be found on frontendmasters.
##Scopes and Closures
* Nested Scope
* Hoisting - doesn't actually exist... just a metaphor.
* Closure
* Modules
###Scope - Where to look for things
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
    bam = "yay";
}
```
[see example 1](section1/script.js)

```javascript
var foo = "bar";
```
A **formal declaration** as there is the 'var' keyword.

```javascript
function bar() {
```
A **formal function declaration** due to the 'function' keyword.

The scope manager is interrogated to find if any of the formal declarations have already been declared. Informal declaration and assigned values are ignored for now.