---
title: "Python Advanced Tricks"
date: 2020-11-02T17:45:00+01:00
draft: false
description: "Advanced tricks in Python that could be useful for tech interviews."
tags: ["Programming Languages", "Python"]
---
* [Decorators](#decorators)
  * [functools](#functools)
    * [cached_property](#cached_property)
    * [lru_cache](#lru_cache)
    * [reduce](#reduce)
  * [dataclasses](#dataclasses)
    * [dataclass](#dataclass)
* [Data Structures](#data-structures)
  * [Dictioaries](#dictioaries)
    * [OrderedDict](#ordereddict)
    * [defaultdict](#defaultdict)
    * [ChainMap](#chainmap)
    * [MappingProxyType](#mappingproxytype)
  * [Arrays](#arrays)
    * [array.array](#arrayarray)
    * [bytes](#bytes)
    * [bytearray](#bytearray)
  * [Records, Structs, and Data Transfer Objects](#records-structs-and-data-transfer-objects)
    * [namedtuple](#namedtuple)
    * [SimpleNamespace](#simplenamespace)
  * [Sets and Multisets](#sets-and-multisets)
    * [frozenset](#frozenset)
    * [Counter](#counter)
  * [Stacks](#stacks)
    * [list](#list)
    * [deque](#deque)
    * [LifoQueue](#lifoqueue)
  * [Queues](#queues)
    * [list](#list-1)
    * [deque](#deque-1)
    * [queue.Queue](#queuequeue)
    * [multiprocessing.Queue](#multiprocessingqueue)
  * [Priority Queues](#priority-queues)
    * [list](#list-2)
    * [heapq](#heapq)
    * [queue.PriorityQueue](#queuepriorityqueue)
  * [Enums](#enums)
    * [Enum](#enum)
    * [IntEnum](#intenum)
* [Functions](#functions)
  * [Lambdas](#lambdas)
* [Classes and OOP](#classes-and-oop)
  * [Own Exceptions](#own-exceptions)
  * [Shallow and Deep Copy](#shallow-and-deep-copy)
  * [Abstract Base Class](#abstract-base-class)
* [Looping and Iteration](#looping-and-iteration)
  * [Enumerate](#enumerate)
  * [Iterators](#iterators)
  * [Generators](#generators)
  * [Equality](#equality)
* [Itertools](#itertools)
  * [groupby](#groupby)
  * [permutations and combinations](#permutations-and-combinations)
* [Miscellaneous](#miscellaneous)
  * [stdin](#stdin)
  * [Binary conversion](#binary-conversion)
  * [String](#string)
  * [Walrus operator](#walrus-operator)
  * [Compare with zip](#compare-with-zip)

This contains some advanced tricks in Python that could be useful for tech interviews.

## Decorators

### functools

#### cached_property

Just like `@property`, with extra caching for expensive properties

```py
from functools import cached_property

class DataSet:
    def __init__(self, sequence_of_numbers):
        self._data = sequence_of_numbers

    @cached_property
    def stdev(self):
        return statistics.stdev(self._data)

    @cached_property
    def variance(self):
        return statistics.variance(self._data)
```

#### lru_cache

It can be used for automatic memoization. If `maxsize=None`, lru feature is turned off and the limit is without bound.

```py
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n: int) -> int:
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

print([fib(n) for n in range(16)])

fib.cache_info()

```

```sh
    [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610]





    CacheInfo(hits=28, misses=16, maxsize=None, currsize=16)
```

#### reduce

It applies the function cumulatively to the iterable.

```py
from functools import reduce
from math import gcd

def gcd_for_list(items: int) -> int:
    return reduce(gcd, items)

print(gcd_for_list([2, 4, 6, 8, 10]))

print(reduce(lambda x, y: x + y, [1, 2, 3]))
```

```sh
    2
    6
```

### dataclasses

#### dataclass

This decorator adds `__init__()`, `__repr__()`, and other special methods automatically.

```py
from dataclasses import dataclass

@dataclass
class InventoryItem:
    """Class for keeping track of an item in inventory."""
    name: str
    unit_price: float
    quantity_on_hand: int = 0
```

## Data Structures

Reference: <https://realpython.com/python-data-structures/>

### Dictioaries

#### OrderedDict

It is a specialized `dict` subclass that remembers the insertion order.

```py
from collections import OrderedDict

popitem(last=True)
move_to_end(key, last=True)
```

#### defaultdict

It provides an easier way to convert tuples into a dictionary. Or it can be used to implement a counter.

```py
from collections import defaultdict


s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
# list is the default factory function that creates an empty list
# if the key is encourntered for the first time.
d = defaultdict(list)

for k, v in s:
    d[k].append(v)

print(d)

nums = [3, 5, 4, 6, 6, 1]
d = defaultdict(int)
for num in nums:
    d[num] += 1

print(d)
```

```sh
    defaultdict(<class 'list'>, {'yellow': [1, 3], 'blue': [2, 4], 'red': [1]})
    defaultdict(<class 'int'>, {3: 1, 5: 1, 4: 1, 6: 2, 1: 1})
```

#### ChainMap

It groups multiple dictionaries into one single mapping.

```py
from collections import ChainMap

dict1 = {
    'one': 1,
    'two': 2
}
dict2 = {
    'three': 3,
     'four': 4
}

# First, we can use unpacking to merge them into one.
print({**dict1, **dict2})

# Or we can use ChainMap.
chained_map = ChainMap(dict1, dict2)
print(chained_map)
# Which can be converted to a normal dict very easily.
print(dict(chained_map))
```

```sh
    {'one': 1, 'two': 2, 'three': 3, 'four': 4}
    ChainMap({'one': 1, 'two': 2}, {'three': 3, 'four': 4})
    {'two': 2, 'three': 3, 'four': 4, 'one': 1}
```

#### MappingProxyType

It is a read-only dictionary.

```py
from types import MappingProxyType
writable = {
    'one': 1,
    'two': 2
}
read_only = MappingProxyType(writable)
print(read_only)
```

```sh
    {'one': 1, 'two': 2}
```

### Arrays

#### array.array

It is a basic C-style typed array that's more space-efficient. Unlike list, it can only have on e type.

```py
import array

arr = array.array('f', (1.0, 1.5, 2.0, 2.5))
print(arr)
```

```sh
    array('f', [1.0, 1.5, 2.0, 2.5])
```

#### bytes

It is an **immutable** array of single bytes ( 0<= int <= 255). Like array, it's also space-efficient.

```py
arr = bytes((0, 1, 2, 255))
print(arr)
```

```sh
    b'\x00\x01\x02\xff'
```

#### bytearray

It is a **mutable** verson of bytes.

```py
arr = bytearray((0, 1, 2, 255))
arr.append(42)
del arr[0]
print(arr)
```

```sh
    bytearray(b'\x01\x02\xff*')
```

### Records, Structs, and Data Transfer Objects

#### namedtuple

```py
from collections import namedtuple

Car = namedtuple('Car', 'color mileage automatic')
car1 = Car('red', 3812.4, True)
print(car1.mileage)

# typing.NamedTuple provides type hints

from typing import NamedTuple

class CarCar(NamedTuple):
    color: str
    mileage: float
    automatic: bool

car2 = CarCar('red', 3812.4, True)
print(car2.mileage)
```

```sh
    3812.4
    3812.4
```

#### SimpleNamespace

It's a dictionary with attribute access.

```py
from types import SimpleNamespace
car1 = SimpleNamespace(color='red', mileage=3812.4, automatic=True)
print(car1.color)
```

```sh
    red
```

### Sets and Multisets

#### frozenset

It is an immutable, hashable set that can be used as dictionary keys or elements of another set.

```py
d = {frozenset({1, 2, 3}): 'hi'}
print(d[frozenset({1, 2, 3})])
```

```sh
    hi
```

#### Counter

It is a multiset or bag.

```py
from collections import Counter

counter = Counter(['a', 'b', 'a'])
print(counter)
print(len(counter))  # Unique elements.
print(sorted(counter.elements()))
print(sum(counter.values()))  # Total counts.
print(counter.most_common(n=1))
print(Counter('aaba'))
counter.update(['a'])
print(counter)
n = 1
print(counter.most_common()[:-n-1:-1])  # n lest common elements.
```

```sh
    Counter({'a': 2, 'b': 1})
    2
    ['a', 'a', 'b']
    3
    [('a', 2)]
    Counter({'a': 3, 'b': 1})
    Counter({'a': 3, 'b': 1})
    [('b', 1)]
```

### Stacks

#### list

The built-in `list` can be used as a stack with push and pop in amortized O(1). However, `list` needs to be resized occasionally.

```py
s = []
s.append('eat')
print(s.pop())
```

```sh
    eat
```

#### deque

It is a robust stack that can be also used as a queue. It's implemented as a doubly-linked list, which means O(1) insertion and deletion but O(n) random access.

```py
from collections import deque

s = deque()
s.append('eat')
print(s[-1])  # Top of the stack.
print(s.pop())
```

```sh
    eat
    eat
```

#### LifoQueue

It is a synchronized stack that provides locking semantics.

```py
from queue import LifoQueue

s = LifoQueue()
s.put('eat')
s.put('sleep')
print(s.get())
print(s.get())
```

```sh
    sleep
    eat
```

### Queues

#### list

It is a terrible choice for a queue since it's super slow for dequeing - O(n).

```py
q = []
q.append('eat')
print(q.pop(0))
```

```sh
    eat
```

#### deque

it is a robust queue that also be used as a stack.

```py
from collections import deque

q = deque()
q.append('eat')
print(q.popleft())
```

```sh
    eat
```

#### queue.Queue

It is a synchronized queue that provides locking semantics. It should be used for multi-threads in a single process.

```py
from queue import Queue

q = Queue()
q.put('eat')
q.put('sleep')
print(q.get())
print(q.get())
```

```sh
    eat
    sleep
```

#### multiprocessing.Queue

It is a synchronized queue that provides locking semantics. It should be used for multi-processes.

```py
from multiprocessing import Queue

q = Queue()
q.put('eat')
q.put('sleep')
print(q.get())
print(q.get())
```

```sh
    eat
    sleep
```

### Priority Queues

#### list

list can be used to implement a prioity queue. But insertion is a slow O(n)

```py
q = []
q.appen†d((2, 'code'))
q.append((1, 'eat'))
q.append((3, 'sleep'))
q.sort(reverse=True)
while q:
    print(q.pop())
```

```sh
    (1, 'eat')
    (2, 'code')
    (3, 'sleep')
```

#### heapq

It is a list-based binary min-heap. Insertion and extraction take O(log n).

```py
import heapq
q = []
heapq.heappush(q, (2, 'code'))
heapq.heappush(q, (1, 'eat'))
heapq.heappush(q, (3, 'sleep'))
while q:
    print(heapq.heappop(q))
```

```sh
    (1, 'eat')
    (2, 'code')
    (3, 'sleep')
```

#### queue.PriorityQueue

It uses heapq internally and is synchronized.

```py
from queue import PriorityQueue
q = PriorityQueue()
q.put((2, 'code'))
q.put((1, 'eat'))
q.put((3, 'sleep'))
while not q.empty():
    print(q.get())
```

```sh
    (1, 'eat')
    (2, 'code')
    (3, 'sleep')
```

### Enums

#### Enum

```py
from enum import Enum, unique, auto

@unique  # By default, the values are not unique.
class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

print(repr(Color.RED))
print(Color.RED.name)
print(Color.RED.value)
print(Color(1))
print(Color['RED'])

class ColorAuto(Enum):
    RED = auto()
    BLUE = auto()
    GREEN = auto()

print(list(ColorAuto))

for name, member in ColorAuto.__members__.items():
    print(f'{name}: {member}')

# Functional API.
Animal = Enum('Animal', 'ANT BEE CAT DOG')
print(Animal.ANT.value)
```

```sh
    <Color.RED: 1>
    RED
    1
    Color.RED
    Color.RED
    [<ColorAuto.RED: 1>, <ColorAuto.BLUE: 2>, <ColorAuto.GREEN: 3>]
    RED: ColorAuto.RED
    BLUE: ColorAuto.BLUE
    GREEN: ColorAuto.GREEN
    1
```

#### IntEnum

It is a subclass of int. This means that `IntEnum` can be compared to integers. However, `Enum` is favored over `IntEnum`.

```py
from enum import IntEnum

class Shape(IntEnum):
    CIRCLE = 1
    SQUARE = 2

class Request(IntEnum):
    POST = 1
    GET = 2

print(Shape.CIRCLE)
print(Shape.CIRCLE == 1)
print(Shape.CIRCLE == Request.POST)
```

```sh
    Shape.CIRCLE
    True
    True
```

## Functions

### Lambdas

```py
tuples = [(1, 'd'), (2, 'b'), (3, 'a')]
tuples.sort(key=lambda x: x[1])
print(tuples)
print(sorted(range(-5, 6), key=lambda x: x * x))

# But usually this
print([x for x in range(16) if x % 2 == 0])
# is better than
print(list(filter(lambda x: x % 2 == 0, range(16))))

xs = {
    'a': 4,
    'c': 2,
    'b': 3,
}
print(sorted(xs.items(), key=lambda x: x[1], reverse=True))
```

```sh
    [(3, 'a'), (2, 'b'), (1, 'd')]
    [0, -1, 1, -2, 2, -3, 3, -4, 4, -5, 5]
    [0, 2, 4, 6, 8, 10, 12, 14]
    [0, 2, 4, 6, 8, 10, 12, 14]
    [('a', 4), ('b', 3), ('c', 2)]
```

## Classes and OOP

### Own Exceptions

Defining our own exceptions can be helpful.

```py
class NameTooShortError(ValueError):
    pass

def validate(name):
    if len(name) < 10:
        raise NameTooShortError
```

### Shallow and Deep Copy

```py
xs = [[1, 2, 3], [4, 5, 6]]
yx = list(xs)  # Shallow copy.
ys = xs[:]  # Shallow copy again.
import copy
yx = copy.deepcopy(yx)
yx = copy.copy(yx)  # Shallow copy again.
```

### Abstract Base Class

```py
from abc import ABCMeta, abstractmethod

class Base(metaclass=ABCMeta):

    @abstractmethod
    def foo(self):
        pass  # Better than raise NotImplementedError.
```

## Looping and Iteration

### Enumerate

```py
for i, item in enumerate(['a', 'b', 'c']):
    print(f'{i}: {item}')
```

```sh
    0: a
    1: b
    2: c
```

### Iterators

A `for in` loop is a syntactic sugar for calling `__iter__` and `__next__`. And we can also use `iter()` and `next()` to invoke these dunders, just like `len()` for `__len__`. And since generators are just simplified iterators, we can also use `next()`.

### Generators

```py
print(sum(x * 2 for x in range(10)))
# is more performant than
print(sum((x * 2 for x in range(10))))
```

```sh
    90
    90
```

### Equality

A dictionary key is equal is they have the same `__hash__` and `__eq__`.

## Itertools

### groupby

It can be used for string compression.

```py
from itertools import groupby

s = [(k, len(list(v)))
    for k, v in groupby('aaabbccda')]
print(s)

s = [list(v) for _, v in groupby('aaabbccda')]
print(s)
```

```sh
    [('a', 3), ('b', 2), ('c', 2), ('d', 1), ('a', 1)]
    [['a', 'a', 'a'], ['b', 'b'], ['c', 'c'], ['d'], ['a']]
```

### permutations and combinations

```py
import itertools

friends = ['Monique', 'Ashish', 'Devon', 'Bernie']

print(list(itertools.permutations(friends, r=2)))
print(list(itertools.combinations(friends, r=2)))
```

```sh
    [('Monique', 'Ashish'), ('Monique', 'Devon'), ('Monique', 'Bernie'), ('Ashish', 'Monique'), ('Ashish', 'Devon'), ('Ashish', 'Bernie'), ('Devon', 'Monique'), ('Devon', 'Ashish'), ('Devon', 'Bernie'), ('Bernie', 'Monique'), ('Bernie', 'Ashish'), ('Bernie', 'Devon')]
    [('Monique', 'Ashish'), ('Monique', 'Devon'), ('Monique', 'Bernie'), ('Ashish', 'Devon'), ('Ashish', 'Bernie'), ('Devon', 'Bernie')]
```

## Miscellaneous

### stdin

```py
import sys

lines = [line.rstrip('\n') for line in sys.stdin.readlines()]
print(lines)

# Or

for line in sys.stdin:
    print(line.rstrip('\n'))
```

### Binary conversion

```py
print(f'{42:b}')
M = 10
format(42, f'0{M}b')
a = f'{42:08b}'
print(a)  # With leading zeros
print(int(a, 2))
```

```sh
    101010
    00101010
    42
```

### String

```py
import string


s = 'foobar'
print(s.startswith('foo'))
print(s.startswith('fooo'))
print(s[::-1])  # Reverse the string.

s = 'f.s,h;q'
s_no_punctuation = s.translate(str.maketrans('', '', string.punctuation))
print(s_no_punctuation)

# And some other handy constants:
print(
string.ascii_letters,
string.ascii_uppercase,
string.ascii_lowercase,
string.digits,
string.hexdigits,
string.octdigits,
string.punctuation,
string.printable,
string.whitespace
)

s = '121211'
print(s.count('1'))
print('1'.isnumeric())
```

```sh
    True
    False
    raboof
    fshq
    abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ ABCDEFGHIJKLMNOPQRSTUVWXYZ abcdefghijklmnopqrstuvwxyz 0123456789 0123456789abcdefABCDEF 01234567 !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~ 0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~


    4
    True
```

### Walrus operator

Use the walrus operator `:=` to reduce unnecessary assignment.

```py
s = 'hello'
if (s_len := len(s)) == 5:
   print(s_len)
```

```sh
      File "<ipython-input-1-00b419d10e06>", line 2
        if (s_len := len(s)) == 5:
                  ^
    SyntaxError: invalid syntax
```

### Compare with zip

```py
from itertools import zip_longest

s1 = 'abcde'
s2 = 'abbde'
for c1, c2 in zip(s1, s2):
    print(c1==c2)

s1 = 'abcde'
s2 = 'abbd'
print(list(zip(s1, s2)))
print(list(zip_longest(s1, s2)))
for c1, c2 in zip_longest(s1, s2):
    print(c1 == c2)
```

```sh
    True
    True
    False
    True
    True
    [('a', 'a'), ('b', 'b'), ('c', 'b'), ('d', 'd')]
    [('a', 'a'), ('b', 'b'), ('c', 'b'), ('d', 'd'), ('e', None)]
    True
    True
    False
    True
    False
```
