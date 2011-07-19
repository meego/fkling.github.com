---
layout: post
title: PHP and JSONP
type: [text, script]
categories: [php, json, jsonp]
---

Since PHP 5.2, with [`json_decode`](http://www.php.net/manual/en/function.json-decode.php), 
we have an easy way to decode [JSON](http://json.org/) data. But sometimes, the only data we can 
get is actually [JSONP](http://en.wikipedia.org/wiki/JSON#JSONP), that is JSON data wrapped in a 
function call like:

    callback({/*.. JSON data .. */});

It is not difficult to use some string functions to remove this "overhead". Here is an example 
implementation of `jsonp_decode` (which offers the same interface as `json_decode`):

    function jsonp_decode($jsonp, $assoc = false) { 
        // PHP 5.3 adds "depth" as third parameter to json_decode
        if($jsonp[0] !== '[' && $jsonp[0] !== '{') { // we have JSONP
            $jsonp = substr($jsonp, strpos($jsonp, '('));
        }
        return json_decode(trim($jsonp,'();'), $assoc);
    }

And example implementation for creating a JSONP string:

    function jsonp_encode($value, $function_name = 'cb') { 
        // 5.3 adds "options" as second parameter to json_encode
        return sprintf('%s(%s);', trim($function_name, '();'), json_encode($value));
    }

----

**Update:**

The code is now [available on GitHub](https://github.com/fkling/PHP-functions).
