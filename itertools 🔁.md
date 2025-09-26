
The `itertools` module provides **memory-efficient iterators** that help in looping, data manipulation, and sequence processing. It includes functions for **infinite iteration, filtering, grouping, and combinatorial operations.**

### **1. `count(start=0, step=1)`**

- **Description:** Generates an **infinite sequence** of numbers, starting from `start` and incrementing by `step`.
- **Use Case:** Used for **indexing** or creating infinite sequences.
- **Example:**
    
    ```python
    import itertools
    for x in itertools.count(10, 2):
        print(x)
        if x > 20:  # Stop manually
            break
    ```
    
    **Output:** `10, 12, 14, 16, 18, 20`

---

### **2. `cycle(iterable)`**

- **Description:** **Repeats an iterable indefinitely**.
- **Use Case:** Useful for **cycling through values** in a loop.
- **Example:**
    
    ```python
    import itertools
    counter = 0
    for x in itertools.cycle(['A', 'B', 'C']):
        print(x)
        counter += 1
        if counter == 6:
            break
    ```
    
    **Output:** `A, B, C, A, B, C`

---

### **3. `repeat(object, times=None)`**

- **Description:** Repeats `object` indefinitely (or `times` times if specified).
- **Use Case:** Useful for **creating constant sequences**.
- **Example:**
    
    ```python
    import itertools
    for x in itertools.repeat('Hello', 3):
        print(x)
    ```
    
    **Output:** `'Hello', 'Hello', 'Hello'`

---

### **4. `chain(*iterables)`**

- **Description:** Combines multiple iterables into a **single continuous sequence**.
- **Use Case:** Helps in **iterating over multiple lists without merging** them.
- **Example:**
    
    ```python
    import itertools
    a = [1, 2]
    b = [3, 4]
    result = itertools.chain(a, b)
    print(list(result))
    ```
    
    **Output:** `[1, 2, 3, 4]`

---

### **5. `zip_longest(*iterables, fillvalue=None)`**

- **Description:** Similar to `zip()`, but fills missing values with `fillvalue`.
- **Use Case:** Merging lists of **unequal lengths**.
- **Example:**
    
    ```python
    import itertools
    a = [1, 2, 3]
    b = ['a', 'b']
    result = itertools.zip_longest(a, b, fillvalue='X')
    print(list(result))
    ```
    
    **Output:** `[(1, 'a'), (2, 'b'), (3, 'X')]`

---

### **6. `combinations(iterable, r)`**

- **Description:** Returns all **unique** combinations of length `r` from `iterable`.
- **Use Case:** Used in **probability, data analysis**.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.combinations([1, 2, 3], 2)))
    ```
    
    **Output:** `[(1, 2), (1, 3), (2, 3)]`

---

### **7. `combinations_with_replacement(iterable, r)`**

- **Description:** Like `combinations()`, but allows **repeated elements**.
- **Use Case:** Used in **lottery-style selections**.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.combinations_with_replacement([1, 2, 3], 2)))
    ```
    
    **Output:** `[(1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)]`

---

### **8. `permutations(iterable, r=None)`**

- **Description:** Returns all **possible orderings** of length `r`.
- **Use Case:** Used for **password cracking, game logic**.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.permutations([1, 2, 3], 2)))
    ```
    
    **Output:** `[(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]`

---

### **9. `product(*iterables, repeat=1)`**

- **Description:** Returns the **Cartesian product** (cross product) of input iterables.
- **Use Case:** Used in **machine learning hyperparameter tuning**.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.product([1, 2], ['a', 'b'])))
    ```
    
    **Output:** `[(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]`

---

### **10. `starmap(function, iterable)`**

- **Description:** Like `map()`, but expects **tuples** as input.
- **Use Case:** Used when function needs **multiple parameters**.
- **Example:**
    
    ```python
    import itertools
    import math
    a = [(2, 5), (3, 3), (4, 2)]
    print(list(itertools.starmap(math.pow, a)))
    ```
    
    **Output:** `[32.0, 27.0, 16.0]`

---

### **11. `filterfalse(function, iterable)`**

- **Description:** Opposite of `filter()`, returns **items where function is `False`**.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.filterfalse(lambda x: x % 2 == 0, range(6))))
    ```
    
    **Output:** `[1, 3, 5]`

---

### **12. `islice(iterable, start, stop, step)`**

- **Description:** Returns a **slice** of the iterable.
- **Use Case:** Useful when working with **large datasets**.
- **Example:**
    
    ```python
    import itertools
    a = range(10)
    print(list(itertools.islice(a, 2, 8, 2)))
    ```
    
    **Output:** `[2, 4, 6]`

---

### **13. `accumulate(iterable, func=operator.add)`**

- **Description:** Returns an **accumulated sum** or any binary function.
- **Use Case:** Used in **running totals**.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.accumulate([1, 2, 3, 4])))
    ```
    
    **Output:** `[1, 3, 6, 10]`

