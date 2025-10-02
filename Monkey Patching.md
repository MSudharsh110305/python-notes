# Monkey patching

**Definition**  
Monkey patching is the practice of changing or extending code at runtime by modifying modules, classes, or objects after they have been defined. In Python, because everything is an object and names are mutable, you can replace functions, methods, or attributes on the fly.

---

## Why it's possible in Python
Python uses **names bound to objects**. You can rebind those names at runtime:

```python
import some_module
some_module.func = my_new_func
```

Because of that mutability, you can alter behavior without changing the original source files.

---

## Common use cases
- **Testing**: Replace slow I/O or external-service calls with fakes/stubs.
- **Hotfixes**: Patch behavior on a running system (rare, risky).
- **Plugins/extension systems**: Dynamically add features to existing classes.
- **Prototyping / quick hacks**: Try new behavior without editing library code.

---

## Simple examples

### 1. Replace a function in a module
```python
# mylib.py
def greet():
    return "hello"

# app.py
import mylib

def new_greet():
    return "patched hello"

mylib.greet = new_greet
print(mylib.greet())  # "patched hello"
```

### 2. Patch a class method (affects all instances)
```python
class A:
    def hello(self):
        return "original"

def new_hello(self):
    return "patched"

A.hello = new_hello

a = A()
print(a.hello())  # "patched"
```

### 3. Patch a single instance method (bind the function)
When binding a function to an instance you must make it a bound method:

```python
import types

class A:
    def hello(self):
        return "original"

def new_hello(self):
    return "patched for one instance"

a = A()
a.hello = types.MethodType(new_hello, a)
print(a.hello())  # "patched for one instance"

b = A()
print(b.hello())  # "original"
```

---

## Using standard tools for temporary patching

### `unittest.mock.patch`
Built-in, safe way to patch during tests or in a `with` block:

```python
from unittest.mock import patch

# patch object attribute for the duration of the context
with patch('mymodule.fetch_data') as mock_fetch:
    mock_fetch.return_value = {"ok": True}
    # code that calls mymodule.fetch_data() sees the mock

# after the with-block, original function is restored
```

You can also use the `@patch` decorator on test functions.

### `pytest` `monkeypatch` fixture
Pytest provides `monkeypatch` for tests:

```python
def test_something(monkeypatch):
    monkeypatch.setattr(mymodule, 'fetch_data', lambda: {"ok": True})
    # test sees patched fetch_data
```

---

## Pitfalls & dangers
- **Hard to reason about**: Behavior changes are global and can surprise other code.
- **Maintenance burden**: Future readers may not expect runtime edits.
- **Order/timing issues**: Patch must occur before code reads the name; importing modules can lock in the original.
- **Breaks encapsulation**: You may depend on private internals of a library.
- **Thread-safety**: Patching global names in multi-threaded programs can be unsafe.
- **Security**: If untrusted code can monkey patch, it can alter behavior maliciously.
- **Tooling/IDE confusion**: Static analysis and refactors may not catch runtime changes.

---

## Best practices
1. **Prefer local / temporary patching** (use `with patch(...)` or test fixtures).
2. **Preserve originals** if doing manual patching:
   ```python
   orig = module.func
   module.func = my_func
   # ... later
   module.func = orig
   ```
3. **Document** the patch clearly and limit its scope.
4. **Avoid in production** unless you have a very compelling reason (hotfix during emergency).
5. **Use dependency injection** (pass in functions/objects) rather than patching when designing code.
6. **Prefer subclassing/adapter/composition** over monkey patching for long-term changes.
7. **Use standard test-mocking tools** (`unittest.mock`, `pytest.monkeypatch`) rather than hand-rolled patches.

---

## Alternatives to monkey patching
- **Dependency injection**: Pass dependencies as parameters so tests can substitute mocks.
- **Subclassing / composition**: Extend behavior in a controlled way.
- **Decorator / wrapper**: Wrap functions to add behavior without replacing them.
- **Plugin architecture**: Let extensions register behavior in a controlled API.

---