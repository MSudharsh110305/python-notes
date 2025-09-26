## Introduction & Basics
### What are Dataclasses?

Dataclasses are Python classes decorated with `@dataclass` that automatically generate common special methods like `__init__`, `__repr__`, and `__eq__`. They reduce boilerplate code and make classes primarily used for storing data more concise and readable.
``` python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    height: float = 0.0  # default value

# Usage
person = Person("Alice", 30, 5.6)
print(person)  # Person(name='Alice', age=30, height=5.6)
```
### Key Benefits

- Eliminates boilerplate `__init__`, `__repr__`, `__eq__` methods
- Type hints provide documentation and IDE support
- Reduces errors from manual implementation
- Consistent behavior across classes
---
## @dataclass Decorator & Parameters

### Full Signature
```python
@dataclass(
    init=True,        # Generate __init__?
    repr=True,        # Generate __repr__?
    eq=True,          # Generate __eq__?
    order=False,      # Generate ordering methods?
    unsafe_hash=False,# Force __hash__ generation?
    frozen=False,     # Make instances immutable?
    match_args=True,  # Generate __match_args__? (3.10+)
    kw_only=False,    # All fields keyword-only? (3.10+)
    slots=False,      # Add __slots__? (3.10+)
    weakref_slot=False # Add weakref slot? (3.11+)
)
class MyClass:
    pass

```
### Parameter Details
#### `init=True`
Controls `__init__` method generation:
```python
@dataclass(init=False)
class Point:
    x: int
    y: int
    
    def __init__(self, coords):
        self.x, self.y = coords
```
#### `repr=True`
Controls `__repr__` method generation:
```python
@dataclass(repr=False)
class Secret:
    value: str
    
# Won't have automatic string representation
```
#### `eq=True`
Controls `__repr__` method generation:
```python
@dataclass(repr=False)
class Secret:
    value: str
    
# Won't have automatic string representation
```
#### `eq=True`
Controls `__eq__` method generation:
```python
@dataclass
class Point:
    x: int
    y: int

p1 = Point(1, 2)
p2 = Point(1, 2)
print(p1 == p2)  # True - compares all fields
```
#### `order=False`
Generate comparison methods (`__lt__`, `__le__`, `__gt__`, `__ge__`):
```python
@dataclass(order=True)
class Student:
    grade: int
    name: str

s1 = Student(90, "Alice")
s2 = Student(85, "Bob")
print(s1 > s2)  # True - compares by fields in order
```
#### `frozen=False`
Make instances immutable:
```python
@dataclass(frozen=True)
class Point:
    x: int
    y: int

p = Point(1, 2)
# p.x = 3  # Raises FrozenInstanceError
```
#### `kw_only=False` (Python 3.10+)
Make all fields keyword-only:
```python
@dataclass(kw_only=True)
class Person:
    name: str
    age: int

# Must use: Person(name="Alice", age=30)
# Not: Person("Alice", 30)
```
#### `slots=False` (Python 3.10+)
Add `__slots__` for memory optimization:
```python
@dataclass(slots=True)
class Point:
    x: int
    y: int

# Uses less memory, prevents dynamic attributes
```
---
## Fields & field() Function

