---
title: "Dart Notes"
date: 2020-06-13T22:48:53+02:00
draft: false
description: "Some notes for the Dart programming language."
tags: ["Programming Languages"]
---
* [Introduction](#introduction)
* [Variables](#variables)
* [Built-in Types](#built-in-types)
  * [Numbers](#numbers)
  * [Strings](#strings)
  * [Booleans](#booleans)
  * [Lists](#lists)
  * [Sets](#sets)
  * [Maps](#maps)
* [References](#references)

## Introduction

1. A basic Dart program:

    ```dart
    printInteger(int aNumber) {
        print('The number iis $aNumber');
    }

    main() {
        var number = 42;
        printInteger(number);
    }
    ```

2. Everything that can be placed in a variable is an object. Even numbers, function, and `null` are objects. Are objects inherit from the `Object` class.

3. Dart is strongly typed. But type annotations are optional thanks to type inference. When we want to say explicitly that no type is expected, use the type `dynamic`.

4. Dart supports generic types like `List<int>` or `List<dynamic>` (a list of objects of any type).

5. Unlike Java, Dart doesn't have `public`, `protected`, and `private`. Prefix an underscore `_` makes it private to the library.

## Variables

1. Use `var` for local variables instead of type annotations.

2. The default value for uninitialized variables is `null`.

    ```dart
    int lineCount;
    assert(lineCount == null);
    ```

3. `final` and `const`:
    * A `final` variable can be set only once. Use `final` if we don't know the value at compile time.

        ```dart
        final name = 'Bob'; // Without a type annotation
        final String nickname = 'Bobby'; // Or this
        ```

    * Use `const` for variables that are **compile-time constants**. If it's at the class level, make it `static const`. We can also use `const` for constant *values*. We can change the value of a non-final, non-const variable, even if it used to have a const value.

        ```dart
        var foo = const [];
        final bar = const [];
        const baz = []; // Equivalent to `const []`
        foo = [1, 2, 3]; // Was const []
        ```

## Built-in Types

### Numbers

1. Both `int` and `double` are subtypes of `num`.

2. Conversion between a string and a number:

    ```dart
    int one = int.parse('1');
    double onePointOne = int.parse('1.1');
    String oneAsString = 1.toString();
    String piAsString = 3.14159.toStringAsFixed(2); // 3.14
    ```

### Strings

1. Both single or double quotes are fine. It's easy to escape the delimiter: `'It\'s easy.'`

2. String interpolation: `${expression}`. If the expression is an identifier, we can skip `{}`.

    ```dart
    var s = 'string interpolation';
    print('Dart has $s');
    ```

3. Like in Python, create a multi-line string using triple quote `'''` or `"""`.

### Booleans

1. Only `true` and `false` have the type `bool`, which means we have to check the values explicitly (unlike Python).

    ```dart
    var fullName = '';
    assert(fullName.isEmpty);
    ```

### Lists

1. Example:

    ```dart
    var list = [1, 2, 3];
    assert(list.length = 3);
    var constantList = const [1, 2, 3];
    // constantList[1]  = 1; // Uncommenting this causes an error.
    ```

2. Spread operator (`...`) and null-aware spread operator (`....?`) provide a concise way to insert all elements into a collection:

    ```dart
    var list = [1, 2, 3];
    var list2 = [0, ...list]; // [0, 1, 2, 3]
    var list3;
    var list4 = [0, ...?list3]; // list3 might be null. Avoid exceptions.
    ```

3. Collection if and collection for (similar to list comprehension in Python):

    ```dart
    var nav = [
        'Home',
        'Furniture',
        'Plants',
        if (promoActive) 'Outlet'
    ];
    var listOfInts = [1, 2, 3];
    var listOfStrings = [
        '#0',
        for (var i in listOfInts) `#$i`
    ];
    ```

### Sets

1. Example:

    ```dart
    var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
    var names = <String>{}; // Empty set
    // Set<String> names = {}; // Works too
    // var names = {}; // Creates a map, not a set.
    var elements = <String>{};
    elements.add('flourine');
    elements.addAll(halogens);
    ```

### Maps

1. Example:

    ```dart
    var nobleGases = {
        2: 'helium',
        10: 'neon',
        18: 'argon'
    };
    // Or we can use the constructor
    var nobleGases = Map(); // The new keyword is optional
    nobleGases[2] = 'helium';
    nobleGases[10] = 'neon';
    nobleGases[18] = 'argon';
    ```

2. The map returns a `null` if the key doesn't exist.

## References

* [A tour of the Dart language](https://dart.dev/guides/language/language-tour)
