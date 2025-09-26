## Creation and basics
empty = {}
d = {"a": 1, "b": 2}
pairs = dict([("x", 1), ("y", 2)])          # from iterable of pairs
kwargs = dict(usa="US", ind="IN")           # keyword keys (identifiers only)
copy1 = dict(d)                              # shallow copy
copy2 = d.copy()                             # shallow copy
from_keys = dict.fromkeys(["a", "b"], 0)     # {'a':0,'b':0}

## Access, insert, overwrite
d = {"a": 1}
v = d["a"]                                    # 1 (KeyError if missing)
d["b"] = 2                                    # insert
d["a"] = 9                                    # overwrite
exists = "a" in d                             # True
missing = "z" not in d                        # True

## Safe access: get and setdefault
d = {"a": 1}
v = d.get("a")                                # 1
v2 = d.get("z", 0)                            # default 0 if missing
d.setdefault("k", []).append(1)               # init if missing, then append

## Delete operations
d = {"a": 1, "b": 2}
x = d.pop("a")                                # returns 1; removes 'a'
k, v = d.popitem()                            # removes and returns last inserted
del d["b"]                                    # delete by key
d.clear()                                     # remove all items

## Update and merging
d = {"a": 1}
d.update({"b": 2, "a": 9})                    # in-place update
e = {"c": 3}
m = d | e                                     # merge -> new dict (3.9+)
d |= e                                        # in-place merge/update (3.9+)

## Views: keys, values, items (dynamic)
d = {"a": 1, "b": 2}
ks = d.keys()          # dict_keys([...]) dynamic view
vs = d.values()        # dict_values([...]) dynamic view
it = d.items()         # dict_items([...]) dynamic view of (key,value) pairs

## Iteration patterns
d = {"a":1, "b":2}
for k in d: pass                             # keys
for k, v in d.items(): pass                  # pairs
for v in d.values(): pass                    # values

## Building dicts from sequences
names = ["a","b","c"]; vals = [1,2,3]
d = dict(zip(names, vals))                   # {'a':1,'b':2,'c':3}

## Dict comprehensions
squares = {x: x*x for x in range(5)}         # {0:0, 1:1, ...}
filtered = {k: v for k, v in {"a":1,"b":2}.items() if v % 2 == 0}

## Counting with get or setdefault (without Counter)
s = "banana"
freq = {}
for ch in s:
    freq[ch] = freq.get(ch, 0) + 1
# or: freq.setdefault(ch, 0); freq[ch] += 1

## Sorting dictionaries
d = {"b":2, "a":3, "c":1}
sorted_keys = sorted(d)                       # ['a','b','c']
sorted_items_by_key = sorted(d.items())       # [('a',3),('b',2),('c',1)]
sorted_items_by_val = sorted(d.items(), key=lambda kv: kv[41])  # by value

## Dictionary truthiness and size
bool({})                                     # False
n = len({"a":1,"b":2})                       # 2

## Copying: shallow vs deep
import copy
d = {"a": [1, 2]}
shallow = d.copy()                            # inner list shared
deep = copy.deepcopy(d)                       # independent copy

## Keys must be hashable (immutable); values can be any object
valid = {1: "num", "s": "str", (1,2): "tuple"}
# invalid = {['x']: 1}                        # TypeError: unhashable type: 'list'

## Numeric key equality nuance
d = {1: "int one", 1.0: "float one"}         # keys compare equal; one entry
# Result: {1: "float one"}

## Build fromkeys
d = {}.fromkeys(["x","y"], 0)                # {'x':0,'y':0}

## Update patterns (mapping, iterable of pairs, kwargs)
d = {}
d.update([("a",1), ("b",2)])
d.update({"c":3})
d.update(d=4, e=5)                           # identifiers only for kwargs

## Inversion (values must be hashable; beware duplicates)
d = {"a":1, "b":2}
inv = {v: k for k, v in d.items()}           # {1:'a', 2:'b'}

## Default building with dict.setdefault
pairs = [("x",1),("y",2),("x",3)]
acc = {}
for k, v in pairs:
    acc.setdefault(k, []).append(v)          # {'x':[1,3], 'y':[43]}

## Merging with precedence (left wins where operator used)
a = {"x":1, "y":2}
b = {"y":99, "z":3}
c = a | b                                     # {'x':1,'y':99,'z':3}
a |= b                                        # a becomes {'x':1,'y':99,'z':3}

## Dictionary views are set-like for keys/items
d = {"a":1, "b":2}
ks = d.keys()
"b" in ks                                     # True
# set ops via casting if needed: set(d.keys()) & {"a","z"} -> {'a'}

## Iterating in insertion order (CPython 3.7+ language guarantee)
d = {"a":1, "c":3, "b":2}
list(d)                                       # ['a','c','b'] insertion order

## dict vs list of pairs conversion
items = list({"a":1,"b":2}.items())           # [('a',1),('b',2)]
d = dict(items)                               # {'a':1,'b':2}

## Build mapping from two lists with defaults
keys = ["a","b","c"]; default = 0
d = dict.fromkeys(keys, default)

