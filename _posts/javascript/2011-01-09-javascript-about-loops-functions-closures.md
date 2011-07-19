---
layout: post
title: "JavaScript: About loops, functions and closures"
type: [text]
categories: [javascript, functions, scope, closures]
---

On [stackoverflow.com](http://stackoverflow.com/), nearly every day (at least it feels like so) 
I find questions about why some JavaScript code gives unexpected results - 
and the reason is almost always the same.

Let me take you on a short journey about loops, functions and closures.

**The loop**

A `for` loop in JavaScript (JS) seems to be the same as a `for` loop in C or 
Java, but it is not. It is actually more like in PHP. But that is another topic.

The most important thing to know about loops in JS is that they do 
**not create a scope**. JS does not have block [scope](http://en.wikipedia.org/wiki/Scope_%28programming%29), 
only function scope. What does this mean?

Consider the following snippet:

    function foo() {
        var bar = 1;
        for(var i = 0; i < 42; i++) {
            var baz = i;
        }
        /* more code */
    }

It is clear that `bar` will be available in the whole function. 
But `baz` (and `i`) will be too! There is only one difference: Until the first 
iteration of the loop, `baz` will have the value `undefined`. After the loop, 
it will have the value `41` (and `i` will be `42`).

So any variable you declare anywhere in a function will be available everywhere 
in the function, but it will only have a value after one was assigned to it. The
above is equivalent to:

    function foo() {
        var bar = 1,
            baz, i;
        for(i = 0; i < 42; i++) {
            baz = i;
        }
        /* more code */
    }

Lets continue.

**The function**

Functions are powerful in JS. Because they are 
[first-class citizens](http://en.wikipedia.org/wiki/First-class_function), 
you can pass them around like any other value. Therefore you can also  create 
functions inside functions and return them, e.g.:

    function foo() {
        return function(x) {
            return 42*x;
        }
    }

Which brings us to the next point.

**Closures**

[Closures](http://en.wikipedia.org/wiki/Closure_%28computer_science%29) are nothing 
else then functions defined in other functions that are passed to some other 
context. They are called *closures* because they close over the local variables 
of the function they are defined in (i.e. they have access to the other 
functions *scope*).
  
Again an example:

    function foo(x) {
        return function() {
            return 42*x;
        }
    }

This time, `x` defined as parameter of `foo`, and `var bar = foo(2)();` would 
return `84`. The inner function, the function that is returned by `foo` has 
access to `x`.

**Aggregation**

Why is all this important? Because it gives those people who want to create 
functions inside loops and that depend on loop variables a hard time. Consider 
this snippet which assignes an `click` handler to various elements:

    // elements is an array of 3 DOM elements
    var values = ['foo', 'bar', 'baz'];

    for(var i = 0, l = elements.length; i < l; i++) {
        var data = values[i];
        elements[i].onclick = function() {
            alert(data);
        };
    }

What is the value they elements will `alert` when they are clicked? It will 
be the same for all, namely `baz`. Here is the reason again: By the time, the 
event handler is called, the `for` has already finished. JS has no block scope, 
i.e. all the handlers share a reference to *the same* `data` variable. After 
the loop, this value will be `values[2]` which is `baz`.
You can also think this way: Every variable declaration in a function creates 
one place in the memory to store the data. In the `for` this data is just 
changed over and over again, the position in the memory stays the same. Every 
event handler access the *same position in the memory*.

**How to solve this?**

The only solution is to introduce another scope that "captures" the current value 
of `data`. JS only has function scope. So we have to introduce another function. 
We could do something like:

    function createEventHandler(x) {
        return function() {
            alert(x);
        }
    }

    for(var i = 0, l = elements.length; i < l; i++) {
        var data = values[i];
        elements[i].onclick = createEventHandler(data);
    }

This works, because the value of `data` will be stored in the local scope of 
`createEventHandler` and this function is executed newly in every iteration.

This can be written shorter using *immediate executing functions*:

    for(var i = 0, l = elements.length; i < l; i++) {
        var data = values[i];
        elements[i].onclick = (function(x) {
               function() {
                   alert(x);
               };
        }(data));
    }

This might look special but it really is not. It is just substituting the 
`createEventHandler` by the function definition.

---

**Further information:**

- [JavaScript Closures for Dummies](http://blog.morrisjohns.com/javascript_closures_for_dummies.html)
- [MDC - Closures](https://developer.mozilla.org/en/JavaScript/Guide/Closures)
- [MDC - Functions](https://developer.mozilla.org/en/Core_JavaScript_1.5_Guide/Functions)
