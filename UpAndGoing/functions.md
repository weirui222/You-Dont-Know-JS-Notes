#Functions as values

```JS

var foo = function() {
  // ..
};

```
The above function expression is assigned to the foo variable and is called an **anonymous function** because it has no name.

```JS
var x = function bar(){
  //..
};

```
The above function expression is **named** (bar), even as a reference to it is also assigned to the x variable. **Named function expressions** are generally more preferable, though **anonymous function expressions** are extremely common.

#Immediatley Invoked Function Expressions(IIFEs)

In the previous snippet, neither of the function expressions are executed-we could if we had included foo() or x(), for instance.

```JS

(function IIFE(){
  console.log('Hello!')
})();
// 'Hello!'

```
The outer ( .. ) that surrounds the (function IIFE(){ .. }) function expression is just a nuance of JS grammar needed to pre‐ vent it from being treated as a normal function declaration.

The final () on the end of the expression—the })(); line—is what actually executes the function expression referenced immediately before it.
That may seem strange, but it’s not as foreign as first glance. Con‐ sider the similarities between foo and IIFE here:

```JS

function foo() { .. }
// `foo` function reference expression, 
// then `()` executes it
foo();

// `IIFE` function expression, // then `()` executes it (function IIFE(){ .. })();

```

As you can see, listing the (function IIFE(){ .. }) before its exe‐ cuting () is essentially the same as including foo before its execut‐ ing (); in both cases, the function reference is executed with () immediately after it.

Because an IIFE is just a function, and functions create variable scope, using an IIFE in this fashion is often used to declare variables that won’t affect the surrounding code outside the IIFE:

```JS

var a = 42;
(function IIFE(){ var a = 10;
        console.log( a );       //10
    })();
console.log( a );               //42

```