---

### **14. `dropwhile(predicate, iterable)`**

- **Description:** Drops elements **while condition is True**, then yields the rest.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.dropwhile(lambda x: x < 3, [1, 2, 3, 4])))
    ```
    
    **Output:** `[3, 4]`

---

### **15. `takewhile(predicate, iterable)`**

- **Description:** Takes elements **while condition is True**.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.takewhile(lambda x: x < 3, [1, 2, 3, 4])))
    ```
    
    **Output:** `[1, 2]`

---

### **16. `pairwise(iterable)` (Python 3.10+)**

- **Description:** Returns consecutive pairs of elements.
- **Example:**
    
    ```python
    import itertools
    print(list(itertools.pairwise([1, 2, 3, 4])))
    ```
    
    **Output:** `[(1, 2), (2, 3), (3, 4)]`

---

## **17. `groupby(iterable, key=None)`**

- **Description:** Groups elements based on a key function.
- **Use Case:** Useful for **categorizing data**.
- **Example:**
    
    ```python
    import itertools
    
    data = [('A', 1), ('A', 2), ('B', 3), ('B', 4), ('C', 5)]
    grouped = itertools.groupby(data, key=lambda x: x[0])
    
    for key, group in grouped:
        print(key, list(group))
    ```
    
    **Output:**
    
    ```
    A [('A', 1), ('A', 2)]
    B [('B', 3), ('B', 4)]
    C [('C', 5)]
    ```
    

---

## **18. `tee(iterable, n=2)`**

- **Description:** Creates **multiple independent iterators** from a single iterable.
- **Use Case:** Used when an iterable **needs to be used multiple times**.
- **Example:**
    
    ```python
    import itertools
    
    it1, it2 = itertools.tee([1, 2, 3], 2)
    print(list(it1))  # First iterator
    print(list(it2))  # Second iterator
    ```
    
    **Output:**
    
    ```
    [1, 2, 3]
    [1, 2, 3]
    ```
    

---

## **19. `batched(iterable, n)` (Python 3.12+)**

- **Description:** Returns **chunks** of `n` elements from an iterable.
- **Use Case:** Used for **batch processing**.
- **Example:**
    
    ```python
    import itertools
    
    result = itertools.batched(range(10), 3)
    print(list(result))
    ```
    
    **Output:** `[(0, 1, 2), (3, 4, 5), (6, 7, 8), (9,)]`

---

## **20. `compress(data, selectors)`**

- **Description:** Filters `data` based on `selectors` (a list of `True/False` values).
- **Use Case:** Used for **conditional filtering**.
- **Example:**
    
    ```python
    import itertools
    
    data = ['A', 'B', 'C', 'D']
    selectors = [1, 0, 1, 0]  # Only 'A' and 'C' will be selected
    print(list(itertools.compress(data, selectors)))
    ```
    
    **Output:** `['A', 'C']`

---

## **21. `permutations(iterable, r=None)`**

- **Description:** Returns **all possible ordered arrangements** of length `r`.
- **Example:**
    
    ```python
    import itertools
    
    print(list(itertools.permutations('ABC', 2)))
    ```
    
    **Output:** `[('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]`

---

## **22. `combinations(iterable, r)`**

- **Description:** Returns **all possible selections** of `r` elements (order doesn't matter).
- **Example:**
    
    ```python
    import itertools
    
    print(list(itertools.combinations('ABC', 2)))
    ```
    
    **Output:** `[('A', 'B'), ('A', 'C'), ('B', 'C')]`

---

## **25. `islice(iterable, start, stop, step)`**

- **Description:** Returns a **slice** of the iterable.
- **Example:**
    
    ```python
    import itertools
    
    a = range(10)
    print(list(itertools.islice(a, 2, 8, 2)))
    ```
    
    **Output:** `[2, 4, 6]`

---

### 🔥 **Bonus Use Cases**

#### **🔹 Generate Infinite Prime Numbers**

```python
import itertools

def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

primes = filter(is_prime, itertools.count(2))
print(list(itertools.islice(primes, 10)))  # First 10 primes
```

**Output:** `[2, 3, 5, 7, 11, 13, 17, 19, 23, 29]`

---
