---
layout: post
title: Fixing jQuery's Opacity in Firefox Extensions
subtitle: Fixing jQuery's Opacity in Firefox Extensions
type:
- text
- script
---

### Update (19.07.2012)

The solution below does not seem to work anymore since jQuery 1.7. [This 
Stack Overflow answer](http://stackoverflow.com/questions/693174/is-it-possible-to-use-jquery-to-manipulate-xul-elements/9071148#9071148) seems to provide a working solution.

---


There is some [information][1] , [more][4] or [less][2] elaborated,  about how to integrate [jQuery][3] in your own Firefox extension.

I managed to get it working too, but I recognized one problem: I was not able to set the opacity of an element via `$(el).css('opacity', 0.5)` nor did the effects like `.fadeIn()` or `.fadeOut()` work.  
It took some time for me to figure it out (couldn't  find anything on the web about it):

When jQuery loads, it checks, which features are supported by the browser. It seems that the check for opacity fails due the fact, that jQuery runs in another context.    
If it is loaded via an overlay for the browser

    <overlay id="extensionID" xmlns="http://www.mozilla.org/keymaster/gatekeeper/there.is.only.xul">
        <!-- whatever here... -->
        <script type="text/javascript" src="chrome://extension/content/jquery.js" />
    </overlay>

`window` is not pointing to the top element of the current page but to the whole browser window. This seems to make at least the *opacity* test fail

In order to manipulate the current HTML document with jQuery from your extension and to get opacity working, you have to keep two things in mind:

1. Before you use jQuery in your extension, set

        jQuery.supported.opacity = true;
        
   This enables opacity.

2. Provide the correct context to jQuery when accessing elements from the current website. The `document` element of the focused website is accessible via `window.content.document`. Thus, to select an element with ID `foo`, you have to do

        $('#foo', window.content.document)




[1]: http://stackoverflow.com/questions/491490/how-to-use-jquery-in-firefox-extension
[2]: http://gluei.com/blog/view/using-jquery-inside-your-firefox-extension
[3]: http://jquery.com
[4]: http://forum.jquery.com/topic/jquery-1-4-2-inside-a-firefox-extension
