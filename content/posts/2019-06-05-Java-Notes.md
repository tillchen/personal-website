+++
draft = false
date = 2019-06-05
title = "Java Notes"
description = "Some notes for Java"
tags = ["Programming Languages", "Java"]
+++

* [Basics](#basics)
* [Java Data Structures](#java-data-structures)
* [References and Garbage Collection](#references-and-garbage-collection)
* [Static](#static)
* [Exceptions](#exceptions)
* [I/O](#io)
* [Constructors](#constructors)
* [Polymorphism](#polymorphism)
* [References](#references)

## Basics

1. SourceCode.java -> Compile (`javac source_code.java`) -> Java bytecode (SourceCode.class) ->Run by the Java Virtual Machine (`java SouceCode`)
    > Write once, run anywhere!

2. Every Java app must have at least **one class** and at least **one main** method (one main per app not per class.)

    ```java
    public static void main (String[] args) {
        // code
    }
    ```

3. `int rand = (int) (Math.random() * 10)` to get random integers in range [0, 9]. (Math is a Java class.)

4. To use `float`, we need to append `f` to the value. This is because Java treats everything with a floating point as `double`: `float f = 3.14f`

5. Instance variables always get a default value, even if we don't initialize it: `int 0`; `float 0.0`; `boolean false`; `reference null`.

6. Use `==` for primitive types and references; `.equals()` for different objects (for example string objects). (`==` checks the reference/address and `.equals` checks the content.)

    ```java
    public class Test {
        public static void main(String[] args) {
        String s1 = new String("HELLO");
        String s2 = new String("HELLO");
        System.out.println(s1 == s2); // false
        System.out.println(s1.equals(s2)); // true
        }
    }
    ```

7. `Integer.parseInt("42")` converts a string to an int using the Java Integer class.

8. The Stack and the Heap:
    * The Stack: all method invocations and local variables (aka, stack variables) live here.
        * The method on the top of the stack is always the currently executing method. When it's removed from the stack, the method is executed, which the executing sequence for constructor chaining.
    * The Heap: **all** objects live here. Also know as the garbage-collectible heap.
        * Instance variables live inside the objects.
    * If the local variable is a reference to an object, the variable (the reference) goes on the stack, and the object is still on the heap.

9. Use `String.valueOf()` to convert nearly anything (including a char array) to String.

10. C `printf()` like formatting:

    ```java
    String s = String.format(%d, 1);
    System.out.println(s);
    ```

11. `.length` for primitive types (e.g. int[], String[]), `.length()` for objects (e.g. String). And `.size()` for collections.

12. `Integer.MAX_VALUE` and `Integer.MIN_VALUE`.

13. `Character.toLowerCase()` and `Character.isLetterOrDigit()`.

14. Convert a char array to string `String.valueOf(charArray)`;

## Java Data Structures

1. ArrayList: (inside the java.util (utility) class) (for primitive types, Java 5 and above enabled autoboxing: `ArrayList<Integer>`)

    ```java
    import java.util.*
    class Foo {
        public static void main(String[] args) {
            List<String> A = new ArrayList<>();
            A.add("hello");
            A.add("world");
            System.out.println("Size is: " + A.size());
            System.out.println("Contains hello? " + A.contains("hello"));
            System.out.println("Index of hello? " + A.indexOf("hello"));
            A.remove("hello"); // by value
            A.remove(0); // by index
            System.out.println("Empty? " + A.isEmpty());
            List<String> myStringList = Arrays.asList("apple", "banana", "cherry");
            String result = String.join(", ", myStringList); // double quotes
            A.get(0);
            A.set(0, "new one");
            A.addAll(B); // concatenate all from B
        }
    }
    ```

2. `Arrays.asList(foo)` converts arrays to lists and `.toArray(new Integer[0])` converts lists to arrays (new int[0]) is the type hint. Can't convert to a primitive array directly.

3. `Comparator`:

    ```java
    Arrays.sort(intervals, new Comparator<int[]>() {
        @Override
        public int compare(int[] interval1, int[] interval2) {
            return interval1[0] - interval2[0]; // negative, smaller; 0, equal; positive, bigger
        }
    });
    // Java 8:
    Arrays.sort(intervals, (interval1, interval2) -> (interval1[0] - interval2[0]));
    ```

4. When using an interface, we usually use implementations like: `List<T> = new ArrayList<T>()`, `Queue<T> = new LinkedList<T>()`

5. Array:
   * `int[] array = new int[] {1,2,3};` (Can NOT specify the dimension here.)

   * Arrays static methods: `binarySearch(A, 42)`, `copyOf(A)`, `sort(A)`.

   * `Arrays.sort(intervals, (x, y) -> Integer.compare(x[0], y[0]));`

   * 2D array `int[][] grid = new int[10][10]`;

   * Array of lists:

        ```java
        List<Integer>[] foo = new List[10];
        for (int i = 0; i < 10; i++) {
            foo[i] = new ArrayList<>();
        };
        ```

   * `Arrays.fill(arr, 1)` fills the array with 1s.

6. String:
   * Methods: `charAt(1)`, `indexOf('A')`, `replace('a', 'A')`, `replace("a", "abc")`, `substring(1,4)`, `toCharArray()`, `toLowerCase()`, `String[] words = s.split(" ")`

   * `if (string == null || string.isEmpty())`

   * StringBuilder: `String reversed = new StringBuilder(s).reverse().toString()`, `.trim()` (removes leading and trailing spaces). `append()` and `deleteCharAt()`.

7. Map:
    * Initialize a map:

        ```java
        Map<Character, Integer> map = new HashMap<>() {{
            put('I', 1);
            put('V', 5);
            put('X', 10);
        }};
        ```

    * `for (int i : map.keySet());` or

        ```java
        for (Map.Entry<String, Object> entry : map.entrySet()) {
            String key = entry.getKey();
            Object value = entry.getValue();
            // ...
        }
        ```

    * `map.put(i, map.get(i)+1);`

    * `map.containsKey();`

    * `map.values()`;

    * A `TreeMap` is a binary search tree (`SortedMap`) implementation. It's naturally sorted by keys. `SortedMap<Integer, String> sm = new TreeMap<Integer, String>();`

8. Stack:

    * Initialization: `Stack<Integer> stack = new Stack<>();`
    * Common Methods:
        * push(element): Adds an element to the top.
        * pop(): Removes and returns the top element.
        * peek(): Returns the top element without removal.
        * isEmpty(): Checks if the stack is empty.

9. Queue (Interface in Java, often realized using LinkedList):

    * Initialization: `Queue<Integer> queue = new LinkedList<>();`
    * Common Methods:
        * offer(element): Adds an element to the end (returns true if success, false if not). Or we can use add(), but it throws an exception when it fails to add.
        * poll(): Removes and returns the front element.
        * peek(): Returns the front element without removal.

10. PriorityQueue (Binary min heap):

    * Initialization: `PriorityQueue<Integer> pq = new PriorityQueue<>();`
    * Common Methods:
        * offer(element): Adds an element. (Can also use add()).
        * poll(): Removes and returns the smallest element.
        * peek(): Returns the smallest element without removal
    * Max heap: `PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder());`

11. Ternary operator: `x = y == z ? y : z`;

12. ceiling is `Math.ceil((double) x / y)`;

## References and Garbage Collection

1. Garbage collection:

    ```java
    Book b = new Book();
    Book c = new Book();
    // By far, 2 references and 2 objects.
    Book d = c; // d and c refer to the same object
    // 3 references and 2 objects.
    c = b; // c refers to b's object now
    // 3 references and 2 objects.
    ```

    ```java
    Book b = new Book();
    Book c = new Book();
    b = c; // b and c refer to the same object
    // 2 references, 1 reachable object, 1 abandoned object.
    // b's original object eligible for garbage collection
    c = null; //null reference
    // 1 active reference, 1 reachable object
    ```

2. Array of objects or collections:

    ```java
    Book[] books;
    books = new Book[10];
    // By far, only an array of references, no objects created.
    // To create the objects:
    for (int i = 0; i < 10; i++) {
        books[i] = new Book();
    }
    Set<Integer>[] sets = new HashSet[10];
    for (int i = 0; i < 10; i++>) {
        sets[i] = new HashSet<>();
    }
    ```

## Static

1. All static variables are initialized before any object can be created.

    ```java
    public class foo {
        static int count = 0;
        public increment() {
            count++;
        }
    }
    ```

2. Static final variables are constants: `public static final double PI = 3.141592653589793;` (In uppercase by convention.) Or we can use the static initializer which runs before any code in the class:

    ```java
    public class foo {
        static {
            // code``
        }
    }
    ```

## Exceptions

1. The method that throws must declare `throws Exception`:

    ```java
    public class Solution {
        public static void main(String args[] ) throws Exception {
            throw new IllegalArgumentException("Invalid Input");
        }
    }
    ```

2. The `finally` block **always runs** no matter what:

    ```java
    try {
        // code
    } catch (//some exception) {
        // code
    } finally {
        // code
    }
    ```

## I/O

1. Example:

    ```java
    import java.util.Scanner;

    class MyClass {
        public static void main(String[] args) {
            Scanner scanner = new Scanner(System.in);

            System.out.println("Enter name, age and salary:");

            // String input
            String name = scanner.nextLine();

            // Numerical input
            int age = scanner.nextInt();
            double salary = scanner.nextDouble();

            // Output input by user
            System.out.println("Name: " + name);
            System.out.println("Age: " + age);
            System.out.println("Salary: " + salary);
        }
    }
    ```

## Constructors

1. Invoking an overloaded constructor: call `this();`. However, we can only call `this()` **OR** `super()`, **never both** inside a constructor.

    ```java
    public class Bar extends Foo {
        String name;
        public Bar() {
            this("Hello"); // MUST BE THE FIRST STATEMENT!!!
            // code
        }
        public Bar(name) {
            // code
        }
    }
    ```

## Polymorphism

1. With polymorphism, the reference type can be a superclass of the object type.

    ```java
    Animal[] animals = new Animal[2];
    animals[0] = new Dog();
    animals[1] = new Cat();
    for (Animal animal : animals) {
        animal.make_sound(); // mew and bark
    }
    ```

2. We can also have polymorphic arguments and return types. (When a superclass type is requested, a subclass is sufficient to use.)

3. Rules for overriding:
    * Arguments must be the same, and return types must be compatible.
    * The method can't be less accessible.

4. Abstract:
    * Use `abstract` to make the base class abstract. `public abstract class animal {}` (can't be instantiated.)
    * Use `abstract` to make a method abstract `public abstract void eat();` (also no method body!)
    * Abstract methods must be put in abstract classes.
    * We must implement abstract methods by overriding.

5. Every class in Java extends the Object class which has the methods like: `.equals()`, `.hashCode()`, `.getClass()`.

## References

* [Head First Java, 2nd Edition](https://www.amazon.com/Head-First-Java-Kathy-Sierra/dp/0596009208/ref=sr_1_1?keywords=head+first+java&qid=1560043638&s=gateway&sr=8-1)
* <https://www.geeksforgeeks.org/difference-equals-method-java/>
