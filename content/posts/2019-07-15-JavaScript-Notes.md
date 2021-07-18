+++
draft = false
date = 2019-07-15
title = "JavaScript Notes"
description = "Some notes for JavaScript"
tags = ["Programming Languages"]
+++

* [Basics](#basics)
* [Functions](#functions)
* [OOP](#oop)
* [DOM (Document Object Model)](#dom-document-object-model)
* [Handling events](#handling-events)
* [References](#references)

## Basics

1. Variables:
    * Global variables live as long as the page.
    * If we forget to declare a variable before using it, it’ll always be global even if we first use it in a function.
    * `let` creates clock-level variables.
    * `const` creates constants.
    * `var` is the traditional way of declaring variables.

        ```js
        // i visible here.
        for (var i = 0; i < 3; i++) {
            // i visible here.
        }
        // i visible here.
        // let i = 0 makes i only visible in the for loop.
        ```

2. `undefined` vs `null` vs `isNaN()`:
    * `undefined` is used for;
        * Unassigned/ Uninitiated variables;
        * A missing property for an object;
        * A missing value for an array;
    * `null` is used for uncreated objects (like `.getElementById()`'s returned value;)
    * `isNaN(foo)` is true if foo is the number can't be represented by a computer like 0/0.

3. `===` is the strict equality check (both the type and value) while `==` is not strict.

4. `===` between two object references will be true only if they refer to the same object.

5. JavaScript has first-class support for functions.

6. Tow built-in numeric types: `Number` and `BigInt`. Integers are implicitly floats: `3 / 2 = 1.5`.

7. `parseInt()` and `parseFloat()`:

    ```js
    parseInt('123'); // 123
    parseInt('11', 2); // 3
    parseInt('hi'); // NaN
    parseFloat('1.2'); // 1.2
    parseFloat('123.2abc'); // 123.2. Parse until invalid char.
    ```

8. `NaN`:

    ```js
    Number.isNaN(NaN); // true. It's only true when the parameter is truly NaN.
    // The global isNaN gives unintuitive behavior, do not use!
    ```

9. `Infinity`:

    ```js
    1 / 0; // Infinity
    - 1 / 0; // -Infinity
    isFinite(Infinity); // false
    isFinite(NaN); // false
    ```

10. Strings:

    ```js
    'hello'.length; // 5
    'hello'.charAt(1); // 'e'
    '1' + 2 + 3; // '123'
    1 + 2 + 3; // '33'
    '' + 2; // '2'. A useful way to convert to string.
    ```

11. Like Python, JavaScript also has truthy and falsy booleans:
    * false: false, 0, '', NaN, null, undefined.
    * true: all the others.

12. Two other ways of for loops:

    ```js
    for (let object in objects) {
    }
    for (let num of array) {
        // for in only iterates over the array indices.
    }
    ```

13. Arrays are a special type of object:

    ```js
    let array = ['dog', 'cat', 'panda'];
    array.length; // 3
    array[100] = 'fox'; // Create a sparse array.
    array.length; // 101
    typeof array[90]; // undefined.
    array.push('new_item');
    array.forEach(element => console.log(element));
    ```

## Functions

1. The named parameters are more like guidelines:
    * Calling a function without enough parameters gives undefined.
    * Calling a function with more parameters than expected ignores the extra parameters.
    * `arguments` is an array-like object holding all the values passed in. The rest parameter syntax `...args` is preferred for ES6. We can also pass in an array with the spread operator `...numbers`.

        ```js
        function foo(...args) {
            for (let arg of args) {}
        }
        ```

2. Anonymous functions:

    ```js
    let foo = function() {}; // Equivalent to function foo().
    ```

3. Arrow function expressions are compact alternatives to traditional functions, but they can't be used in all situations.

    ```js
    const animals = ['cat', 'dog', 'panda'];
    console.log(animals.map(animal => animal.length));
    // Multiple arguments or no arguments require the parentheses.
    (a, b) => a + b;
    () => console.log('No arguments.');
    // Multiline body requires braces and return.
    (a, b) => {
        let foo = 42;
        return a + b + foo;
    }
    let foo = a => a + 100; // Named function.
    ```

## OOP

1. Objects are like Dictionaries in Python and HashMaps in Java.

2. `let obj = {};` is the preferred way to create an empty object.

3. Example:

    ```javascript
    let foo = {
        name: "bar",
        coding: function() {
            this.name = "coding bar";
            alert.log("I'm coding now.");
        },
        age: 17
    };
    foo.height = 190; // This adds a new property.
    console.log(foo["name"]); // Also works.
    delete foo.age; // This deletes the property.
    for (let prop in foo) { // Print all properties.
        console.log(prop + ": " + foo[prop]);
    }
    ```

4. JavaScript (before ES6) doesn't have classes. For inheritance, we have prototypal inheritance. (Prototype is like the parent class.)

    ```javascript
    function Foo(bar, stuff) {
        this.bar = bar;
        this.stuff = stuff;
    }

    Foo.prototype.foobar = "hello";
    var bob = new Foo("this is", "cool");
    console.log(bob.foobar) // "hello"
    function Foooo(bar, stuff, more) {
        Foo.call(this, bar, stuff); // calling the Foo constructor
        this.more = more;
    }
    Foooo.prototype = new Foo();
    Foooo.prototype.new_property = "blah";
    Foooo.prototype.constructor = Foooo;
    ```

## DOM (Document Object Model)

1. `document.getElementById("foo").innerHTML` gives the content of the html element with the id 'foo'. JavaScript does this by interacting with the DOM.

2. `foo.setAttribute("class", "bar")` sets the attribute and `var text = document.getElementById("bar").getAttribute("alt")` gets the 'alt' attribute.

## Handling events

1. `onclick`:

    ```javascript
    var image = document.getElementById("foo");
    image.onclick = bar();
    function bar() {
        var image = document.getElementById("foo");
        image.src = "new.png"; // We can change the property when we have the element.
    }
    var images = document.getElementsByTagName("img"); // Get a bunch
    for (var i = 0; i < images.length; i++) {
        images[i].onclick = bar();
    }
    ```

## References

* [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

* [Head First JavaScript Programming: A Brain-Friendly Guide, 1st Edition](https://www.amazon.com/Head-First-JavaScript-Programming-Brain-Friendly/dp/144934013X/ref=sr_1_1?crid=2NISC4BUYXL03&keywords=head+first+javascript+programming&qid=1562253717&s=gateway&sprefix=head+first+javascript%2Caps%2C402&sr=8-1)

* <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions>