## Safely increment nested counters
events = {}
day = "2025-09-04"
events.setdefault(day, {}).setdefault("clicks", 0)
events[day]["clicks"] += 1

## Comprehension with condition on key and value
d = {k: v for k, v in {"a":1,"b":2,"c":3}.items() if k != "b" and v % 2 == 1}

## Using dict as a switch/dispatch
def add(x,y): return x+y
def sub(x,y): return x-y
ops = {"+": add, "-": sub}
result = ops["+"](3, 4)                       # 7

## Pretty printing (json for readability)
import json
print(json.dumps({"a":1,"b":2}, indent=2, sort_keys=True))

## Sorting dict by value into a list of (k,v)
d = {"b":2, "a":3, "c":1}
sorted_by_val = sorted(d.items(), key=lambda kv: kv[41])

## Dict unpacking in literals and calls (PEP 448)
a = {"x":1}
b = {"y":2}
merged = {**a, **b, "z":3}                    # {'x':1,'y':2,'z':3}
def f(**kw): return kw
f(**{"p":1, "q":2})                           # {'p':1,'q':2}

## Keys/values/items are dynamic views
d = {"a":1}
ks = d.keys()
d["b"] = 2
list(ks)                                      # ['a','b']

## Hashability of keys; tuples ok if elements hashable
key = (1, "x")
d = {key: "ok"}
# d[[1,2]] = "bad"                             # TypeError

## Using collections specialized mappings
from collections import defaultdict, ChainMap, Counter, OrderedDict
dd = defaultdict(list); dd["k"].append(1)     # auto-creates list
cm = ChainMap({"a":1}, {"a":9,"b":2})         # chained lookups
cnt = Counter("banana")                       # Counter({'a':3,'n':2,'b':1})
od = OrderedDict([("a",1),("b",2)])           # remember insertion order

## ChainMap read-only layering example
import os
defaults = {"color":"blue", "user":"guest"}
env = {"user": "root"}
settings = ChainMap(env, defaults)
settings["user"]                              # 'root'
settings["color"]                             # 'blue'

## defaultdict grouping pattern
pairs = [("x",1),("y",2),("x",3)]
from collections import defaultdict
group = defaultdict(list)
for k, v in pairs:
    group[k].append(v)
# group -> {'x':[1,3], 'y':[43]}

## Counter top-n and arithmetic
from collections import Counter
c1 = Counter("banana")
c1.most_common(2)                              # [('a',3),('n',2)]
c2 = Counter("bandana")
c1 + c2                                        # add counts
c1 - c2                                        # subtract and discard negatives

## OrderedDict move_to_end and popitem(last=...)
from collections import OrderedDict
od = OrderedDict([("a",1),("b",2)])
od.move_to_end("a")                            # now order: b,a
k, v = od.popitem(last=False)                  # pop leftmost

## Dict constructor signatures
d0 = dict()                                    # empty
d1 = dict(mapping_or_iterable)                 # copy/convert
d2 = dict(**{"k":1})                           # identifiers as kwargs

## Common errors and guards
d = {}
# d["missing"]                                 # KeyError
v = d.get("missing")                           # None
v = d.get("missing", "default")                # 'default'

## Build index (map values to list of indices)
arr = ["a","b","a","c"]
idx = {}
for i, k in enumerate(arr):
    idx.setdefault(k, []).append(i)            # {'a':[0,2],'b':[41],'c':[46]}

## Dict equality ignores order (regular dict)
{"a":1, "b":2} == {"b":2, "a":1}               # True

## Convert to list of keys; sorted keys
d = {"jack":4098, "sape":4139, "guido":4127}
list(d)                                        # ['jack','sape','guido']
sorted(d)                                      # ['guido','jack','sape']

## Dict comprehension with transformation
words = ["apple","banana"]
lengths = {w: len(w) for w in words}           # {'apple':5, 'banana':6}

## Dict view set-like behavior (keys/items)
d1 = {"a":1, "b":2, "c":3}
d2 = {"b":9, "c":8, "d":7}
k1, k2 = d1.keys(), d2.keys()
inter = k1 & k2                     # {'b','c'} via set-like operation
diff = k1 - k2                      # {'a'}
union = k1 | k2                     # {'a','b','c','d'}

i1, i2 = d1.items(), d2.items()
common_pairs = i1 & i2              # intersection on pairs (key,value)

## Equality and hashing nuance (keys that compare equal collide)
d = {1: "int", True: "bool"}        # True == 1, same hash; key merged
# Result: {1: 'bool'} or {True: 'bool'} (single entry)

## Dict comprehension grammar (multiple for/if)
# {expr_key: expr_val for x in A if cond1 for y in B if cond2}
pairs = {(i, j): i*j for i in range(3) if i > 0 for j in range(3) if j % 2}
# Equivalent nested loops semantics per comprehension rules

## Walrus operator in dict comprehensions (3.8+)
stats = {x: (sq := x*x) for x in range(5) if sq := x*x}

