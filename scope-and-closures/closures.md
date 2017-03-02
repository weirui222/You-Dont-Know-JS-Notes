#Scope Closures

##Enlightenment 

Closure is when a function is able to recognize and remeber its lexical scope even when tht function is executing code outside its lexical scope.

Lets jump into some code to illustrate that definition.

```JS

function bar() {
  var a = 2;

  function bar() {
    console.log( a ); //2
  }

  bar();
}

foo();

```

Let us then consider code that brings closure into full light:

```JS

function foo() {
  var a = 2;

  function bar() {
    console.log( a );
  }

  return bar; 

}

var baz = foo();

baz(); //2 -- whoa, closure just observed, man.

```
The function bar() has lexical scope access to the inner scope of foo(). But then, we take bar(), the function itself, and pass it as a value. In this case, we return the function object itself that bar refer‐ ences.

After we execute foo(), we assign the value it returned (our inner bar() function) to a variable called baz, and then we actually invoke baz(), which of course is invoking our inner function bar(), just by a different identifier reference.

bar() is executed, for sure. But in this case, it’s executed outside of its declared lexical scope.

After foo() executed, normally we would expect that the entirety of the inner scope of foo() would go away, because we know that the engine employs a garbage collector that comes along and frees up memory once it’s no longer in use. Since it would appear that the con‐ tents of foo() are no longer in use, it would seem natural that they should be considered gone.

But the “magic” of closures does not let this happen. That inner scope is in fact still in use, and thus does not go away. Who’s using it? The function bar() itself.

By virtue of where it was declared, bar() has a lexical scope closure over that inner scope of foo(), which keeps that scope alive for bar() to reference at any later time.

bar() still has a reference to that scope, and that reference is called closure.

So, a few microseconds later, when the variable baz is invoked (in‐ voking the inner function we initially labeled bar), it duly has access to author-time lexical scope, so it can access the variable a just as we’d expect.

The function is being invoked well outside of its author-time lexical scope. Closure lets the function continue to access the lexical scope it was defined in at author time.

Of course, any of the various ways that functions can be passed around as values, and indeed invoked in other locations, are all ex‐ amples of observing/exercising closure.

```JS

function foo() {
  var a = 2;

  function baz() {
    console.log( a );
  }

  bar( baz )  
}

function bar(fn) {
  fn(); // look, a closure!
}

```

We pass teh inner function baz over to bar, and call that inner function (labled fn now ) , and when we do, its closure over the inner scope of foo() is observed by accessing a.

These passings-around of functions can be indirect, too.

```JS

var fn;

function foo() {
  var a = 2;

  function baz() {
    console.log( a ); 
  }

  fn = baz; //assign baz to global variable 
}

function bar() {
  fn(); //closure
}

foo();
bar(); // 2

```

Whatever facility we use to transport an inner function outside of its lexical scope, it will maintain a scope reference to where it was originally delared, and wherever we can execute him, that closure will be exercised.


##Now I Can See

The previous code snippets are somewhat academic and artifically constructed to illustrate using closure. But I promised you something more than just a cool new toy. I promised that closure was something all around you in your existing code. Let us now see that truth.

```JS

function wait(message) {
  
  setTimeout(function timer(){
    console.log( message );
  }, 1000);
}

wait("hello, closure")

```

We take an inner function (named timer) and pass it to setTime out(..). But timer has a scope closure over the scope of wait(..), indeed keeping and using a reference to the variable message.

A thousand milliseconds after we have executed wait(..), and its inner scope should otherwise be long gone, that anonymous function still has closure over that scope.

Deep down in the guts of the engine, the built-in utility setTime out(..) has reference to some parameter, probably called fn or func or something like that. Engine goes to invoke that function, which is invoking our inner timer function, and the lexical scope reference is still intact.
Closure.



```JS

function setupBot (name, selector) {
  $( selector ).click( function activator (){
    console.log("Activating: " + name); 
  });
}

setupBot("Closure Bot 1", "#bot_1")
setupBot("Closure Bot 2", "#bot_2")

```

I am not sure what kind of code you write, but I regularly write code that is responsible for controlling an entire global drone army of clo‐ sure bots, so this is totally realistic!

(Some) joking aside, essentially whenever and wherever you treat functions (that access their own respective lexical scopes) as first-class values and pass them around, you are likely to see those functions exercising closure. Be that timers, event handlers, Ajax requests, cross-window messaging, web workers, or any of the other asynchronous (or synchronous!) tasks, when you pass in a callback function, get ready to sling some closure around!

#Loops and Closures

The most common canoical example used to illistrate closures involves the humble for loop

```JS

for(var i = 0; i <= 5, i++){
  setTimeout (function timer(){
    console.log(i);
  }, i * 1000); 
}


```

This doesnt work it just print 6 6 times instead of 1,2,3,4,5,6

Lets try this..

```JS

for (var i = 1; i <= 5; i++){
  (function(){
    setTimeout( function timer(){
      console.log( i );
    },1000 * i);
  })(); // IIFE
}

```

Does that work? 

I’ll end the suspense for you. Nope. But why? We now obviously have more lexical scope. Each timeout function callback is indeed closing over its own per-iteration scope created respectively by each IIFE.

It’s not enough to have a scope to close over if that scope is empty. Look closely. Our IIFE is just an empty do-nothing scope. It needs some‐ thing in it to be useful to us.

It needs its own variable, with a copy of the i value at each iteration.

```JS

for(var i = 1; i <= 5; i++){
  (function(){
    var j = i;

    setTimeout( function timer (){
        console.log( j );
      }, 1000 * j);
  })();
}

```

This works!

a slight variation that some prefer is..

```JS

for(var i = 1; i <= 5; i++){
  (function(j){
    setTimeout(function timer(){
      console.log( j );
    }, j * 1000);
  })( i );
}

```
Of course, since these IIFEs are just functions, we can pass in i, and we can call it j if we prefer, or we can even call it i again. Either way, the code works now.

The use of an IIFE inside each iteration created a new scope for each iteration, which gave our timeout function callbacks the opportunity to close over a new scope for each iteration, one which had a variable with the right per-iteration value in it for us to access.

Problem solved!


