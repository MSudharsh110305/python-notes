> [!note]  
> Duck typing: “If it looks like a duck and quacks like a duck, it’s a duck” — emphasize interfaces (methods/attributes) over concrete types.

> [!note]  
> EAFP: “Easier to Ask Forgiveness than Permission” — try the operation and catch exceptions if the assumption fails; contrasts with LBYL (“Look Before You Leap”)

## Core concepts

- Duck typing bases correctness on whether an object provides the needed methods/attributes, avoiding explicit type checks like isinstance and type wherever possible.
- EAFP assumes required operations will succeed and uses try/except to recover, resulting in concise, fast code when exceptions are rare.
- LBYL explicitly probes preconditions (hasattr, in, len checks), which can be verbose and can introduce race conditions between the check and the use.
## Why duck typing

- Flexibility: independent classes that expose the same interface can be used interchangeably without inheritance or common base classes.
- Decoupling: caller code depends only on behavior (methods/attributes), which fosters polymorphic substitution and easier testing/mocking.
- Complement with ABCs: abstract base classes can document and enforce interfaces where necessary without abandoning duck typing.

---

## EAFP vs LBYL

- EAFP pattern:
```python
try:
    thing.quack(); thing.fly()
except AttributeError:
    handle()

```
- This is idiomatic in Python and avoids preflight checks for every attribute.
- LBYL pattern:
```python
if hasattr(thing, "quack") and callable(thing.quack):
    thing.quack()

```
- This grows unwieldy as checks accumulate and can still fail under concurrent mutation.
- In multi-threaded contexts, LBYL can race: e.g., checking membership and then indexing can fail if the mapping changes between steps; EAFP or proper locking avoids this.
## Examples: behavior over type

- Interchangeable classes:
```python
class Duck: 
    def quack(self): ...
    def fly(self): ...

class Person:
    def quack(self): ...
    def fly(self): ...

def quack_and_fly(obj):
    obj.quack(); obj.fly()
```
- This works without checking obj’s type because both provide the required methods.
- Birds example (independent types, shared interface):
```python
class Duck: 
    def swim(self): ...
    def fly(self): ...
class Swan:
    def swim(self): ...
    def fly(self): ...

```
- Iterating and calling fly/swim on each instance demonstrates duck typing flexibility.
## EAFP patterns in practice

- Attribute access:
```
try:
    thing.quack()
except AttributeError:
    recover()

```
- Prefer this to hasattr + callable checks unless failure is expected and frequent.
- Mapping access:
```
try:
    print(person["name"], person["age"], person["job"])
except KeyError as e:
    print("missing key:", e)

```
- Avoid pre-checks like "key in dict" for each key to reduce verbosity and races.
- Indexing sequences:
```
try:
    value = items[5]
except IndexError:
    handle()

```
- Prefer to length checks when intermediate modifications or readability concerns exist.

## Performance and readability

- EAFP can be faster when success is common because each operation is attempted once instead of being preceded by multiple probes.
- Many find EAFP-based code more readable because it expresses intent directly and centralizes error handling via exceptions.

---

## Concurrency and race conditions

- LBYL can fail under concurrency due to time between “look” and “leap,” e.g., if key in mapping followed by mapping[key] can race if another thread mutates the mapping.
- EAFP avoids the window by performing the operation and handling exceptions atomically from the caller’s perspective, or by guarding with locks if needed.

---

## When to complement duck typing

- Use ABCs or protocols (structural typing with typing.Protocol) to document/validate required interfaces when libraries need explicit contracts or when static checking helps.
- Use hasattr sparingly when exceptions would be routine control flow (e.g., optional attribute probes) or when probing is cheaper than failing.

---

## Anti-patterns and caveats

- Overusing isinstance/type checks undermines polymorphism and reduces flexibility; reserve for cases where strict type semantics matter (e.g., numeric tower distinctions).
- Catching broad exceptions can hide bugs; prefer specific exceptions (AttributeError, KeyError, IndexError, OSError) and minimize try-block scope.
- EAFP is not a mandate: choose LBYL when a check is semantically necessary or when failure would be common and expensive to handle via exceptions.