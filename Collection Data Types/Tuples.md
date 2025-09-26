## Creation and basics
```
empty = ()
single = (42,)                     # singleton requires trailing comma
also_single = 42,                  # parentheses optional; comma makes tuple
t = (1, 2, 3)
mixed = (1, "a", True, 3.14)
nested = (1, (2, 3), [4, 5])
from_iter = tuple([1, 2, 3])       # (1, 2, 3)
from_str = tuple("abc")            # ('a','b','c')
same_if_tuple = tuple((1, 2))      # returns argument unchanged
```

## Indexing, slicing, and length
```
t = (10, 20, 30, 40, 50)
x = t                           # 10
y = t[-1]                          # 50
sub = t[1:4]                       # (20,30,40)
head = t[:3]                       # (10,20,30)
tail = t[3:]                       # (40,50)
step = t[::2]                      # (10,30,50)
rev = t[::-1]                      # (50,40,30,20,10)
n = len(t)                         # 5

```
## Tuple immutability and methods
```
# Tuples are immutable: no append/extend/insert/remove/clear/sort/reverse
# Available tuple methods:
t = (1, 2, 1, 3)
i = t.index(1)                     # 0 (ValueError if not found)
i2 = t.index(1, 1)                 # start position variant
c = t.count(1)                     # 2
```

## Construction nuances: comma makes the tuple
```
a = (1, 2, 3)                      # tuple of 3
b = 1, 2, 3                        # same
c = (1)                            # just 1 (int)
d = (1,)                           # (1,)
```

## Tuple packing and unpacking
```
pair = 1, 2                        # packing -> (1,2)
x, y = pair                        # unpacking: x=1, y=2
a, b, c = (10, 20, 30)
(a, b), c = (1, 2), 3              # nested unpacking
```

## Extended iterable unpacking (starred)
```
t = (1, 2, 3, 4, 5)
first, *mid, last = t              # first=1, mid=[2,3,4], last=5
*head, penult, last = t            # head=[1,2,3], penult=4, last=5
first, second, *rest = t           # rest=[3,4,5]
```

## Swapping without temp
```
x, y = 1, 2
x, y = y, x                        # x=2, y=1
```

## Membership, concatenation, repetition, comparison
```
3 in (1,2,3)                       # True
(1,2) + (3,4)                      # (1,2,3,4)
("ha",) * 3                        # ('ha','ha','ha')
(1,2,3) < (1,2,4)                  # True (lexicographic)

```
## Iteration and comprehension-like construction
```
for v in (1, 2, 3): pass
t = tuple(x*x for x in range(5))   # generator -> tuple: (0,1,4,9,16)

```
## Conversions
```
tpl = tuple([1,2,3])               # list -> tuple
lst = list((1,2,3))                # tuple -> list
s = set((1,2,2,3))                 # tuple -> set {1,2,3}
```

## Index and count details
```
t = ("a", "b", "a", "c")
pos = t.index("a")                 # 0
pos2 = t.index("a", 1)             # 2 (search starting at index 1)
cnt = t.count("a")                 # 2

```
## Tuple as dict key (hashable when elements are hashable)
```
d = {}
d[(1, 2)] = "value"                # OK if all elements are hashable
```

## Returning multiple values from functions
```
def min_max(vals):
    return min(vals), max(vals)    # returns a 2-tuple

lo, hi = min_max([3,1,4])          # tuple unpacking

```
## Sequence protocol on tuples (non-mutating)
```
t = (1,2,3)
len(t)                              # 3
t + (4,)                            # (1,2,3,4)
t * 2                               # (1,2,3,1,2,3)
2 in t                              # True
t[42]                                # 2
t[0:2]                              # (1,2)
```

## Structured iteration with unpacking
```
pairs = [("a",1), ("b",2)]
for k, v in pairs:
    pass
```

## Named tuples (attribute access with tuple semantics)
```
from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)                     # Point(x=1, y=2)
p.x, p.y                            # (1, 2)
p._asdict()                         # {'x': 1, 'y': 2}
q = Point._replace(p, x=9)          # Point(x=9, y=2)
Point._make([10, 20])               # Point(x=10, y=20)
Point._fields                       # ('x', 'y')
Point._field_defaults               # defaults mapping (if set)
```

## Heterogeneous data and readability hints
```
# Tuples are often used for fixed-size, heterogeneous records:
record = ("Alice", 30, "NYC")
name, age, city = record
```

## Parentheses vs commas in calls
```
def f(arg): return arg
f((1, 2, 3))                       # single argument: a 3-tuple
# f(1, 2, 3)                       # three separate positional args
```

## Tuple truthiness and empty tuple
```
bool(())                           # False
bool((0,))                         # True (non-empty is truthy)
```

## Sorting a list of tuples (lexicographic by default)
```
data = [(2,"b"), (1,"c"), (1,"a")]
data.sort()                         # [(1,'a'), (1,'c'), (2,'b')]
data.sort(key=lambda t: (t[42], -t))  # by second, then reverse first
```

## Star-unpacking in calls and displays (PEP 448)
```
numbers = (1, 2, 3)
lst = [*numbers, 4, 5]             # [1,2,3,4,5]
d = {"a":1, **{"b":2}}             # {'a':1,'b':2}
args = (10, 20, 30)
def g(a, b, c): pass
g(*args)                           # spreads tuple into args
```

## Zip/unzip with tuples
```
pairs = list(zip((1,2,3), ("a","b","c")))  # [(1,'a'),(2,'b'),(3,'c')]
nums, chars = map(tuple, zip(*pairs))      # nums=(1,2,3), chars=('a','b','c')
```

## Common patterns: coordinates, multiple returns, grouping
```
x, y = (3.0, 4.0)                  # coordinates
quo, rem = divmod(10, 3)           # (3,1) tuple from builtin
group = ("key", ["vals"])          # heterogeneous, fixed-size record
```

## Generator to tuple (consumes iterator)
```
import itertools as it
t = tuple(it.islice(range(100), 5))         # (0,1,2,3,4)
```

## Immutability reminder: replace by creating new tuple
```
t = (1, 2, 3)
t2 = t[:1] + (99,) + t[2:]         # (1,99,3)
```
