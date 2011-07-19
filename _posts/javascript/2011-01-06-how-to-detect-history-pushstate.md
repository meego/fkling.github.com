---
layout: post
title: How to detect when HTML5's history.pushState() is called?
type: [text, script]
categories: [javascript, html5, history]
---

That was the question I had and 
[galambalazs](http://stackoverflow.com/users/378024/galambalazs) helped me to 
solve this question on [stackoverflow.com](http://stackoverflow.com/questions/4570093/how-to-get-notified-about-changes-of-the-history-via-history-pushstate).

---

**Problem description and background:**

The latest version of my Firefox add-on ([FloatNotes](http://www.floatnotes.org)), 
also listens to the event `hashchange`, which is raised when the 
[fragment-identifier](http://en.wikipedia.org/wiki/Fragment_identifier) of 
the [URL](http://en.wikipedia.org/wiki/Url) is changed. As you may know, this 
technique is used in Ajax-enabled websites (like Google Mail) to make URLs be 
part of the browsing history without reloading the page. So the idea is that if 
the hash has changed, the user actually visits another page.

Now, with HTML5, the [`window.history`](http://www.w3.org/TR/html5/history.html#the-history-interface) 
object gets new methods which take the previous mentioned idea further: 
`pushState` and `replaceState`. As the name suggests, `pushState` adds a new 
URL to existing history stack and (if the protocol and host stay the same) 
**does not trigger a reload of the page**. Most browsers will display the new 
URL in the address bar too so it really looks like you clicked a normal link, 
but the content is loaded via Ajax. You can test this using Firefox 4 or 
Chrome 8 and Facebook.

The problem is that calling `pushState` does generate any event (like changing 
the hash did) so loading of new content cannot be detected anymore.

---

**The solution**

I first tried to replace the whole history object with a custom object, but 
that did not work (probably `window.history` is readonly). 
Then [galambalazs](http://stackoverflow.com/users/378024/galambalazs) proposed 
to just replace the `pushState` method. Here is his code:

    (function(history){
        var pushState = history.pushState;
        history.pushState = function(state) {
            if (typeof history.onpushstate == "function") {
                history.onpushstate({state: state});
            }
            // ... whatever else you want to do
            // maybe call onhashchange e.handler
            return pushState.apply(history, arguments);
        }
    })(window.history);


So whenever the `pushState` method is called, you can trigger your own event 
handler.

It is a hack, but until an event is raised by default, this seems to be the 
only way to get notified when `history.pushState()` is called.