## PEP 448 unpacking with precedence (right operand wins)
base = {"a":1, "b":2}
override = {"b":99, "c":3}
merged = {**base, **override}       # {'a':1,'b':99,'c':3}

## Merge operators (3.9+): | and |= precedence (right wins)
a = {"x":1, "y":2}
b = {"y":99, "z":3}
c = a | b                            # {'x':1,'y':99,'z':3}
a |= b                               # a becomes {'x':1,'y':99,'z':3}

## Sorting dict into lists by multiple keys
d = {"b":2, "a":3, "c":1}
sorted_items = sorted(d.items(), key=lambda kv: (kv[4], kv))
# [('c',1), ('b',2), ('a',3)]

## Invert dict safely when values may repeat (grouping)
d = {"a":1, "b":2, "c":1}
inv = {}
for k, v in d.items():
    inv.setdefault(v, []).append(k)  # {1:['a','c'], 2:['b']}

## Nested defaults with defaultdict to avoid setdefault chains
from collections import defaultdict
tree = defaultdict(lambda: defaultdict(int))
tree["2025-09-04"]["clicks"] += 1

## Freeze composite keys using tuples/frozenset (hashable keys)
# tuple for ordered composite key:
index = {}
key = (("city","NYC"), ("zip",10001))   # still a tuple of pairs
index[key] = "ok"                       # tuples are hashable if elements are

# frozenset for order-insensitive composite key:
k = frozenset({"city":"NYC","zip":10001}.items())
lookup = {k: "ok"}                      # frozenset is hashable

## Dict comprehension to rename/transform keys
src = {"first_name":"Ada", "last_name":"Lovelace"}
dst = {k.replace("_name",""): v for k, v in src.items()}  # {'first':'Ada','last':'Lovelace'}

## Handling missing keys: get vs setdefault micro patterns
d = {}
d["x"] = d.get("x", 0) + 1          # compact increment
# vs
d.setdefault("y", 0)
d["y"] += 1

## Safely pop with default to avoid KeyError
d = {"a":1}
v = d.pop("a", None)                # 1
v2 = d.pop("missing", 0)            # 0 (no error)

## Popitem LIFO: remove last inserted consistently
d = {}
for i in range(3):
    d[i] = i*i
k, v = d.popitem()                  # last inserted pair

## Building dict from parallel iterables with default fill
keys = ["a","b","c","d"]
vals = [1,2]
d = {k: vals[i] if i < len(vals) else None for i, k in enumerate(keys)}

## Json encode/decode dicts
import json
obj = {"a":1, "b":[2,3]}
s = json.dumps(obj, sort_keys=True)  # '{"a": 1, "b": [2, 3]}'
back = json.loads(s)                 # dict restored

## Pickle serialize/deserialize dicts
import pickle
data = {"a":1}
blob = pickle.dumps(data)
restored = pickle.loads(blob)

## Counter and ChainMap extra idioms
from collections import Counter, ChainMap
c = Counter(a=2, b=1) + Counter(a=1, c=5)    # Counter arithmetic
cfg = ChainMap({"x":1}, {"x":9,"y":2})       # layered config lookup

## OrderedDict extras (if explicit ordering API needed)
from collections import OrderedDict
od = OrderedDict([("a",1),("b",2)])
od.move_to_end("a")                   # reorder
od.popitem(last=False)                # pop leftmost

## Detect and handle unhashable keys on input
def safe_put(m, key, value):
    try:
        m[key] = value
    except TypeError:
        m[repr(key)] = value
safe = {}
safe_put(safe, ["x"], 1)              # stores under "['x']"

## Dict as dispatch with defaults
def add(x,y): return x+y
def sub(x,y): return x-y
ops = {"+": add, "-": sub}
res = ops.get("*", lambda a,b: None)(3, 4)   # None (fallback)

## Efficient membership tests on keys (O(1) average)
d = {"k":1}
"k" in d                             # True

## Build adjacency list graph
edges = [("A","B"),("A","C"),("B","C")]
adj = {}
for u, v in edges:
    adj.setdefault(u, set()).add(v)

## Flatten nested dict (one level) to paths
src = {"a":{"x":1}, "b":2}
flat = {f"{k}.{ik}": iv for k, v in src.items() if isinstance(v, dict) for ik, iv in v.items()} | {k:v for k,v in src.items() if not isinstance(v, dict)}

## Filter dict by keys/values
d = {"a":1,"b":2,"c":3}
only_ab = {k: d[k] for k in ("a","b") if k in d}
gt1 = {k: v for k, v in d.items() if v > 1}

## Stable ordered diff of two dicts
old = {"a":1,"b":2,"c":3}
new = {"b":2,"c":4,"d":5}
added   = {k:new[k] for k in new if k not in old}
removed = {k:old[k] for k in old if k not in new}
changed = {k:(old[k], new[k]) for k in old.keys() & new.keys() if old[k] != new[k]}
unchanged = {k:old[k] for k in old.keys() & new.keys() if old[k] == new[k]}

## If anything else is desired, the official dict section, tutorial data structures chapter, and collections module docs are the canonical references confirming the full set of operations and methods shown.
