### **Mathematical Tricks**

**Count Digits in One Line**

```python
from math import log10
digits = int(log10(num)) + 1 if num > 0 else 1
```

**Check if a Number is a Power of 2**

```python
is_power_of_2 = lambda n: n > 0 and (n & (n - 1)) == 0
```

**Fast Exponentiation (Binary Exponentiation)**

```python
power = lambda base, exp: pow(base, exp)
```

**Swap Two Numbers Without a Temp Variable**

```python
a, b = b, a
```

**Find GCD Using One-Liner**

```python
from math import gcd
result = gcd(a, b)
```

---

### **Array & String Tricks**

**Reverse a String in One Line**

```python
reversed_string = s[::-1]
```

**Remove Duplicates from Sorted List in O(N)**

```python
unique_nums = list(dict.fromkeys(nums))
```


**Check If Two Strings Are Anagrams**

```python
from collections import Counter
is_anagram = lambda s, t: Counter(s) == Counter(t)
```

**Find First Non-Repeating Character in O(N)**

```python
from collections import Counter
first_unique = next((c for c in s if Counter(s)[c] == 1), '_')
```

---

### **Bit Manipulation**

**Find the Only Non-Repeating Number in an Array**

```python
from functools import reduce
unique_number = reduce(lambda x, y: x ^ y, nums)
```

**Check if a Number is Odd or Even in O(1)**

```python
is_even = lambda n: not n & 1
```

 **Toggle a Bit at a Given Position**

```python
n ^= (1 << pos)
```

**Check If Kth Bit is Set**

```python
is_set = lambda n, k: (n & (1 << k)) != 0
```

**Set or Unset the Kth Bit**

```python
n |= (1 << k)   # Set kth bit
n &= ~(1 << k)  # Unset kth bit
```

---

### **Data Structure Hacks**

**Use Counter for Fast Frequency Counting**

```python
from collections import Counter
freq = Counter(arr)
```

**Find Kth Largest Element Using Heap**

```python
import heapq
kth_largest = heapq.nlargest(k, nums)[-1]
```

**Two Sum in O(N) Using Dictionary**

```python
def two_sum(nums, target):
    seen = {}
    return next(([seen[target - n], i] for i, n in enumerate(nums) if target - n in seen or (seen.setdefault(n, i), False)[1]), [])
```

**Detect a Cycle in a Linked List (Floyd’s Algorithm)**

```python
def has_cycle(head):
    slow, fast = head, head
    while fast and fast.next:
        slow, fast = slow.next, fast.next.next
        if slow == fast:
            return True
    return False
```

**Find Lowest Common Ancestor in Binary Tree**

```python
def lowest_common_ancestor(root, p, q):
    return root if not root or root in (p, q) else next((x for x in (lowest_common_ancestor(root.left, p, q), lowest_common_ancestor(root.right, p, q)) if x), root)
```

---

### **Mathematical & Number Tricks**

1️⃣ **Sum of First N Natural Numbers in O(1)**

```python
sum_n = lambda n: n * (n + 1) // 2
```

2️⃣ **Convert a Number to Binary Without `bin()`**

```python
binary = lambda n: f"{n:b}"
```

3️⃣ **Find Square Root Without `math.sqrt()`**

```python
sqrt_n = lambda n: n ** 0.5
```

4️⃣ **Calculate nCr (Combinations) Using Factorial**

```python
from math import comb
nCr = comb(n, r)
```

5️⃣ **Find LCM Using GCD**

```python
from math import gcd
lcm = lambda a, b: (a * b) // gcd(a, b)
```

---

### **Array & String Tricks**

6️⃣ **Flatten a Nested List in One Line**

```python
from itertools import chain
flat_list = list(chain(*nested_list))
```

7️⃣ **Transpose a Matrix Using Zip**

```python
transposed = list(zip(*matrix))
```

8️⃣ **Find Most Frequent Element in a List**

```python
from collections import Counter
most_frequent = max(Counter(arr), key=Counter(arr).get)
```

9️⃣ **Reverse Words in a Sentence**

```python
reversed_words = ' '.join(sentence.split()[::-1])
```

🔟 **Find the Longest Word in a Sentence**

```python
longest_word = max(sentence.split(), key=len)
```

---

### **Bitwise Tricks**

1️⃣1️⃣ **Check If a Number Has an Odd Number of Set Bits (Parity Check)**

```python
is_odd_parity = lambda n: bin(n).count('1') % 2 == 1
```

1️⃣2️⃣ **Multiply a Number by 2 Using Bitwise Shift**

```python
double = lambda n: n << 1
```

1️⃣3️⃣ **Divide a Number by 2 Using Bitwise Shift**

```python
half = lambda n: n >> 1
```

1️⃣4️⃣ **Convert Lowercase to Uppercase Without `upper()`**

```python
to_upper = lambda c: chr(ord(c) & ~32)
```

1️⃣5️⃣ **Convert Uppercase to Lowercase Without `lower()`**

```python
to_lower = lambda c: chr(ord(c) | 32)
```

---

### **Data Structure Hacks**

1️⃣6️⃣ **Find Top K Frequent Elements in a List**

```python
from collections import Counter
from heapq import nlargest
top_k = lambda nums, k: [x for x, _ in nlargest(k, Counter(nums).items(), key=lambda x: x[1])]
```

1️⃣7️⃣ **Find All Subsets of a List**

```python
from itertools import chain, combinations
subsets = lambda arr: list(chain.from_iterable(combinations(arr, r) for r in range(len(arr) + 1)))
```

1️⃣8️⃣ **Generate All Permutations of a List**

```python
from itertools import permutations
all_permutations = list(permutations(arr))
```

1️⃣9️⃣ **Check If a List is Sorted**

```python
is_sorted = lambda arr: all(arr[i] <= arr[i+1] for i in range(len(arr) - 1))
```

2️⃣0️⃣ **Find Intersection of Two Lists in One Line**

```python
intersection = list(set(list1) & set(list2))
```

---

