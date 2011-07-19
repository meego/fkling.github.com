---
layout: post
title: jQuery Function Toggle Plugin
type:
- text
---

Already some while ago, I created a jQuery plugin which I call *jQuery Function Toggle*. 

The idea is that upon successive events (e.g. consecutive clicks), the plugin loops through functions and executes one for each event.

Example:

<iframe style="width: 100%; height: 300px" src="http://jsfiddle.net/fkling/4EHjM/embedded/js,html,result,resources">%nbsp;</iframe>

The function `funcToggle()` basically provides the same interface as `bind()`, with the difference that it accepts more than one function.

The source code is [available on GitHub](https://github.com/fkling/jQuery-Function-Toggle-Plugin).
