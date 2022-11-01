---
title: "TypeScript Notes"
date: 2022-10-31T10:57:10-07:00
draft: false
description: "TypeScript notes"
tags: ["Programming Languages", "TypeScript", "Web Development"]
---

* [Basics](#basics)
* [ES6](#es6)
* [OOP](#oop)
* [Debugging](#debugging)

## Basics

1. `tsc -init` generates `tsconfig.json` and now `tsc` will compile all the files in the directory and subdirectories.

2. Arrays.

    ```ts
    const pets: string[] = ['cats', 'dogs'] // can only hold strings now.
    const weirdArray: any[] = ['cats', 42] // can hold any type.
    ```

3. Tuples are just arrays with a specific number of elements of specific types. There are no enforcements in JS, but it's all enforced in TS.

    ```ts
    const author: [string, number] = ['Bob', 42];
    ```

4. Enums are TS only.

    ```ts
    enum Food {
        Pizza, // 0
        Burger = 500,
        Rice // 501
    }
    const myFavorite = Food.Burger
    console.log(myFavorite) // 500
    ```

5. Functions.

    ```ts
    function add(x: number, y: number): string {
        return '' + x + y
    }
    let f: (x: number, y: number) => string = add
    ```

6. Objects.

    ```ts
    const car: {type: string, model: string, year: number} = {
        type: 'Toyota',
        model: 'Corolla',
        year: 2009
    }
    ```

7. `null` and `undefined` are subtypes of all other types. So we can do `let foo: number = null`. We can turn on `strictNullChecks` in `tsconfig.json`,and then the compiler will complain if we assign null or undefined to any variable except if it is declared as type any. If we do `let foo = null`, we can only assign null to it due to the inferred null type.

8. `void` is typically used only when the function returns no value.

9. Type alias.

    ```ts
    type Person = {
        firstName: string, lastName: string, age: number
    }
    const person1: Person = {
        firstName: 'Mike', lastName: 'White', age: 42
    }
    ```

10. Union types.

    ```ts
    let foo: number | string;
    foo = 42
    foo = '42'
    ```

11. Use third-party libraries with the `@types` type declaration file.

    ```ts
    npm install foo
    npm install --save-dev @types/foo
    ```

## ES6

1. We get ES6 features out-of-the-box when using TypeScript.

2. Arrow functions and default parameters.

    ```ts
    const addNums = (a: number, b: number = 10): number => a + b
    ```

3. Spread, rest, and optional arguments.

    ```ts
    const addNums = (a?: number, b?: number): number => a + b // optional arguments
    const nums: number[] = [5, 6]
    addNums(...nums) // spread
    ```

    ```ts
    const addNums = (...a: number[]) : number => a.reduce((acc, val) => acc + val) // rest
    const nums: number[] = [5, 6]
    addNums(...nums)
    ```

4. Destructuring (objects and arrays).

    ```ts
    const person = {
        firstName: 'Mike', lastName: 'White', age: 42
    }
    const {firstName, lastName, age} = person
    const a = ['abc', 11, 'wow']
    const [x, y, z] = a
    [foo, bar] = [bar, foo] // swaps the values
    ```

## OOP

1. We can declare properties outside of constructors. And TS adds visibilities (private, protected).

    ```ts
    class Planet {
        private name: string = 'none'
        mass: number;
        static meStatic: boolean = true
        constructor(inName: string, inMass: number) {
            // We can still set the properties in constructors
            this.name = inName
            this.mass = inMass
        }
    }
    ```

2. Getters and setters.

    ```ts
    class Planet {
        private _name: string = 'no name set'
        get name() {
            return `Name is ${self._name}`
        }
        set name(inName: string) {
            this._name = inName
        }
        readonly foo: string = 'read only' // or provide a getter only to make it read-only
    }
    ```

## Debugging

1. `tsc --sourceMap foo.ts` generates a `foot.js.map` file, which allows us to debug the TS code in the browser by appending `//# sourceMappingURL=foo.js.map` to the TS file.
