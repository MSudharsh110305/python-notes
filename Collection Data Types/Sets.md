## Creation and basics
```
s = {1, 2, 3}                      # set literal with elements
t = set([1, 2, 2, 3])              # {1, 2, 3} from iterable (duplicates removed)
u = set("banana")                  # {'b','a','n'}
empty = set()                      # empty set ({} is an empty dict)
```

## Membership, size, truthiness
```
2 in {1,2,3}                       # True
0 in {1,2,3}                       # False
len({1,2,3})                       # 3
bool(set())                        # False; non-empty sets are True
```

## Add, update, remove
```
s = {1, 2}
s.add(3)                           # {1,2,3}
s.update([3, 4], {5, 6})           # add from iterables -> {1,2,3,4,5,6}
s.remove(2)                        # remove 2; KeyError if absent
s.discard(99)                      # no error if absent
x = s.pop()                        # remove and return an arbitrary element
s.clear()                          # empty set
```

## Set algebra (operators and methods)
```
A = {1,2,3}; B = {3,4,5}

A | B                              # union -> {1,2,3,4,5}
A & B                              # intersection -> {3}
A - B                              # difference -> {1,2}
A ^ B                              # symmetric difference -> {1,2,4,5}

A.union(B)                         # union
A.intersection(B)                  # intersection
A.difference(B)                    # difference
A.symmetric_difference(B)          # symmetric difference
```

## Subset/superset relations (comparisons)
```
A <= B                             # subset (A is subset of B)
A < B                              # proper subset
A >= B                             # superset
A > B                              # proper superset
A.isdisjoint(B)                    # True if A∩B is empty
```

## In-place algebra (mutating)
```
A = {1,2,3}; B = {3,4}
A |= B                             # A = A ∪ B -> {1,2,3,4}
A &= {2,3,5}                       # A = A ∩ {...} -> {2,3}
A -= {3}                           # A = A \ {...} -> {2}
A ^= {2,7}                         # A = A △ {...} -> {7}
```

## Copying and construction
```
A = {1,2,3}
B = set(A)                         # shallow copy
C = A.copy()                       # shallow copy
D = set(range(3))                  # {0,1,2}
```

## Set comprehension
```
squares = {x*x for x in range(6)}  # {0,1,4,9,16,25}
evens = {x for x in range(10) if x % 2 == 0}
pairs = {(i, j) for i in range(2) for j in range(2)}  # tuples are hashable
```

## Unhashable elements not allowed (use frozenset/tuple)
```
# { [1,2], 3 }          # TypeError: unhashable type: 'list'
ok = { (1,2), 3 }                      # tuples are hashable
key = frozenset({1,2})                 # frozenset is hashable
ok2 = { key, frozenset({3}) }          # set of frozensets
```

## frozenset: immutable set
```
fs = frozenset([1, 2, 2, 3])           # frozenset({1, 2, 3})
fs2 = frozenset("abca")                # frozenset({'a','b','c'})
# usable as dict keys or set elements
d = {fs: "immutable key"}
# frozenset supports all non-mutating ops: | & - ^, subset tests, etc.
fs3 = fs | frozenset({4})              # frozenset({1,2,3,4})
```

## Removing many elements efficiently (difference/update)
```
A = set(range(10))
A.difference_update({0,1,2})          # A = A \ {0,1,2}
A.intersection_update(range(5))       # A = A ∩ {0..4}
```

## Disjointness and filters
```
A = {1,3,5}; B = {2,4}
A.isdisjoint(B)                        # True
filtered = {x for x in range(10) if x not in {0,1,2}}
```

## Convert between containers
```
lst = list({3,1,2})                   # order arbitrary
tup = tuple({1,2})                    # (1,2) in arbitrary order
st = set([1,1,2,3])                   # {1,2,3}
```

## Deduplicate and preserve original order (stable)
```
seen = set()
dedup = [x for x in [3,1,3,2,1] if not (x in seen or seen.add(x))]

```
## Intersections/Unions across many sets
```
sets = [{1,2,3}, {2,3,4}, {0,2,3,5}]
common = set.intersection(*sets)       # {2,3}
union_all = set.union(*sets)           # {0,1,2,3,4,5}
```

## Build adjacency (graph) with sets
```
edges = [("A","B"),("A","C"),("B","C")]
adj = {}
for u, v in edges:
    adj.setdefault(u, set()).add(v)
    adj.setdefault(v, set()).add(u)
```

## Remove items while iterating: create new set
```
S = {1,2,3,4}
S = {x for x in S if x % 2 == 0}      # {2,4}
```

## Sorted views when deterministic order needed
```
s = {3,1,2}
sorted_list = sorted(s)                # [1,2,3]
```

## Set comparison across set/frozenset types (works)
```
set({1,2}) <= frozenset({1,2,3})      # True
frozenset({1,2,3}) > set({1,2})       # True

```
## Performance notes
```
# membership: average O(1)
# add/remove: average O(1)
# iteration order: arbitrary (insertion-affected but not guaranteed)
```

## Empty set literal caveat
```
# {} is an empty dict; use set() for empty set
empty = set()

```