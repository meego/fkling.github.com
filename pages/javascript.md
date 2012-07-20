---
layout: default
menu: [javascript]
---

*This is my attempt of creating a quite comprehensive summary and reference for JavaScript and related technologies. It is not a finished guide, it will grow over time. Please feel free to add comments and let me know how this guide could be improved.*

The [Mozilla Developer Network] (https://developer.mozilla.org/) (MDN) is a community driven web site containing a lot of various useful information regarding web development.  
It is important to distinguish between JavaScript, the **language** as such, and the **environment** it is executed, which is the **browser** in most cases. Some technologies heavily used together with JavaScript in the browser are actually not part of JavaScript, but of the it's HTML and DOM implementation.

---

### JavaScript, the language

The [MDN JavaScript Guide](https://developer.mozilla.org/en/JavaScript/Guide) provides a very detailed introduction to JavaScript and is definitely a "must read". A few concepts are very essential in order to understand JavaScript and be able to use it to its full potential:

- [**Functions**](https://developer.mozilla.org/en/JavaScript/Guide/Functions) are [first-class objects](https://en.wikipedia.org/wiki/First-class_citizen), which means they can be created on the fly and passed as arguments to other functions.

- [**Scope**](https://developer.mozilla.org/en/JavaScript/Reference/Functions_and_function_scope) in JavaScript is only created by functions and not, unlike other languages, through [blocks](https://en.wikipedia.org/wiki/Block_%28programming%29). This is the reason for a common mistake, namely creating [closures](https://developer.mozilla.org/en/JavaScript/Guide/Closures) [in a loop](http://stackoverflow.com/questions/750486/javascript-closure-inside-loops-simple-practical-example).

- JavaScript is [**object**](https://developer.mozilla.org/en/JavaScript/Guide/Working_with_Objects)**-oriented**. Everything, apart from primitive data types, derives from the basic `Object` data type, even functions and arrays. Because of that, arrays are often misused in situations where plain objects would have been more appropriate and this leads to unexpected, but explainable behaviour.

- **`this`** is a special variable available in all functions. The value of `this` is not specified during definition time, but during runtime. How its value is determined is thoroughly decribed in the [MDN documentation](https://developer.mozilla.org/en/JavaScript/Reference/Operators/this).

- JavaScript does not follow a classical inheritance paradigm, but a [prototype based](http://en.wikipedia.org/wiki/Prototype-based_programming) one. It therefore does not provide classes, but so called *constructor functions*, which can be seen as object generators. ["Introduction to Object-Oriented JavaScript"](https://developer.mozilla.org/en/Introduction_to_Object-Oriented_JavaScript) provides more information about OOP in JavaScript.

Last but not least, the [ECMAScript specification](http://es5.github.com/), of which JavaScript is an implementation, gives a more technical description of the language. It is not easy to read, but necessary if someone wants to understand the inner workings of JavaScript.  
Here are some examples from Stack Overflow which uses the specification to explain certain behaviour:

- [Why does `('0' ? 'a' : 'b')` behave different than `('0' == true ? 'a' : 'b')`](http://stackoverflow.com/q/7496727/218196)
- [Why does `",,," == Array(4)` in Javascript?](http://stackoverflow.com/q/10905350/218196)
- [`'\n\t\r' == 0` is `true`?](http://stackoverflow.com/q/10376179/218196)
- [What is the difference between `if (!x)` and `if (x == null)`?](http://stackoverflow.com/q/5791158/218196)
- [`+0` and `-0` in JavaScript](http://stackoverflow.com/q/7223359/218196)

---

### DOM (**D**ocument **O**bject **M**odel)

Although JavaScript [found its way on servers](http://nodejs.org/), it is still mostly used for client side scripting in web browsers. In this context it is embedded in [HTML](https://developer.mozilla.org/en/HTML) and is utilized to manipulate the DOM generated from the HTML. Therefore it is crucial to understand what the DOM is, its termionolgy, and how it can be manipulated:

- [Relationships between DOM nodes](/blog/2011/09/20/relationship-in-the-dom)
- [MDN DOM introduction](https://developer.mozilla.org/en/DOM/About_the_Document_Object_Model) 
- [MDN DOM reference](https://developer.mozilla.org/en/DOM) 
- [W3C DOM specification](http://www.w3.org/DOM/)

The most common use of JavaScript with DOM is probably to react to user interaction (events). [quirksmode.org](http://quirksmode.org) provides a thorough [description](http://www.quirksmode.org/js/introevents.html) of the different ways the event handlers can be attached to DOM elements and also points out differences between browsers - invaluable for cross-browser support.

- [Inline event handlers](http://www.quirksmode.org/js/events_early.html) are added to DOM elements by assigning JavaScript code to certain attributes in the original HTML source code.

- [Traditional event handlers](http://www.quirksmode.org/js/events_tradmod.html) are JavaScript functions which are directly assigned to the properties of DOM elements. This approach might seem to be the same as inline event handlers. The difference is that with inline event handlers, the browser generates the event handler from the value of the HTML attribute, while with traditional event handlers, the function is explicitly created and assigned with JavaScript.

- In contrast to the previous ways, [advanced event handlers](http://www.quirksmode.org/js/events_advanced.html) allow the registration of multiple, independent event handlers for the same event and element. Unfortunately this is also the approach with the biggest browser differences. The W3C-standard method `addEventListener` does not exist in IE8 and below.

[Event order](http://www.quirksmode.org/js/events_order.html) describes how handlers for nested elements are resolved. The fact that events bubble up the DOM tree leads to sophisticated ways to deal with events, such as [event delegation](http://stackoverflow.com/questions/1687296/what-is-dom-event-delegation).

When creating cross-browser compatible code, knowing about the differences is vital. quirksmode.org provides tables with information about which methods and properties are supported by which browsers (<http://www.quirksmode.org/dom/events/index.html>, <http://www.quirksmode.org/dom/w3c_core.html>, <http://www.quirksmode.org/dom/w3c_html.html>).

Working with the plain DOM API can be daunting. There are several libraries such as [jQuery](http://jquery.com/) or [Prototype.js](http://www.prototypejs.org/) which provide convenient wrappers around the DOM API and more.
Still, to get most out of these libraries, you should understand the DOM.

---

### CSS (Cascading Style Sheets)

HTML only describes the **structure** of a document, **CSS** defines how a document should **look** like.

Like JavaScript, CSS is embedded into the HTML. The style of elements can also be manipulated with JavaScript.

For more information about CSS, have a look at the [MDN documentation](https://developer.mozilla.org/en/CSS).

---

<div id="disqus_thread"></div>
<script type="text/javascript">
    /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
    var disqus_shortname = 'fthoughts'; // required: replace example with your forum shortname
    var disqus_identifier = 'javascript_summary';

    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
         var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
         dsq.src = 'http://' + disqus_shortname + '.disqus.com/embed.js';
         (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
<a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>
