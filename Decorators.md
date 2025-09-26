## Core recap
- A decorator is a callable that takes a callable and returns a new callable; using @decorator above a definition is equivalent to name = decorator(name).
- Typical uses: logging, timing, caching, validation, retries, auth, rate limiting, and feature flags.
---
## Preserving metadata (functools.wraps)
- Problem: Wrappers hide original metadata like **name**, **doc**, and signature; tools like help(), introspection, and debuggers rely on them.
- Solution: Use functools.wraps(func) on the wrapper to copy metadata and attach **wrapped** pointing to the original.

``` python
import functools

def log_calls(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"calling {func.__name__} with {args=} {kwargs=}")
        return func(*args, **kwargs)
    return wrapper

@log_calls
def add(x, y):
    "Add two numbers."
    return x + y

print(add.__name__)      # 'add' (not 'wrapper')
print(add.__doc__)       # "Add two numbers."
print(add.__wrapped__)   # original function object

```
---
## Accessing the original function
- Via **wrapped**: When wraps is used, the decorated function exposes the original at decorated.**wrapped**.
```python
result_direct = add(2, 3)            # goes through the decorator
result_original = add.__wrapped__(2, 3)  # bypasses decorator

```
- Manual reference before decorating: Save a handle to the function, then decorate.
```python
def greet(name): return f"Hi {name}"

original_greet = greet  # keep original
def dec(f):
    def w(*a, **k): print("wrapped"); return f(*a, **k)
    return w

greet = dec(greet)  # now greet is wrapped
print(original_greet("Ada"))  # calls original
print(greet("Ada"))           # calls wrapped

```
- Storing the original inside the wrapper: Attach a custom attribute when returning the wrapper.
```python
def dec(f):
    def w(*a, **k): return f(*a, **k)
    w._original = f
    return w

@dec
def hello(): return "ok"

print(hello._original())  # access original via custom handle

```
---
## Using **wrapped**() carefully
- **wrapped** is intended for introspection and tooling, but it is callable and safely bypasses the wrapper; be consistent and document its use if code depends on bypassing decorators.
- If writing custom decorators, always apply functools.wraps so downstream users and tools can rely on **wrapped**.
---
## Stacking multiple decorators
- Multiple decorators can be layered; order matters. The bottom decorator applies first, then the one above wraps that result.
```python
def A(f):
    def w(*a, **k):
        print("A before"); r = f(*a, **k); print("A after"); return r
    return w

def B(f):
    def w(*a, **k):
        print("B before"); r = f(*a, **k); print("B after"); return r
    return w

@A
@B
def work():
    print("doing work")

# Equivalent to: work = A(B(work))
work()
# A before
# B before
# doing work
# B after
# A after

```

- Guideline: Put the most “inner” concern (closest to the original call) at the bottom. For example, caching near the function, then auth, then logging/timing on top.
---
## Parameterized decorators
- Add an outer layer to capture options; return the actual decorator that receives the function.
```python
import functools
def retry(times=3):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            last_exc = None
            for _ in range(times):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    last_exc = e
            raise last_exc
        return wrapper
    return decorator

@retry(times=5)
def flaky(): ...

```
---
## Returning values and transforming results
- Decorators can post-process return values or handle exceptions.
```python
import functools

def add_one(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        value = func(*args, **kwargs)
        return value + 1
    return wrapper

@add_one
def forty_one(): return 41

assert forty_one() == 42

```

