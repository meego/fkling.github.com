---
layout: post
title: Getset objects in collections
type:
- text
- script
---

Quite often you find yourself in a situation where you have to an object to a collection
or change a certain property. The "normal" is to test via an `if` statement whether the
object exists or not:

    var obj = collection[someId];
    if(!obj) {
        obj = {};
        collection[someId] = obj;
    }
    // do something with object
    
This works, is readable, but can be annoying if you have to do this often.

The shorthand syntax for setting a default value for, lets say, a parameter is probably
well know:

    param = param || 'default';
    
Can we use this somehow in this situation too? Yes we can. "Luckily", the assignment operation
*returns* the value that was assigned to a variable:

> ##[11.13.1   Simple Assignment ( = )](http://ecma262-5.com/ELS5_HTML.htm#Section_11.13.1)
>
> The production *AssignmentExpression : LeftHandSideExpression = AssignmentExpression* is evaluated as follows:
>   
> 1. Let *lref* be the result of evaluating *LeftHandSideExpression*.
> 2. Let *rref* be the result of evaluating *AssignmentExpression*.
> 3. Let *rval* be GetValue(*rref*).
> 4. Throw a **SyntaxError** exception if the following conditions are all true:
>
>    - Type(*lref*) is Reference is **true**
>    - IsStrictReference(*lref*) is **true**
>    - Type(GetBase(*lref*)) is Enviroment Record
>    - GetReferencedName(*lref*) is either "**`eval`**" or "**`arguments`**"
>
> 5. Call PutValue(*lref*, *rval*).
> 6.Return *rval*.

*(Reference: <http://ecma262-5.com/ELS5_HTML.htm>)*

That means we can use this slightly less readble but much shorter version:

    var obj = collection[someId] || (collection[someID] = {});