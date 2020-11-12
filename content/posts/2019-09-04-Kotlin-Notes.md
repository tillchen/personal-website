+++
draft = false
date = 2019-09-04
title = "Kotlin Notes"
description = "Some notes for Kotlin"
tags = ["Programming Languages"]
+++

## Table of Contents

* [Table of Contents](#table-of-contents)
* [Basics](#basics)
* [OOP](#oop)
* [Nulls and Exceptions](#nulls-and-exceptions)
* [Collections](#collections)
* [Lambdas and Higher-Order Functions](#lambdas-and-higher-order-functions)
* [References](#references)

## Basics

1. Kotlin requires one `main` per app:

    ```kotlin
    fun main(args: Array<String>) { // the args part can be omitted
        println("Hello Kotlin!")
    }
    ```

2. Shorter `if`

    ```kotlin
    println(if (x > y) "x is greater" else "x is not greater")
    return if (x > y) x else y
    ```

3. `var` vs `val`:

    * When using `var`, we can assign another value to the variable.
    * When using `val`, the reference to the object stays forever. However, if the variable is an array, the array itself can be updated. (Similar to `let` in Swift)

4. Primitive types are also objects controlled by references.

5. Similar to Swift, we can specify the type:

    ```kotlin
    var z: Int = 6
    var x: Long = z.toLong() // similarly, toFloat(), to Byte(), etc.
    ```

6. Arrays:

    ```kotlin
    var myArray = arrayOf(1, 2, 3)
    var myLength = myArray.size
    var explicitArray: Array<Int> = arrayOf(1, 2, 3)
    ```

7. Strings:

    ```kotlin
    var x = 42
    var myString = "x is $x"
    // When accessing a property or function of a object, use ${}
    var myArray = arrayOf(1, 2, 3)
    var arraySize = "The size if ${myArray.size}"
    var firstItem = "The first item is ${myArray[0]}"
    "12.345-6.A".split(".", "-")  // splits at both . and -
    // We don't need to escape for triple quote strings for regular expressions.
    val regex = """(.+)/(.+)\.(.+)""".toRegex()
    // We can also use triple quotes for multiline strings like in Python.
    ```

8. Functions;

    ```kotlin
    // We can have default values like in Python.
    fun foo(bar: Int = 1): Int { // Unit means no return value, or just omit it
        // stuff
        return 1
    }

    var result = foo(1)

    fun max(a: Int, b: Int): Int = if (a > b) a else b // also works

    fun listOf<T> (vararg values: T): List<T> {...}  // vararg makes it variadic.
    // And we need to explicitly unpack the array when passing:
    listOf("args:", *args)
    ```

9. Loops:

    ```kotlin
    for (x in 1..100) println(x) // end inclusive
    for (x in 1 until 100) println(x) // not end inclusive
    for (x in 15 downTo 1) println(x) // end inclusive
    for (x in 1..100 step 2) println(x)
    for (item in items) println(item)
    ```

10. Input:

    ```kotlin
    val userInput = readLine()
    ```

11. `when`:

    ```kotlin
    when (x) {
        0 -> println("It's 0")
        1, 2 -> println("It's 1 or 2")
        else -> {
            println("It's not 0.")
            println("It's not 1 nor 2.")
        }
    }
    // We can also use when without the argument, then each case needs to be a Boolean.
    ```

## OOP

1. Example class:

    ```kotlin
    class Dog(val name: String, var weight: int, val breed: String = "default") {
        var temperament: String = ""
        // All properties must be initialized.
        // Or:
        // lateinit var temperament = String
        fun bark() {
            println("Woof!")
        }
    }
    ```

2. Custom getters and setters:

    ```kotlin
    ...
    val weightInKgs: Double
        get() = weight / 2.2
    var weight = weightParam
        set(value) {
            if (value > 0) field = value // field is a keyword
        }
    ...
    ```

3. In fact, the complier adds getters and setters automatically:

    ```kotlin
    var myProperty: String
        get() = field
        set(value) {
            field = value
        }
    ```

4. Classes, variables, and methods are final by default. To enable inheritance and overriding, add `open` before them. We also need to add `override` to variables.

5. The `init {}` blocks are called during initialization.

6. If a property is defined using `val` in the superclass, we must override it in the subclass if we want to assign a new value to it. However, if it's defined using `var`, we just need to reassign it in the `init {}` block without using `override`.

7. `abstract` for classes, variables, and methods. These properties must all be overridden later.

8. An `interface` lets us define common behavior OUTSIDE a superclass hierarchy. (Not IS-A, but share a property.) A class can have multiple interfaces, but can only inherit from a single direct superclass. `class X: A, B {}` (No parentheses. `class X: A()` means we are inheriting.)

9. Use `is` to check the type: `if (animal is Wolf)`. And use `as` for explicit casting.

10. `Any` is the superclass of everything.

11. Add `data` to the start of the class to make it behave like a `struct` in Swift. Then we can use `==` or `.equals` to test the equivalence. (Equal objects have the same `.hashCode()` value.) (And `.toString()` returns the value of each property.) (BTW, `===` is used for referential check (identity).)

12. For `data` objects, we have destructuring declaration:

    ```kotlin
    val (title, number) = r
    // is equivalent to
    val title = r.component1
    val number = r.component2
    // And this can be used to return multiple values from a function, or we can use Pair.
    ```

13. Named arguments (`var r = Recipe(title = "title", foo = "bar")`) are also available like in Swift.

14. Secondary constructors:

    ```kotlin
    constructor(foo: Boolean) : this(0, foo) {} // Calls the primary constructor.
    ```

15. `enum` class:

    ```kotlin
    enum class Color {
        RED, YELLOW, BLUE
    }

    fun foo(color: Color) =  // Returns the value directly.
        when (color) {
            Color.RED -> 1
            Color.YELLOW -> 2
            Color.BLUE -> 3
        }
    ```

16. Everything is `public` by default. We also have `private`, `protected`, and `internal` (for a module).

## Nulls and Exceptions

1. Use `?` for nullable objects just like in Swift.

2. `w?.eat()` is a safe call. It's only called when w is not null.

3. Like optional binding in Swift, use `let`:

    ```kotlin
    w?.let {
        // executed when w is not null
    }
    ```

4. The Elvis operator `?:`

    ```kotlin
    w?.hunger ?: -1 // if null, return -1
    ```

5. `!!` is like `!` in Swift. It throws a NullPointerException if the value is null: `w!!.hunger`.

6. Like Java, the same `try catch finally` block for exception handling. And the same `throw`. But we ca use `try` as an expression, which returns the value to the variable:

    ```kotlin
    val number = try {
        Integer.parseInt(reader.readLine())
    } catch (e: NumberFormatException) {
        return  // Or null if we want the flow to continue.
    }
    println(number)
    ```

## Collections

1. `List`, `Set`, `Map`, `MutableList`, `MutableSet`, `MutableMap`.

2. `listOf()`, `mutableListOf()`. `mList.set(1, "foo")`, `.shuffle()`, `.last()`, `.max()`.

3. `mapOf(0 to 'a', 1 to 'b', 2 to 'c')` `for ((key, value) in mMap)`. (`to` actually creates a `Pair<K, V>`).

4. Add `out` (`<out E>`) to make the generics covariant (use a subtype instead of a supertype) - achieving polymorphism. Add `in` to make it contravariant - the opposite of covariance.

5. We can use `in` to check existence just like in Python.

6. Like `enumerate()` in Python, we have `.withIndex()`:

    ```kotlin
    for ((index, element) in collection.withIndex()) {...}
    ```

## Lambdas and Higher-Order Functions

1. We can assign a lambda to a variable:

    ```kotlin
    val addFive: (Int) -> Int = {x: Int -> x + 5}
    val result = addFive.invoke(1) // 6
    // or
    val result = addFive(1)
    ```

2. `{it + 5}` is equivalent to `{x -> x + 5}`.

3. Lambdas can also be passed to functions.

    ```kotlin
    fun convert(x: Double, converter: (Double) -> Double) : Double {
        return converter(x)
    }
    println(convert(1, {c: Double -> c * 1.8 + 32}))
    ```

4. `.min()` and `.max()` work with basic types: `val mMax = mList.max()`

5. `minBy {}` and `maxBy {}` work with all types: `val mMaxQuantity = groceries.maxBy {it.quantity}`. Also `sumBy {}` and `sumByDouble {}` : `mMap.values.sumBy {it}`. We can also use `::` reference like in Java 8 (`::foo` calls the top-level function `foo`).

6. `filter {}`: `val pricesOver3 = groceries.filter {it.price > 3.0}`. Also `filterNot {}`. We can also use `.count()` instead of `.filter().size`.

7. `map {}`: `val doubleInts = ints.map {it * 2}`

8. `forEach {}`: `groceries.forEach {println(it.name)}`

9. Closure means that a lambda can access any local variable that it captures.

10. `groupBy {}` returns a `Map`.

11. `fold {}`: `val sumOfInts = ints.fold(0) {mSum, item -> mSum + item}`

12. `all` and `any`:

    ```kotlin
    listOf(Person("Alice", 27), Person("Bob", 31)).all {it.age < 28}
    ```

## References

* [Head First Kotlin: A Brain-Friendly Guide](https://www.amazon.com/Head-First-Kotlin-Brain-Friendly-Guide/dp/1491996692/ref=sr_1_1?keywords=head+first+kotlin&qid=1567580813&s=gateway&sr=8-1)
