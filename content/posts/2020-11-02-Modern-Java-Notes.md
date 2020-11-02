---
title: "Modern Java Notes"
date: 2020-11-02T19:23:08+01:00
draft: false
description: "Modern Java notes for Java 8+."
tags: ["Programming Languages", "Java"]
---

* [Method reference and lambdas](#method-reference-and-lambdas)
* [Streams](#streams)
* [Default methods](#default-methods)
* [Miscellaneous](#miscellaneous)
* [References](#references)

Notes for the modern Java (Java 8+.)

## Method reference and lambdas

1. Java 8+ treats functions and lambdas as first-class citizens, which means we can pass functions around using method reference. Note that lambdas can only capture `final` variables in the same scope.

    ```java
    inventory.sort(comparing(Apple::getWeight));
    // Or
    inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
    // And the types can be inferred
    inventory.sort((a1, a2) -> a1.getWeight().compareTo(a2.getWeight()));
    // Instead of
    Collections.sort(inventory, new Comparator<Apple>() {
        public int compare(Apple a1, Apple a2) {
            return a1.getWeight().compareTo(a2.getWeight());
        }
    });

    File[] hiddenFiles = new File(".").listFiles(File::isHidden);
    // Instead of
    File[] hiddenFiles = new File(".").listFiles(new FileFilter() {
        public boolean accept(File file) {
            return file.isHidden();
        }
    });

    filterApples(inventory, (Apple a) -> a.getWeight() > 150 );

    // We can also use a Predicate to achieve behavior parameterization.

    public interface Predicate<T> {
        boolean test(T t);
    }
    public static <T> List<T> filter(List<T> list, Predicate<T> p) {
        List<T> result = new ArrayList<>();
        for(T e: list) {
            if(p.test(e)) {
                parameter T result.add(e);
            }
        }
        return result;
    }

    filter(numbers, (Integer i) -> i % 2 == 0);

    Thread t = new Thread(() -> System.out.println("Hello world"));
    // Instead of
    Thread t = new Thread(new Runnable() {
        public void run() {
            System.out.println("Hello world");
        }
    });

    // Callable is like the upgraded Runnable. It sends the task to a tread pool and the result
    // is stored in a Future.
    ExecutorService executorService = Executors.newCachedThreadPool();
    Future<String> threadName = executorService.submit( () -> Thread.currentThread().getName());
    // Instead of
    Future<String> threadName = executorService.submit(new Callable<String>() {
        @Override
        public String call() throws Exception {
            return Thread.currentThread().getName();
        }
    });

    // We can also use .and() .or() to create more complicated lambdas.
    Predicate<Apple> redAndHeavyAppleOrGreen =
        redApple.and(apple -> apple.getWeight() > 150).or(apple -> GREEN.equals(apple.getColor()));

    // Composing functions.
    f.andThen(g) // g(f(x))
    f.compose(g) // f(g(x))
    ```

## Streams

1. By using streams, we get parallel processing for free.

    ```java
    import static java.util.stream.Collectors.toList;
    // Sequential
    List<Apple> heavyApples = inventory.stream()
        .filter((Apple a) -> a.getWeight() > 150).collect(toList());
    // Parallel
    List<Apple> heavyApples = inventory.parallelStream()
        .filter((Apple a) -> a.getWeight() > 150).collect(toList());
    ```

## Default methods

1. Default methods for an interface allow concrete implementations not have to change.

    ```java
    // In List
    default void sort(Comparator<? super E> c) {
        Collections.sort(this, c);
    }
    // This made it possible to call apples.sort().
    ```

## Miscellaneous

1. Diamond operator `<>`:

    ```java
    List<String> listOfStrings = new ArrayList<>(); // The type here will be inferred.
    ```

## References

* [Modern Java in Action](https://www.goodreads.com/book/show/46213396-modern-java-in-action?from_search=true&from_srp=true&qid=Sqwlop5UTf&rank=1)
