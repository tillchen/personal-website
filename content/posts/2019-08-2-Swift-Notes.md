+++
draft = false
date = 2019-08-02
title = "Swift Notes"
description = "Some notes for Swift"
tags = ["Programming Languages"]
+++

* [Basics](#basics)
* [OOP](#oop)
* [References](#references)

## Basics

1. How to compile and run:

    ```sh
    swift hello_world.swift
    ```

2. No semicolons (though allowed) and no main functions. But semicolons must be used to write multiple statements in a single line.

3. Use `let` to make a constant and `var` to make a variable. (Use `let` whenever possible) Plus, multiple variables can be declared in one line: `var x = 0, y = 0`.

4. Types are inferred. If we need to make the type explicit, add the type after: (In practice, we rarely need to use the type annotations.)

    ```swift
    var implicitInt = 70
    var explicitInt: Int = 70
    ```

5. Use `\()` to include values in a string:

    ```swift
    var foo = 17
    print("hello world \(foo)")
    var optionalString: String? = "I exist"
    print(\(optionalString!)) // forced unwrapping, we are sure the value exists. And ! is required
    var assumedString: String! = "I definitely exist"
    print(\(assumedString))
    ```

6. Use `[]` for both arrays and dictionaries. A comma is allowed after the last element.

7. Use `"""` for multiple-line strings:

    ```swift
    let foo = """
    Hello
    World
    """
    ```

8. Arrays, dictionaries and sets:

    ```swift
    var emptyArray = [String]()
    emptyArray.append("first")
    emptyArray += ["second", "third"]
    emptyArray.insert("fourth", at: 3)
    var emptyDictionary = [String: Float]()
    // Or below if the type can be inferred
    var emptyArray = []
    var emptyDictionary = [:]
    var threeDoubles = Array(repeating: 0.0, count: 3)
    var shoppingList: [String] = ["Eggs", "Milk"]
    var shoppingListInferred = ["Eggs", "Milk"]
    var fooSet: Set<Int>
    ```

9. The for loop:

    * `for foo in foos {}`.
    * `for (foo, bar) in foobarDictionary {}`.
    * `for i in 0..<4 {}` is equivalent to `for i in range(0, 4):` in python.
    * `for i in 0...4 {}` is equivalent to `for i in range(0, 5):` in python.
    * `for (index, value) in shoppingList.enumerated()` gives tuples: (0, foo), (1, bar)

10. Optionals: add `?` to indicate that the value might be missing: `var optionalString: String? = "foo"` (it can later be set to `nil`.) (The type is required to init a nil.)

11. Use `??` to provide a default value for an optional:

    ```swift
    let foo: String?
    let bar: String = "Default"
    print(foo ?? bar)
    ```

12. Optional binding

    ```swift
    if let constantName = someOptional { // if the value != nil
        // statements
    }
    ...
    if let theApartmentNumber = person.residence?.address?.apartmentNumber {
        // optional chaining
    }
    ```

13. Functions:

    ```swift
    func foo(bar: String) -> String {
        return "hello \(bar)"
    }

    foo(bar: "Till")
    // A tuple can be used to return multiple values
    // Functions call also be returned (First-class functions)
    // _ bar: String means no argument label when being called
    ```

14. String methods:
    * `.isEmpty` and `.count` (no parentheses) (.count is for length).
    * `.lowercased()`
    * `.hasPrefix()`, `.hasSuffix()`, `contains()`

15. By default, the `switch` in Swift doesn't fall through, which means we don't need `break`. (If needed, `fallthrough` can be added.) Multiple conditions are allowed: `case "a", "e", "i", "o", "u"`. Range values are allowed: `case 0...9`.

16. `@discardableResult func foo() -> String {}` means the return is discardable.

17. `Double(x)` converts the type to Double.

18. `.toggle()` toggles boolean values.

19. The default type for a char is `String`:

    ```swift
    let a = "a" // String
    let b : Character = "b" // Character
    ```

20. `Any` and `as?`:

    ```swift
    var items: [Any] = [5, "Bill", 6.7, Dog()]
    if let firstItem = items[0] as? Int {
        // Any is generic. as? is used for conditional casting
    }
    ```

21. `enum`:

    ```swift
    enum CompassPoint {
        case north, east, south, west
    }
    var compassHeading: CompassPoint = .west
    ```

22. `guard`:

    ```swift
    guard let name = nameField.text else {
        show("No name to submit")
        return
    }
    ```

## OOP

1. Use `init()` `super.init` and `self` to make a constructor (similar to python):

    ```swift
    class Foo: Bar {
        var foobar: String

        init(foobar: String, name: String) {
            super.init(name: name)
            self.foobar = foobar
        }

        override func fooFunc()
    }

    let foo = Foo(foobar: "hello", name: "world")
    ```

2. In a setter, the new value has the implicit name `newValue`.

3. Use `===` and `!==` to see if two references are referring to the same object.

4. A convenience initializer is like a helper, which always calls another initializer in the same class. `convenience init()`

5. `mutating func` changes a property.

6. `willSet` and `didSet` are property observers.

## References

* The Swift Programming Language

* [iOS Programming: The Big Nerd Ranch Guide](https://www.amazon.com/iOS-Programming-Ranch-Guide-Guides/dp/0134682335/ref=sr_1_2?keywords=ios+programming&qid=1564912891&s=gateway&sr=8-2)

* App Development with Swift
