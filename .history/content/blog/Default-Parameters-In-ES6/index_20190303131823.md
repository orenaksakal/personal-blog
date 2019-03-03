---
title: Default Parameters in ES6
date: '2019-03-03'
---


#### **WHAT IS A PARAMETER ?**
Parameter is a variable found in the **function definition**.
In compilation phase when we are on the line of a function definition with a parameter 
that parameter is registered to **function's lexical scope as a local variable**.

#### PARAMETER === LOCAL VARIABLE
Lets try to prove that parameters are local variables in ES5 within example below, we will be taking advantage of scopes and hoisting in ES6 while doing so;
  ```js
    var foo = 'global.foo';
    var baz = 'global.baz';
    
    function bar(foo) {
        console.log(foo, 1); // logs parameter.foo
        foo = 'local.foo'; // this assignment is only for local foo -
        // which is parameter, global foo variable is not affected by this

        console.log(foo, 2); // logs 'local.foo' because of line above
        baz = 'local.baz'; // this assignment affects global baz,
        // because there is no local scoped baz variable
        console.log(baz, 3);
    }
    
    bar('parameter.foo');
    console.log(foo, 4); // logs 'global.foo' to prove global foo is not affected
    console.log(baz, 5); // logs 'local.baz' to prove global baz was affected
```
So now we know its true that paramaters are local variables and if we invoke a function **without argument** (undefined argument) 
our parameter will be initialized as **undefined**. This can cause exceptions if we are not careful enough. We had a solution for this in ES5 with parameter assignmens, lets take a look to example below.


#### ES5 VERSION OF DEFAULT PARAMETERS
```js
    function greet(title, name){
        title = title || 'King'; // if title parameter is-
        // undefined (or falsy!) we immediately assign it to 'King'
	    name = name || 'Doe'; // if title parameter is-
        // undefined (or falsy!) we immediately assign it to 'King'
        console.log('Hello %s %s', title, name);
    }
    
    greet('Mr', 'Potato'); // logs 'Hello Mr Potato'
    greet(); // logs 'Hello King Doe'
```

We should make more strict checks and only assign default parameter if parameter is undefined also this is against default airbnb eslint rules as [no-param-reassign](https://eslint.org/docs/rules/no-param-reassign) so we have new syntactically sugared way of doing this in ES6:

#### ES6 VERSION OF DEFAULT PARAMETERS
```js
    let greet = (title = 'King', name = 'Doe') => {
        console.log('Hello %s %s', title, name);
    }
    
    greet('Lord', 'Potato'); // logs 'Hello Lord Potato'
    greet() // logs 'King Doe'
```

There are still few things we should be aware of while using default parameters in ES6

#### DEFAULT PARAMETERS ARE ASSIGNED ONLY IF ARGUMENT IS UNDEFINED !
#### YOU CAN ASSIGN FUNCTION INVOCATIONS TO DEFAULT PARAMETERS
```js
    function isRequired(field){
        throw new Error(field + ' is required');
    }
    
    function greet(title = isRequired('Mrs'), name) {
        console.log('Hello %s %s', title, name);
    }
    
    greet(); //logs Mrs undefined - because we did not cover name parameter in default parameter assignment.
```
#### DEFAULT PARAMETERS ARE AVAILABLE TO LATER DEFAULT PARAMETERS
You can use earlier declared paramters in default parameters as the example below
```js
    let autoPluralize = (name, pluralName = name + 's') => {
        console.log('Hello %s', pluralName);
    }
    
    autoPluralize('phone'); // PHONES
```
#### WATCHOUT
There is one pitfull using function executions as default parameters:
**Functions defined in function body will throw Referance Error**

```js
    function foo(bar = baz()) { // Referance Error since baz is defined in function's body
	    function baz() { return 'baz'; }
    }
        
    foo();
```

#### YOU CAN USE DECONSTRUCTURING IN DEFAULT PARAMETERS

```js
    function foo([bar, baz] = [1, 2]) {
      return bar + baz;
    }
    
    foo(); // logs 3 as default parameters are 1 and 2
    foo([3,4]); // logs 7 as expected
```

*03.03.2019 - Warsaw*
# 
### Sources you may find useful
https://www.sitepoint.com/es6-default-parameters/<br/>
https://www.youtube.com/watch?v=0_5Nns9YgjQ<br/>
https://codeburst.io/parameters-arguments-in-javascript-eb1d8bd0ef04<br/>
https://medium.freecodecamp.org/learn-es6-the-dope-way-part-iv-default-parameters-destructuring-assignment-a-new-es6-method-44393190b8c9