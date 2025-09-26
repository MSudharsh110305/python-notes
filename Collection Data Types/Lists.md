## Creation and basics
```
empty = []
nums = [1, 2, 3]
mixed = [1, "a", True, 3.14]
nested = [[1, 2], [3, 4]]
copy1 = list(nums)                 # shallow copy
copy2 = nums[:]                    # shallow copy
from_iter = list("abc")            # ['a','b','c']
```


## Indexing, slicing, and views
```
a = [10, 20, 30, 40, 50]
x = a[0]                           # 10
y = a[-1]                          # 50
sub = a[1:4]                       # [20,30,40]
head = a[:3]                       # [10,20,30]
tail = a[3:]                       # [40,50]
step = a[::2]                      # [10,30,50]
rev = a[::-1]                      # [50,40,30,20,10]
a[1:3] = [99, 100]                 # slice assign: [10,99,100,40,50]
a[2:2] = [7, 8]                    # insert via empty slice
a[1:4] = []                        # delete slice range
```


## Length, membership, concatenation, repetition, comparison
```
n = len([1,2,3])                   # 3
b = 2 in [1,2,3]                   # True
c = [1,2] + [3,4]                  # [1,2,3,4]
d = [0] * 4                        # [0,0,0,0]
e = [1,2,3] < [1,2,4]              # True (lexicographic)
```


## Mutating methods
```
a = [1, 2, 3]
a.append(4)                        # [1,2,3,4]
a.extend([5, 6])                   # [1,2,3,4,5,6]
a.insert(1, 99)                    # [1,99,2,3,4,5,6]
v = a.pop()                        # returns 6
v2 = a.pop(1)                      # returns 99
a.remove(3)                        # first 3 removed
a.clear()                          # []

```

## Non-mutating built-ins
```
nums = [3, 1, 4, 1, 5]
mx = max(nums)                     # 5
mn = min(nums)                     # 1
sm = sum(nums)                     # 14
any_true = any([0, "", 2])         # True
all_true = all([1, 2, 3])          # True
```


## Searching, counting, index
```
a = ["a", "b", "a", "c"]
i = a.index("a")                   # 0
cnt = a.count("a")                 # 2
```


## Sorting
```
a = [3, 1, 4, 1, 5]
a.sort()                           # in-place ascending
a.sort(reverse=True)               # in-place descending
a.sort(key=abs)                    # custom key
b = [3, 1, 4]
c = sorted(b)                      # [1,3,4]
d = sorted(b, reverse=True)        # [4,3,1]
e = sorted(["aa","b","ccc"], key=len)  # ['b','aa','ccc']

```

## Reversing
```
a = [1, 2, 3]
a.reverse()                        # in-place -> [3,2,1]
b = list(reversed([1, 2, 3]))      # [3,2,1]
```


## Copying and aliasing
```
a = [1, 2, 3]
b = a                              # alias (same object)
c = a[:]                           # shallow copy
d = list(a)                        # shallow copy
import copy
e = copy.deepcopy([[42], [43]])    # deep copy
```


## list.copy()
```
a = [1, 2, 3]
b = a.copy()                       # shallow copy
```


## List comprehension basics
```
squares = [x*x for x in range(5)]                  # [0,1,4,9,16]
evens = [x for x in range(10) if x % 2 == 0]       # [0,2,4,6,8]
pairs = [(i,j) for i in range(2) for j in range(2)]# [(0,0),(0,1),(1,0),(1,1)]
```


## Conditional expression in comprehension
```
labels = ["even" if x%2==0 else "odd" for x in range(5)]

```

## Multiple for/if in comprehension
```
pairs = [(i,j) for i in range(3) for j in range(i)]
nums = [i for i in range(100) if i > 10 if i < 20]

```

## Unpacking
```
points = [(1, 2), (3, 4)]
xs = [x for (x, y) in points]       # [1,3]
a, b = [10, 20]                     # a=10, b=20
head, *mid, tail = [1,2,3,4]        # head=1, mid=[2,3], tail=4
```


## Common slice patterns
```
a = [1,2,3,4,5]
a[:0] = [-1, 0]                     # prepend
a[len(a):] = [6,7]                  # append
a[::2] = [9,9,9]                    # extended slice assign
del a[1:3]                          # delete slice
del a[::2]                          # delete extended slice
```


## Enumerate, zip
```
for i, v in enumerate(["a","b","c"], start=1): pass
pairs = list(zip([1,2,3], ["a","b","c"]))

```

## Map/filter
```
vals = list(map(str, [1,2,3]))      # ['1','2','3']
evs = list(filter(lambda x: x%2==0, range(6)))
```


## itertools tricks
```
import itertools as it
a = [1,2,3]
list(it.accumulate(a))
list(it.chain(a, [4,5]))
list(it.combinations(a, 2))
list(it.permutations(a, 2))
list(it.product([0,1], repeat=2))
list(it.groupby(sorted("banana")))
```


## Stack and queue
```
stack = []
stack.append(1); stack.append(2); stack.pop()
from collections import deque
q = deque([1,2]); q.append(3); q.popleft()
```


## Binary search helpers
```
import bisect
a = [1,2,4,4,5]
i = bisect.bisect_left(a, 4)        # 2
j = bisect.bisect_right(a, 4)       # 4
bisect.insort(a, 3)                 # [1,2,3,4,4,5]
```


## Heapq (priority queue)
```
import heapq
h = []
heapq.heappush(h, 3); heapq.heappush(h, 1)
m = heapq.heappop(h)                # 1
heapq.heapify(h)
heapq.heapreplace(h, 10)
heapq.heappushpop(h, 0)
heapq.nlargest(2, [5,1,3,2,4])
heapq.nsmallest(2, [5,1,3,2,4])
```


## Transpose matrix
```
m = [[1,2,3],[4,5,6]]
t = [list(col) for col in zip(*m)]
```


## Flatten
```
nested = [[1,2],[3,4]]
flat = [x for sub in nested for x in sub]
```


## Deduplicate preserving order
```
seen = set()
dedup = [x for x in [3,1,3,2,1] if not (x in seen or seen.add(x))]
```


## Partition by predicate
```
a = [1,2,3,4,5]
left = [x for x in a if x%2==0]
right = [x for x in a if x%2!=0]
```


## Rotate with deque
```
from collections import deque
d = deque([1,2,3,4]); d.rotate(1)
rot = list(d)

```

## Frequencies
```
from collections import Counter
cnt = Counter([1,2,1,3,2,1])
top2 = cnt.most_common(2)
```


## Stable sort multi-key
```
from operator import itemgetter
data = [("b",2),("a",3),("a",1)]
data.sort(key=itemgetter(0,1))

```