---
title: "Dart Notes"
date: 2020-06-13T22:48:53+02:00
draft: false
description: "Some notes for the Dart programming language."
tags: ["Programming Languages"]
---
* [Introduction](#introduction)
* [Variables](#variables)
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

## References

* [A tour of the Dart language](https://dart.dev/guides/language/language-tour)
