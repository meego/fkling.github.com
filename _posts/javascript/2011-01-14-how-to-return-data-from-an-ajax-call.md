---
layout: post
title: How to return data from an Ajax call?
type:
- text
---

That is another question that is frequently asked on [stackoverflow.com](http://stackoverflow.com/questions/5316697/jquery-return-data-after-ajax-call-success).

**The short answer:** You can't (if you don't want to give the benefits of Ajax a miss).

### Ajax

What does [Ajax](http://en.wikipedia.org/wiki/Ajax_(programming)) stand for? 
It stands for *Asynchronous JavaScript And XML* and the important word here is **asynchronous**.

Fetching the data from the server is not done in the normal program control flow, but parallel to it.

That is why, in one way or another, you specify a **callback** that handles the response from the server 
when it is available.

### Callbacks?

One could say the philosophy about [callbacks](http://en.wikipedia.org/wiki/Callbacks) is: 
*Don't call us, we call you*. You are not waiting for a function to return a value, but you provide 
another function that gets called as soon as this value is available.

So instead of doing

    var val = foo();
    // do some processing with val here

you do

    function callback(val) {
        // do some processing with val here
    }

    foo(callback);

This might change your the control flow and it can be tricky to convert non-callback code to callback, 
but it is quite powerful too. Typically, one would put all the code that has to process `val` into the 
callback.

### Getting Ajax into the game

Here is a telephone analogy: Imagine you are calling one of your colleagues (friends, fellow students, ...) 
and ask him to do a job for you that takes some time (let's say picking something up from somewhere). Now 
what would you do? Would you *wait on the phone* until the other person returns (e.g. waiting for an hour)? 
Or would you rather say: "Ey, know what? *Call me back* when you have my stuff. (Here is my number:...)"?

Some people seem to think that JavaScript is waiting for the call to finish (waiting on the phone) 
and wonder why code like

    function foo() {
        var result;
       $.get("<some url>", function(data) {
           result = data;
       });
       return result;
    }

    var val = foo();
    // do some processing with val here

does **not** work (I'm using [jQuery](http://jquery.com) in this example, `$.get()` just makes an 
GET Ajax request to `<some url>` and executes the callback passed as second parameter).

It is because JavaScript **does not wait** until the Ajax request is finished. When the function is 
called, it will call `$.get()`, which will setup and send the Ajax request, but returns *immediately*. 
Then the function `foo` returns, *before* the callback passed to `$.get()` is even called. I.e. the 
assignment `result = data;` was not executed yet and the function returns `undefined`.

In order to fix this, all the logic that should operate with `val` has to be put into the callback:

    function foo(callback) { 
        $.get("<some url>", callback); 
    }

    foo(function(val){
        // do some processing with val here
    });



### To be fair

There is a way to make the previously not-working code work. Ajax requests can be set up to be *not* 
asynchronous (is it a Jax request then? Or Sjax?). I.e. the execution will halt and continue once the 
response from the server was received.  
That said, this should be avoided by all means (there might be some edge cases where it is ok but it is 
not the normal case). One of the big advantages of Ajax *is* the asynchronicity.  Depending on how long 
it takes to get the response, using a synchronous request might freeze the user interface and you gain 
nothing but annoyed users.
