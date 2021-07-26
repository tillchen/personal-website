+++
draft = false
date = 2019-07-15
title = "JavaScript Notes"
description = "Some notes for JavaScript"
tags = ["Programming Languages"]
+++

* [Basics](#basics)
* [Numbers](#numbers)
* [Functions](#functions)
* [OOP](#oop)
* [DOM (Document Object Model)](#dom-document-object-model)
* [Handling events](#handling-events)
* [Pitfalls](#pitfalls)
* [JSON](#json)
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
        let a = 1, b = 2; // Possible to declare multiple variables in one line.
        ```

2. `undefined` vs `null` vs `isNaN()`:
    * `undefined` is used for:
        * Unassigned/ Uninitiated variables;
        * A missing property for an object;
        * A missing value for an array;
    * `null` is used for uncreated objects (like `.getElementById()`'s returned value;)
    * `NaN` is a number that can't be represented (0/0).

3. `===` is the strict equality check (both the type and value) while `==` is not strict.

4. `===` between two object references will be true only if they refer to the same object.

5. Strings:

    ```js
    // There's no character type in JS.
    'hello'.length; // 5
    'hello'.charAt(1); // 'e'
    'hello'.toUpperCase(); // 'HELLO'
    '1' + 2 + 3; // '123'
    1 + 2 + 3; // '33'
    '' + 2; // '2'. A useful way to convert to string.
    'A' === '\u0041'; // \ is the escape character.
    const foo = 42;
    `The answer is ${foo}`; // Back ticks give us template literals. They also allow multiline strings.
    // JS uses UTF-16, which is not enough for all the symbols. So a single 16-bit code unit is used for common characters but two code units are used for rare ones.
    ```

6. Like Python, JavaScript also has truthy and falsy booleans:
    * false: false, 0, '', NaN, null, undefined.
    * true: all the others.

7. Two other ways of for loops:

    ```js
    for (let object in objects) {
    }
    for (let num of array) {
        // for in only iterates over the array indices.
    }
    ```

8. Arrays are a special type of object:

    ```js
    let array = ['dog', 'cat', 'panda'];
    array.length; // 3
    array[100] = 'fox'; // Create a sparse array.
    array.length; // 101.
    typeof array[90]; // undefined.
    array.push('new_item');
    let new_item = array.pop(); // new_item.
    array.forEach(element => console.log(element));
    delete array[1]; // Because arrays are really objects.
    typeof array; // object.
    let foo = array.concat([1, 2, 3]); // ['dog', 'cat', 'panda', 1, 2, 3].
    let bar = array.join('#');
    array.reverse();
    array.sort(function (a, b) {
        return a - b;
    });
    array.slice(1, 3); // Like in Python.
    let squared = array.map(a => a * a);
    let filtered = array.filter(a => a.length > 3);
    ```

9. `/**/` can also occur in regular expression literals, so it's recommended to use `//` instead.

10. `regexp.test(string)` returns true if the string matches.

11. Destructuring (similar to Python and Kotlin):

    ```js
    const x = [1, 2];
    const [y, z] = x; // y = 1, z = 2.
    let a = 1, b = 2;
    [a, b] = [b, a]; // Swap like in Python.
    ```

## Numbers

1. Two built-in numeric types: `Number` and `BigInt`. Integers are implicitly floats (64-bit like `Double`): `3 / 2 = 1.5`. This is good as short integer overflows are avoided.

2. `parseInt()` and `parseFloat()`:

    ```js
    parseInt('123'); // 123
    parseInt('11', 2); // 3
    parseInt('hi'); // NaN
    parseFloat('1.2'); // 1.2
    parseFloat('123.2abc'); // 123.2. Parse until invalid char.
    ```

3. `NaN`:

    ```js
    Number.isNaN(NaN); // true. It's only true when the parameter is truly NaN.
    // The global isNaN gives unintuitive behavior, do not use!
    ```

4. `Infinity`:

    ```js
    1 / 0; // Infinity
    - 1 / 0; // -Infinity
    isFinite(Infinity); // false
    isFinite(NaN); // false
    ```

5. `1e2` is the same as 100.

6. `Math.floor()` converts a number to an integer.

## Functions

1. JavaScript has first-class support for functions. Functions are linked to `Function.prototype`, which is self is linked to `Object.prototype`.

2. The named parameters are more like guidelines:
    * Calling a function without enough parameters gives undefined.
    * Calling a function with more parameters than expected ignores the extra parameters.
    * `arguments` is an array-like object holding all the values passed in. The rest parameter syntax `...args` is preferred for ES6. We can also pass in an array with the spread operator `...numbers`.

        ```js
        function foo(...args) {
            for (let arg of args) {}
        }
        ```

3. Anonymous functions:

    ```js
    const foo = function() {}; // Equivalent to function foo().
    // This is called a function expression, which is not hoisted (unlike function declaration).
    ```

4. Arrow function expressions are compact alternatives to traditional functions, but they can't be used in all situations.

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

5. JS has by default function scopes instead of block scopes, which means a variable defined anywhere in a function is visible everywhere in the function. This also causes it to be better to declare all the variables in a function at the top instead of as late as possible. (ONLY applicable to `var` instead of `let`.)

6. `function.apply(thisArg, argArray)` sets `this` with the first parameter.

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
    console.log(foo["name"]); // Also works,though the dot notation is preferred.
    delete foo.age; // This deletes the property.
    for (let prop in foo) { // Print all properties.
        console.log(prop + ": " + foo[prop]);
    }
    let bar = foo.bar || "default"; // || gives the default value,
    foo.bar.foobar; // Throw TypeError, as foo.bar is undefined.
    foo.bar && foo.bar.foobar; // Prevent TypeError.
    ```

4. We use functions to define custom objects and prototypes to add methods. A prototype is an object shared by all instances of that class. This is similar to extension functions in Kotlin and Swift. We can do the same for built-in objects (augmenting types).

    ```js
    function Person(first, last) {
        this.first = first;
        this.last = last;
    }
    Person.prototype.fullName = function() {
        return this.first + ' ' + this.last;
    };
    let person = new Person('foo', 'bar');
    ```

5. Delegation is used for prototype chaining. A retrieval looks up the property in the object, then the prototype, then the prototype's prototype, and eventually `Object.prototype`.

6. `typeof` looks up the prototype chain. We can use `hasOwnProperty()` to check if the object has a property without looking up the chain: `flight.hasOwnProperty('constructor')`.

7. Exceptions:

    ```js
    throw {
        name: 'TypeError',
        message: 'foo'
    }
    try {
    } catch (e) {
    }
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

## Pitfalls

1. `typeof null` gives `object`. A better null test is simply `foo === null`. `typeof []` also gives `object`. `typeof NaN` gives `number`.

2. `parseInt('16 tons')` returns 16 without warnings.

3. `NaN === NaN` returns false.

4. Always use `===` and `!==` to avoid surprises.

5. Bitwise operations in JS are very slow and far from the hardware, as JS doesn't have integers and have to to conversions.

6. JS declarations are hoisted. This means a variable with `var` can be used before the declaration. Hoisting moves all declarations to the top of the scope.

7. If we forget to use `new`, `this` is bound to the global object and the function will clobber  global variables.

8. Avoid `void`.

## JSON

1. JSON strings use double quotes. All property names are strings. No functions or comments allowed.

2. `JSON.parse()` converts JSON into a JS object. `JSON.stringify()` converts a JS object to a JSON string.

## References

* [A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

* [Head First JavaScript Programming: A Brain-Friendly Guide, 1st Edition](https://www.amazon.com/Head-First-JavaScript-Programming-Brain-Friendly/dp/144934013X/ref=sr_1_1?crid=2NISC4BUYXL03&keywords=head+first+javascript+programming&qid=1562253717&s=gateway&sprefix=head+first+javascript%2Caps%2C402&sr=8-1)

* <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions>

* [JavaScript: The Good Parts by Douglas Crockford](https://www.goodreads.com/book/show/2998152-javascript?from_search=true&from_srp=true&qid=a2NTYmkfEm&rank=1)