### Basic Field Annotations
```python
@dataclass
class Product:
    name: str                    # Required field
    price: float                 # Required field
    quantity: int = 0            # Optional with default
    tags: list[str] = None      # Don't do this! (mutable default)
```
### Using field() Function
```python
from dataclasses import dataclass, field

@dataclass
class Product:
    name: str
    price: float
    quantity: int = field(default=0, repr=False)
    tags: list[str] = field(default_factory=list)
    metadata: dict = field(default_factory=dict, compare=False)
```
### field() Parameters
```python
field(
    default=MISSING,           # Default value
    default_factory=MISSING,   # Function to create default
    init=True,                # Include in __init__?
    repr=True,                # Include in __repr__?
    compare=True,             # Include in comparisons?
    hash=None,                # Include in __hash__?
    metadata=None,            # Arbitrary metadata dict
    kw_only=MISSING           # Force keyword-only? (3.10+)
)
```
### Field Examples
```python
@dataclass
class User:
    # Basic fields
    username: str
    email: str
    
    # Field with default
    active: bool = True
    
    # Field excluded from repr
    password: str = field(repr=False)
    
    # Field excluded from comparison
    last_login: datetime = field(compare=False, default=None)
    
    # Field not in __init__, computed later
    full_name: str = field(init=False)
    
    # Mutable default using factory
    permissions: list[str] = field(default_factory=list)
    
    # Field with metadata
    age: int = field(metadata={"validation": "positive"})
    
    def __post_init__(self):
        self.full_name = f"{self.first_name} {self.last_name}"
```
---
## Default Values & Factories
### Simple Defaults
```python
@dataclass
class Config:
    debug: bool = False
    port: int = 8080
    host: str = "localhost"
```
### Mutable Defaults (WRONG WAY)
```python
@dataclass
class BadExample:
    items: list = []  # DON'T DO THIS! All instances share same list
```
### Mutable Defaults (CORRECT WAY)
```python
@dataclass
class GoodExample:
    items: list = field(default_factory=list)
    settings: dict = field(default_factory=dict)
    timestamp: datetime = field(default_factory=datetime.now)

```
### Custom Default Factories
```python
def create_uuid():
    return str(uuid.uuid4())

@dataclass
class Record:
    id: str = field(default_factory=create_uuid)
    data: dict = field(default_factory=lambda: {"version": 1})
```
### Complex Default Factory Examples
```python
@dataclass
class DatabaseConfig:
    host: str = "localhost"
    port: int = 5432
    credentials: dict = field(
        default_factory=lambda: {
            "user": os.getenv("DB_USER", "admin"),
            "password": os.getenv("DB_PASS", "")
        }
    )
```
---
## Magic Methods Generated
### **init** Method
```python
@dataclass
class Point:
    x: int
    y: int
    z: int = 0

# Generated __init__:
# def __init__(self, x: int, y: int, z: int = 0):
#     self.x = x
#     self.y = y  
#     self.z = z
```
### **repr** Method
```python
@dataclass
class Person:
    name: str
    age: int

p = Person("Alice", 30)
print(repr(p))  # Person(name='Alice', age=30)
```
### **eq** Method
```python
@dataclass
class Point:
    x: int
    y: int

p1 = Point(1, 2)
p2 = Point(1, 2)
print(p1 == p2)  # True - compares all fields
```
### **hash** Method (with conditions)
```python
# Hash only generated if eq=True and frozen=True
@dataclass(frozen=True)
class Point:
    x: int
    y: int

p = Point(1, 2)
print(hash(p))  # Now hashable, can be used in sets/dict keys
```
### Ordering Methods (order=True)
```python
@dataclass(order=True)
class Student:
    grade: int
    name: str

s1 = Student(90, "Alice")
s2 = Student(85, "Bob")
print(s1 > s2)   # True
print(s1 <= s2)  # False
```
---
## Type Annotations & Special Types
### Basic Type Hints
```python
from typing import Optional, List, Dict, Union

@dataclass
class Product:
    name: str
    price: float
    quantity: int
    tags: List[str]
    metadata: Dict[str, str]
    discount: Optional[float] = None
```
### ClassVar - Class Variables
```python
from typing import ClassVar

@dataclass
class Employee:
    # Class variable - ignored by dataclass
    company_name: ClassVar[str] = "TechCorp"
    
    # Instance fields
    name: str
    salary: int

print(Employee.company_name)  # "TechCorp"
e = Employee("Alice", 50000)
# company_name not in __init__, __repr__, etc.
```
### InitVar - Initialization-Only Variables
```python
from dataclasses import InitVar

@dataclass
class User:
    name: str
    normalized_name: str = field(init=False)
    _raw_name: InitVar[str] = None
    
    def __post_init__(self, _raw_name):
        if _raw_name:
            self.name = _raw_name
        self.normalized_name = self.name.lower().strip()

# Usage
user = User(name="Alice", _raw_name="  ALICE  ")
print(user.normalized_name)  # "alice"
```
### KW_ONLY - Keyword-Only Fields (Python 3.10+)
```python
from dataclasses import KW_ONLY

@dataclass
class Point3D:
    x: float
    y: float
    _: KW_ONLY  # Everything after this is keyword-only
    z: float
    color: str = "red"

# Usage
p = Point3D(1.0, 2.0, z=3.0, color="blue")  # z and color must be keywords
```
---
## Post-Init Processing
### Basic **post_init**
```python
@dataclass
class Rectangle:
    width: float
    height: float
    area: float = field(init=False)
    
    def __post_init__(self):
        self.area = self.width * self.height
```
### With InitVar Parameters
```python
@dataclass
class User:
    first_name: str
    last_name: str
    full_name: str = field(init=False)
    display_name: str = field(init=False)
    
    # InitVar fields passed to __post_init__
    title: InitVar[str] = None
    
    def __post_init__(self, title):
        self.full_name = f"{self.first_name} {self.last_name}"
        if title:
            self.display_name = f"{title} {self.full_name}"
        else:
            self.display_name = self.full_name

user = User("John", "Doe", title="Dr.")
print(user.display_name)  # "Dr. John Doe"
```
### Validation in **post_init**
```python
@dataclass
class BankAccount:
    balance: float
    account_type: str
    
    def __post_init__(self):
        if self.balance < 0 and self.account_type != "credit":
            raise ValueError("Regular accounts cannot have negative balance")
        if self.account_type not in ["checking", "savings", "credit"]:
            raise ValueError("Invalid account type")
```
### Calling Parent **post_init**
```python
@dataclass
class Shape:
    name: str
    
    def __post_init__(self):
        print(f"Created shape: {self.name}")

@dataclass
class Rectangle(Shape):
    width: float
    height: float
    area: float = field(init=False)
    
    def __post_init__(self):
        super().__post_init__()  # Call parent's __post_init__
        self.area = self.width * self.height
```
---
## Inheritance
### Basic Inheritance
```python
@dataclass
class Animal:
    name: str
    species: str
    age: int = 0

@dataclass
class Dog(Animal):
    breed: str
    species: str = "Canis lupus"  # Override default

# Usage
dog = Dog(name="Buddy", breed="Golden Retriever", age=3)
print(dog)  # Dog(name='Buddy', species='Canis lupus', age=3, breed='Golden Retriever')
```
### Field Order in Inheritance
#### Fields from base classes come first, then child class fields:
```python
@dataclass
class Base:
    x: int = 1
    y: int = 2

@dataclass  
class Child(Base):
    z: int = 3
    x: int = 10  # Override base field

# Generated __init__(self, x: int = 10, y: int = 2, z: int = 3)
```
#### Inheritance with Non-Dataclass Base
```python
class Rectangle:
    def __init__(self, height, width):
        self.height = height
        self.width = width

@dataclass
class Square(Rectangle):
    side: float
    
    def __post_init__(self):
        # Must manually call parent __init__
        super().__init__(self.side, self.side)
```
#### Multiple Inheritance Challenges
```python
@dataclass
class Mixin1:
    field1: int
    
    def __post_init__(self):
        print("Mixin1 post_init")

@dataclass
class Mixin2:
    field2: str
    
    def __post_init__(self):
        print("Mixin2 post_init")

@dataclass
class Combined(Mixin1, Mixin2):
    field3: bool
    
    def __post_init__(self):
        # Only one __post_init__ is called by default!
        # Need to manually call others if needed
        super().__post_init__()
        print("Combined post_init")
```
#### kw_only for Safer Inheritance (Python 3.10+)
```python
@dataclass(kw_only=True)
class Animal:
    name: str
    age: int = 0

@dataclass(kw_only=True)
class Dog(Animal):
    breed: str

# All parameters are keyword-only, avoiding position issues
dog = Dog(name="Buddy", breed="Labrador", age=3)
```
---
## Utility Functions
### fields()
Get information about dataclass fields:
```python
from dataclasses import fields

@dataclass
class Person:
    name: str
    age: int = 0

field_info = fields(Person)
for field in field_info:
    print(f"{field.name}: {field.type}, default={field.default}")
```
### asdict()
Convert dataclass to dictionary:
```python
from dataclasses import asdict

@dataclass
class Point:
    x: int
    y: int

@dataclass
class Circle:
    center: Point
    radius: float

c = Circle(Point(1, 2), 5.0)
data = asdict(c)
print(data)  # {'center': {'x': 1, 'y': 2}, 'radius': 5.0}
```
### astuple()
 Convert dataclass to tuple:
