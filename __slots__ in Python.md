## What is `__slots__`?
In Python, classes typically use a special internal dictionary to store instance attributes. However, by defining `__slots__` in a class, you can **prevent the creation of this dictionary** and instead define a fixed set of attributes. This can help reduce memory usage and improve performance, especially when you have many instances of the class.

### Why Use `__slots__`?
1. **Memory Efficiency**: Python usually stores attributes in a dictionary. With `__slots__`, the class uses a more memory-efficient internal structure.
2. **Faster Access**: Accessing attributes can be quicker because the dictionary lookup is bypassed.
3. **Enforcing Structure**: It enforces that only a predefined set of attributes are allowed, preventing errors like adding unexpected attributes.

## How to Use `__slots__`
You define `__slots__` as a list of attribute names, like so:

```python
class Point:
    __slots__ = ['x', 'y']  # Only x and y are allowed

    def __init__(self, x, y):
        self.x = x
        self.y = y
```

Now, if you try to add an attribute not listed in `__slots__`, you'll get an `AttributeError`.

### Example:
```python
p = Point(3, 4)
print(p.x)  # Output: 3
print(p.y)  # Output: 4

# This will raise an error because 'z' is not in __slots__
p.z = 5  # AttributeError: 'Point' object has no attribute 'z'
```

## Comparison: With and Without `__slots__`

### Without `__slots__` (Normal Class)
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(3, 4)
p.z = 5  # Works fine! You can add new attributes anytime.
```

### With `__slots__`
```python
class Point:
    __slots__ = ['x', 'y']  # Only x and y are allowed

    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(3, 4)
p.z = 5  # Raises an error: AttributeError: 'Point' object has no attribute 'z'
```

## Memory Efficiency
Using `__slots__` can save memory. Normally, Python stores object attributes in a dictionary. With `__slots__`, it uses a more compact internal structure, reducing the memory footprint.

```python
import sys

class PointWithoutSlots:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class PointWithSlots:
    __slots__ = ['x', 'y']

    def __init__(self, x, y):
        self.x = x
        self.y = y

# Create instances
p1 = PointWithoutSlots(3, 4)
p2 = PointWithSlots(3, 4)

print(sys.getsizeof(p1))  # Memory size of object without __slots__
print(sys.getsizeof(p2))  # Memory size of object with __slots__
```

## Key Points About `__slots__`

1. **No Dynamic Attributes**: You can’t add new attributes to an object after it’s created if you use `__slots__`. This ensures that only the defined attributes are allowed.
   
2. **Inheritance**: If a base class defines `__slots__`, subclasses can either:
   - Use the same slots (if they don’t define their own).
   - Define their own set of slots for additional attributes.

Example:
```python
class Base:
    __slots__ = ['x']

class Derived(Base):
    __slots__ = ['y']  # Only 'x' and 'y' are allowed in Derived class

d = Derived()
d.x = 10
d.y = 20
# d.z = 30  # Raises error: AttributeError: 'Derived' object has no attribute 'z'
```

3. **Accessing Attributes**: Python uses a **memory view** to access the attributes rather than a dictionary lookup, which makes it faster but less flexible.

4. **Using `__dict__` with `__slots__`**: The `__dict__` attribute (which stores instance attributes) won’t exist unless you explicitly add `__dict__` to `__slots__`.

```python
class Point:
    __slots__ = ['x', 'y', '__dict__']  # Allows using __dict__ too

p = Point(3, 4)
print(p.__dict__)  # This will now work, even with __slots__
```
