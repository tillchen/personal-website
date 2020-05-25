+++
draft = false
date = 2019-07-15
title = "JavaScript Notes"
description = "Some notes for JavaScript"
tags = ["Programming Languages"]
+++

## Basics

1. Variables:
    * Global variables live as long as the page.
    * If we forget to declare a variable before using it, it'll always be global even if we first use it in a function.

2. The syntax for an array is similar to Python, using `[]` instead of `{}`:

    ```javascript
    var foo = [1, 2, 3, 4, 5];
    foo.push(6);
    console.log(foo.length);
    foo[6] = 7; // We can do this, but be sure to avoid creating a sparse array
    ```

3. `undefined` vs `null` vs `isNaN()`:
    * `undefined` is used for;
        * Unassigned/ Uninitiated variables;
        * A missing property for an object;
        * A missing value for an array;
    * `null` is used for uncreated objects (like `.getElementById()`'s returned value;)
    * `isNaN(foo)` is true if foo is the number can't be represented by a computer like 0/0.

4. `===` is the strict equality check (both the type and value) while `==` is not strict.

5. `===` between two object references will be true only if they refer to the same object.

6. A function in JavaScript can behave just like any data type. It can be assigned to, passed, and returned.

## OOP

1. Example:

    ```javascript
    var foo = {
        name: "bar", // A comma here
        coding: function() {
            this.name = "coding bar"; // Must add `this.`
            alert.log("I'm coding now.");
        }, // A comma here
        age: 17 // NO comma here. But can be added after ES5. However, not in JSON.
    };
    foo.height = 190; // This adds a new property
    console.log(foo["name"]); // Also works
    delete foo.age; // This deletes the property
    for (var prop in foo) { // Prints all properties
        console.log(prop + ": " + foo[prop]);
    }
    ```

2. Like in Java, use `this` and `new` for constructors.

3. JavaScript (before ES6) doesn't have classes. For inheritance, we have prototypal inheritance. (Prototype is like the parent class.)

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

* [Head First JavaScript Programming: A Brain-Friendly Guide, 1st Edition](https://www.amazon.com/Head-First-JavaScript-Programming-Brain-Friendly/dp/144934013X/ref=sr_1_1?crid=2NISC4BUYXL03&keywords=head+first+javascript+programming&qid=1562253717&s=gateway&sprefix=head+first+javascript%2Caps%2C402&sr=8-1)
