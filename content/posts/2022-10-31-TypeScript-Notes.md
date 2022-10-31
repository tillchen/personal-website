---
title: "TypeScript Notes"
date: 2022-10-31T10:57:10-07:00
draft: false
description: "TypeScript notes"
tags: ["Programming Languages", "TypeScript", "Web Development"]
---

* [Basics](#basics)

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