- They can also convert exceptions to default values or structured results.
```python
def suppress(exc, default=None):
    def decorator(func):
        def wrapper(*args, **kwargs):
            try:
                return func(*args, **kwargs)
            except exc:
                return default
        return wrapper
    return decorator

@suppress((KeyError, IndexError), default=None)
def get_item(d, k): return d[k]

```
---
## args and kwargs forwarding
- Use *args and **kwargs in wrappers so the decorator is generic and works for any function signature.
```python
def timeit(func):
    import time, functools
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        t0 = time.perf_counter()
        try:
            return func(*args, **kwargs)
        finally:
            dt = time.perf_counter() - t0
            print(f"{func.__name__} took {dt:.6f}s")
    return wrapper

```
## Class and method decoration
- Methods: The wrapper’s first parameter must accept self or cls as appropriate.
```python
import functools
def announce(func):
    @functools.wraps(func)
    def wrapper(self, *args, **kwargs):
        print(f"{self.__class__.__name__}.{func.__name__}")
        return func(self, *args, **kwargs)
    return wrapper

class Greeter:
    @announce
    def hello(self, name): return f"Hi {name}"

```

- Class decorators: Receive a class, modify or return it.
```python
import functools
def announce(func):
    @functools.wraps(func)
    def wrapper(self, *args, **kwargs):
        print(f"{self.__class__.__name__}.{func.__name__}")
        return func(self, *args, **kwargs)
    return wrapper

class Greeter:
    @announce
    def hello(self, name): return f"Hi {name}"

```
---
## Built-in decorators worth knowing
- functools.wraps: preserve metadata and set **wrapped**.
- functools.lru_cache: memoization with optional maxsize; use for pure functions.
- functools.singledispatch: type-based function overloading on first argument.
- property / setter / deleter: manage attributes declaratively in classes.
---
## Common pitfalls and tips
- Always return the wrapper from the decorator; forgetting to return breaks the binding.
- Prefer functools.wraps to preserve name, docstring, module, annotations, and provide **wrapped**.
- Be cautious when altering signatures; if needed, use inspect.signature to map names to values robustly.
- For stacked decorators, test order explicitly; reordering can change semantics (e.g., cache before/after auth).
- If a decorator is meant for a specific signature, document it clearly; otherwise, keep it generic with \*args \*\*kwargs.
---
## Quick reference patterns
- Logging/tracing
```
import functools
def log(func):
    @functools.wraps(func)
    def w(*a, **k):
        print(f"{func.__name__}{a}{k}")
        return func(*a, **k)
    return w

```
- Caching
```
import functools
@functools.lru_cache(maxsize=256)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)

```
- Validation
```
import functools
def require_positive_first_arg(func):
    @functools.wraps(func)
    def w(*a, **k):
        x = a[0] if a else k.get('x', None)
        if x is None or x <= 0:
            raise ValueError("first argument must be positive")
        return func(*a, **k)
    return w

```
- Bypassing wrapper when needed
```
# Assuming wraps was used
decorated_func.__wrapped__(...)

```
- Manual storing and later decoration
```
def f(...): ...
orig = f
f = some_decorator(f)
# use orig(...) to call original behavior

```

## Built-in decorators
### @staticmethod
- Can be called on the class itself or an instance of the class.
- Don’t have access to `self` or `cls` (no instance or class-specific data).
- Are often used for utility functions that are logically related to the class, but don't need instance data.
> whereas normal method needs instance to be accessed
```python
class MathOperations:
    
    @staticmethod
    def add(a, b):
        return a + b
    
    @staticmethod
    def multiply(a, b):
        return a * b

# Calling static methods without creating an instance
print(MathOperations.add(5, 3))        # Output: 8
print(MathOperations.multiply(4, 2))   # Output: 8

# You can also call static methods through an instance
math_instance = MathOperations()
print(math_instance.add(10, 20))      # Output: 30
print(math_instance.multiply(3, 3))   # Output: 9

```

### @property
- When you use `@property`, it makes the method act like an **attribute** (i.e., you don't need parentheses to access the value).
- If you don't use the `@property` decorator, you have to explicitly define methods to get or set values, and they will always behave like normal methods (i.e., requiring parentheses when you call them).
```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius
    
    @property
    def area(self):
        return 3.14 * (self._radius ** 2)

# Usage:
circle = Circle(5)

# Accessing the property
print(f"Radius: {circle.radius}")  # Calls the radius() method
print(f"Area: {circle.area}")  # Calls the area() method

```