```python
from dataclasses import astuple

point = Point(1, 2)
coords = astuple(point)
print(coords)  # (1, 2)
```
### replace()
Create a copy with modified fields:
```python
from dataclasses import replace

@dataclass(frozen=True)
class Person:
    name: str
    age: int

p1 = Person("Alice", 30)
p2 = replace(p1, age=31)  # Creates new instance
print(p2)  # Person(name='Alice', age=31)
```
### make_dataclass()
Dynamically create dataclasses:
```python
from dataclasses import make_dataclass, field

# Create a dataclass at runtime
Point = make_dataclass(
    'Point',
    [
        'x',                           # Just name, type defaults to Any
        ('y', int),                    # Name and type
        ('z', int, field(default=0))   # Name, type, and Field
    ]
)

p = Point(1, 2, z=3)
print(p)  # Point(x=1, y=2, z=3)
```
### is_dataclass()
Check if object is a dataclass:
```python
from dataclasses import is_dataclass

@dataclass
class MyClass:
    value: int

print(is_dataclass(MyClass))      # True (class)
print(is_dataclass(MyClass(1)))   # True (instance)

# Check for instance specifically
def is_dataclass_instance(obj):
    return is_dataclass(obj) and not isinstance(obj, type)
```
---
## Advanced Features

### Frozen Dataclasses (Immutable)
```python
@dataclass(frozen=True)
class ImmutablePoint:
    x: int
    y: int

p = ImmutablePoint(1, 2)
# p.x = 3  # Raises FrozenInstanceError

# Frozen objects can be hashed
point_set = {ImmutablePoint(1, 2), ImmutablePoint(3, 4)}
```
### Slots for Memory Optimization
```python
@dataclass(slots=True)
class EfficientPoint:
    x: float
    y: float

# Uses __slots__ internally for memory efficiency
# Prevents dynamic attribute addition
```
### Custom Equality and Hashing
```python
@dataclass(eq=False)  # Disable automatic __eq__
class Person:
    name: str
    age: int
    ssn: str
    
    def __eq__(self, other):
        # Custom equality based only on SSN
        if not isinstance(other, Person):
            return False
        return self.ssn == other.ssn
    
    def __hash__(self):
        return hash(self.ssn)
```
### Descriptors in Dataclasses
```python
class ValidatedAttribute:
    def __init__(self, validation_func):
        self.validation_func = validation_func
        
    def __set_name__(self, owner, name):
        self.name = f"_{name}"
        
    def __get__(self, obj, owner):
        if obj is None:
            return self
        return getattr(obj, self.name)
    
    def __set__(self, obj, value):
        if not self.validation_func(value):
            raise ValueError(f"Invalid value: {value}")
        setattr(obj, self.name, value)

@dataclass
class Person:
    age: int = ValidatedAttribute(lambda x: isinstance(x, int) and x >= 0)
```
---
