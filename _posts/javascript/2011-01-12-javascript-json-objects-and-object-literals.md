---
layout: post
title: JavaScript, JSON objects and object literals
src: [http://json.org/]
type: [text]
categories: [javascript, json]
---


This is my crusade against the inconsiderate usage of the term *JSON object* in the context of JavaScript.

### JSON and JSON objects

JSON objects are [defined](http://json.org/) as follows:

<img src="http://json.org/object.gif" alt="JSON object" width="100%" />

Where `value` can be:

- a string (`"string"`)
- a number (`42`)
- another object (`{"key": ...}`)
- an array (`[42, "string", {...}, ...]`)
- `true`
- `false`
- `null`

So this is a valid JSON string with a JSON object:

    {"foo": "bar", "baz": 0}


### Object literals in JavaScript

Some people think, the [object literals](https://developer.mozilla.org/en/Core_JavaScript_1.5_Guide/Core_Language_Features#Object_Literals) 
in JavaScript and JSON objects are the same and use the latter to describe the former. 
But that is wrong. The syntax for JSON objects is just a *subset* of the possible syntax for JavaScript 
objects. The object literal notation was just the model for JSON.

This is **not** a JSON object:

    var foo =  {foo: "bar", baz: 0};

It is an object literal. It would be only valid to talk about a JSON object in this case:

     var foo = '{"foo": "bar", "baz": 0}';

because now here we have a **string** that contains JSON and a JSON object.


### The JSON object in JavaScript

Using the term JSON object for object literal notation seems to be even more wrong considering that 
*there actually exists a JSON object in JavaScript*. At least in the newer browsers. It is an object, 
available via the global variable [`JSON`](https://developer.mozilla.org/en/json) and provides the methods

- `JSON.parse` to decode JSON data into JavaScript objects
- `JSON.stringify` to encode JavaScript values (primitive values, arrays and objects)
   into JSON data (if possible)


### Bottom line

Don't use the term  *JSON object* to describe JavaScript objects defined with *object literal notation*. 
It is misleading and confusing.

---

### Further information:

- [There's no such thing as a "JSON Object"](http://benalman.com/news/2010/03/theres-no-such-thing-as-a-json/)
