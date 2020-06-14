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
  * [Runes and Grapheme Clusters](#runes-and-grapheme-clusters)
  * [Symbols](#symbols)
* [Functions](#functions)
  * [Optional Parameters](#optional-parameters)
    * [Named Parameters](#named-parameters)
    * [Positional parameters](#positional-parameters)
  * [Anonymous Functions](#anonymous-functions)
* [Operators](#operators)
* [Exceptions](#exceptions)
* [Classes](#classes)
* [Enum](#enum)
* [Asynchrony](#asynchrony)
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

5. Unlike Java, Dart doesn't have `public`, `protected`, and `private`. Prefix an underscore `_` makes it private to the library. Every Dart app is a library.

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

4. Strings use UTF-16.

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

4. `forEach()` can replace a for loop; `where()` can be used to filter:

    ```dart
    candidates
        .where((c) => c.yearsOfExperience >= 5)
        .forEach((c) => c.interview());
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

### Runes and Grapheme Clusters

1. In Dart, runes expose the Unicode code points of a string.

2. Because a Dart string is a sequence of UTF-16 code units, the usual way to express a code point is `\uXXXX`, where XXXX is a 4-digit hexadecimal value. For more or less than 4 hex digits, place the value in curly brackets.

    ```dart
    import 'package:characters/characters.dart';

    var hi = 'Hi 🇩🇰';
    print('The las character: ${hi.characters.last}');
    ```

### Symbols

1. A `Symbol` object represents an operator or identifier declared in a Dart program. We might never need to use symbols, but they're invaluable for APIs that refer to identifiers by name, because minification changes identifier names but not identifier symbols.

    ```dart
    #radix
    #bar
    ```

## Functions

1. Dart is a true object-oriented language. Even functions are objects and have a type.

2. Although Effective Dart recommends type annotations for public APIs, the function still works if you omit the types.

    ```dart
    bool isNoble(int atomicNumber) {
        return _nobleGases[atomicNumber] != null;
    }
    // Or the below
    isNoble(atomicNumber) {
        return _nobleGases[atomicNumber] != null;
    }
    // Or the arrow syntax
    bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
    ```

3. `..` is a cascade, which enables us to perform multiple operations on the members of a single object:

    ```dart
    querySelector('id')
      ..text = 'Click me!'
      ..onClick.listen(reverseText);
    ```

4. All functions return a value. If no return value is specified, it returns `null`.

### Optional Parameters

1. Optional parameters can be either named or positional, but not both.

#### Named Parameters

1. Example:

    ```dart
    void enableFlags({bool bold, bool hidden}) {...}

    enableFlags(bold: true, hidden: false);
    ```

2. Although named parameters are a kind of optional parameter, we can used `@required` to make it mandatory:

    ```dart
    const Scrollbar({Key key, @required Widget child})
    ```

#### Positional parameters

1. Wrapping a set of parameters in `[]` makes them optional positional:

    ```dart
    String say(String from, String msg, [String device]) {...}
    ```

### Anonymous Functions

1. Example:

    ```dart
    var list = ['apples', 'bananas', 'oranges'];
    list.forEach((item) {
        print('${list.indexOf(item)}: $item');
    });
    // Or the arrow notation
    list.forEach(
        (item) => print('${list.indexOf(item)}: $item'));
    ```

## Operators

1. `~/` is the floor division. The rest are the same as the ones in Java.

2. `as`, `is`, and `is!`:
    * `as`: Typecast. Use it iff we are sure the object is of that type: `(emp as Person).firstName = 'Bob'`
    * `is` True if the object has the type. `is!` False if the object has the type.

3. `b ??= value` assigns if b is null; otherwise b stays the same.

4. `String playerName(String name) => name ?? 'Guest';` gives name `'Guest'` if name is null.

5. `?.` conditional access like in Swift/Kotlin. `foo?.bar` is null if foo is null.

## Exceptions

1. Example:

    ```dart
    throw FormatException('Expected at least 1 section');
    try {
        breadMoreLlamas();
    } on OutOfLlamasException {
        buyMoreLlamas();
    } on Exception catch (e, s) {
        print('Unknown exception: $e');
        print('Stack trace $s');
    } catch (e) {
        // handles all
        print('Something really unknown: $e');
        rethrow; // Allow callers to see the exception.
    } finally {
        // Always executes.
    }
    ```

## Classes

1. All instance variables generate an implicit getter method. Non-final instance variables also generate an implicit setter. Just do `foo.x`.

2. Constructor syntactic sugar:

    ```dart
    class Point {
        double x, y;

        Point(this.x, this.y);
    }
    ```

3. Named constructors for extra clarity:

    ```dart
    Point.origin() {
        x = 0;
        y = 0;
    }
    ```

4. Invoke the super constructor almost like in C++:

    ```dart
    Employee.fromJson(Map data) : super.fromJson(data) {
        ...
    }
    ```

5. Redirecting constructors: `Point.alongXAxis(double x) : this(x, 0)` delegates to the main constructor.

6. Use `factory` for a constructor that doesn't always create a new instance of the class. It may return an instance from a cache or return an instance of a subtype.

7. Getters and setters:

    ```dart
    // We can define explicit getters and setters using get and set

    double get right => left + width;
    set right(double value) => left = value - width
    ```

8. Abstract classes and methods:

    ```dart
    abstract class AbstractContainer {
        void updateChildren(); // Abstract method
    }
    ```

9. Every class implicitly defines an interface containing all instance members of the class and of any interfaces it implements. Like in Java, we have `extends`, `implements`, and `@override`.

10. Liked in C++, operators can be overriden:

    ```dart
    Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
    ```

11. Mixins: a way of reusing a class's code in multiple class hierarchies. Use `with`:

    ```dart
    class Musician extends Performer with Musical {
        ...
    }
    // To implement, declare no constructors
    mixin Musical { // mixin Musical on Musician to restrict the types that can use the mixin
        ...
    }
    ```

## Enum

1. Example:

    ```dart
    enum Color {red, green, blue}

    assert(Color.red.index == 0);
    List<Color> colors = Color.values;
    ```

## Asynchrony

1. To use `await`, code must be in an `async` function:

    ```dart
    Future checkVersion() async { // It returns a Future object
        var version = await lookUpVersion();
    }
    ```

2. Use `await for ()` to handle a Stream (wait for all of the streams results). But we should not use this for UI event listeners, because UI frameworks send endless streams of events.

## References

* [A tour of the Dart language](https://dart.dev/guides/language/language-tour)
