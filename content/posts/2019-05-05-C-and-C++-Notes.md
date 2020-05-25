+++
draft = false
date = 2019-05-05
title = "C and C++ Notes"
description = "Some notes for C and C++"
tags = ["Programming Languages"]
+++

* [Basics](#basics)
* [Dynamic Memory Allocation](#dynamic-memory-allocation)
* [Makefiles](#makefiles)
* [Templates](#templates)
* [STL](#stl)
* [Miscellaneous](#miscellaneous)

## Basics

1. How to compile:

    ```sh
    gcc -Wall -o foo foo.c
    ```

2. Input in C:

    ```c
    char foo[100];
    int bar;
    fgets(foo, sizeof(foo), stdin);
    scanf("%d", &bar);
    getchar(); // must be added after scanf. It handels /n
    ```

3. Input in C++:

    ```c++
    string foo;
    getline(cin, foo);
    ```

4. When two types sum, a promotion happens. When a type is passed to a function, a demotion happens.

5. Prefix and postfix `++` `--` differ when used in an assignment:

    ```c
    int a = 1;
    int b = ++a; // a = 2, b = 2
    int b = a++; // a = 3, b = 2
    ```

6. 0 is false. Everything else is true.

7. In `argv`, the first name is the program's name.

8. `puts()` will append `\n` to the end of the string and print.

## Dynamic Memory Allocation

1. In C:

    ```c
    int* foo;
    foo = (int*) malloc(sizeof(int) * 10);
    if (foo == NULL) {
        // blah blah
    }
    free(foo);
    ```

2. In C++: `T* ptr = new T(value)` is equivalent to `T* ptr = new T[1] {value}` ((value) and {value} can be omitted.) (`delete [] ptr` for arrays; `delete ptr` for single values.)

## Makefiles

1. How to run:

    ```sh
    make // if no extension
    ```

    ```sh
    make -f Mymakefile.txt
    make
    ```

## Templates

1. Generic Functions:

    ```c++
    template <class T>
    T array_sum(T arr[], int size) {
        int i;
        T sum = arr[0];
        for (i = 1; i < size; i ++) {
            sum = sum + arr[i];
        }
        return sum;
    }
    ```

2. Generic Classes:

    ```c++
    template <class T>
    class BoundedArray {
        T array[size];
    public:
        BoundedArray(){};
        T& operator[](int); // overloaded access operator
    };

    template<class T>
    T& BoundedArray<T>::operator[](int pos) {
        if ((pos < 0) || (pos >= size)) {
            exit(1);
        }
        return array[pos];
    }
    // ...
    BoundedArray<int> intArray;
    ```

## STL

1. Vectors:

    ```c++
    #include <vector>
    // ...
    vector<int> vInt(5);
    vInt.push_back(1); // .clear, .size, .pop_back
    ```

2. Deques: (Double Ended Queues)

    ```c++
    #include <deque>
    // .push_front, .pop_front
    ```

3. Lists: (Doubly Linked Lists) (No random access.)

4. Iterators:

    ```c++
    vector<int> vInt;
    vector<int>::iterator vIterator;
    for (vIterator = vInt.begin(); vIterator != vInt.end(); vIterator++) {}
    ```

5. Set: (insert, erase, clear, empty, size, find, count)

6. Maps: (find, clear, erase, insert)

    ```c++
    map<const char*, int, LessThanStr> months;
    months["january"] = 31;
    ```

## Miscellaneous

1. When we initialize an int vector, int array, etc., they are filled with 0s.

2. The public interface is inherited to the inherited class. (The interface of the base class is a subset of the derived class.)

3. We must initialize static data members outside of the class:

    ```c++
    class foo {
        static int var;
    }

    int foo::var = 0;
    ```

4. When we write to a file, the data is first stored in the buffer. When the buffer is flushed, the data is written into the file.

5. `const` with pointers

    ```c
    int a = 2;
    const int* ptr = &a; // constant data (2), not constant pointer
    int* const ptr_1 = &a; // constant pointer, not constant data
    const int* const ptr_2 = &a; // constant pointer and data
    ```

6. Auto-dereferencing in C++:

    ```c++
    int foo = 1;
    int& bar = foo;
    bar = 2; // Both are 2 now
    ```
