**Overview**

**T-strings** (Template String Literals) are a new feature introduced in Python 3.14 via PEP 750. They are a generalization of f-strings that use a t prefix instead of f. Unlike f-strings that immediately evaluate to a str, t-strings evaluate to a Template object that preserves the structure of static text and interpolated expressions for custom processing.

**Key Differences from F-strings**

|   |   |   |
|---|---|---|
|Feature|F-strings|T-strings|
|**Prefix**|f"..."|t"..."|
|**Result Type**|str|string.templatelib.Template|
|**Evaluation**|Immediate string formation|Deferred rendering|
|**Structure**|Lost after evaluation|Preserved for inspection|
|**Use Case**|Direct string output|Custom string processing|

**Basic Syntax**

```python
# Import required for type annotations and manual construction  
from string.templatelib import Template, Interpolation  
  
# Basic t-string creation  
name = "Alice"  
age = 30  
template = t"Hello {name}, you are {age} years old"  
print(type(template))  # <class 'string.templatelib.Template'>  
```
  

**Template Object Structure**

The Template object has three main attributes:

**1. .strings - Static String Parts**

```python
name = "Bob"  
template = t"Hello {name}, welcome!"  
print(template.strings)  # ('Hello ', ', welcome!')  
  
# Empty strings are included  
response = "Hi"  
template2 = t"Ah! {response}{name}."  
print(template2.strings)  # ('Ah! ', '', '.')  
```
  

**2. .interpolations - Dynamic Parts**

```python
template = t"Hello {name}, you are {age} years old"  
print(template.interpolations)  
# (Interpolation('Bob', 'name', None, ''), Interpolation(30, 'age', None, ''))  
  
for interp in template.interpolations:  
    print(f"Value: {interp.value}")  
    print(f"Expression: {interp.expression}")  
    print(f"Conversion: {interp.conversion}")  
    print(f"Format Spec: {interp.format_spec}")  
```
  

**3. .values - Evaluated Values**

```python
template = t"Hello {name}, you are {age} years old"  
print(template.values)  # ('Bob', 30)  
```
  

**Interpolation Object Properties**

Each Interpolation object contains:
- **value**: The evaluated result of the expression
- **expression**: The original text of the Python expression
- **conversion**: Conversion flag (None, 's', 'r', or 'a')
- **format_spec**: Format specification string

```python
template = t"Value: {42:.2f!s}"  
interp = template.interpolations[0]  
print(f"Value: {interp.value}")        # 42  
print(f"Expression: {interp.expression}")  # '42'  
print(f"Conversion: {interp.conversion}")   # 's'  
print(f"Format Spec: {interp.format_spec}") # '.2f'  
  
```

**Template Iteration**

Templates support iteration, yielding non-empty strings and interpolations:

```python
name = "Alice"  
for item in t"Hello {name}!":  
    print(f"{item} -> {type(item)}")  
  
# Output:  
# Hello  -> <class 'str'>  
# Interpolation('Alice', 'name', None, '') -> <class 'string.templatelib.Interpolation'>  
# ! -> <class 'str'>  
```
  

**Note**: Empty strings are excluded from iteration but included in .strings.

**Building Strings from Templates**

**Basic String Reconstruction**

```python
def build_template(template: Template) -> str:  
    """Convert a template back to a string."""  
    values = []  
    for item in template:  
        if isinstance(item, str):  
            values.append(item)  
        else:  # Interpolation  
            values.append(str(item.value))  
    return "".join(values)  
  
name = "Bob"  
profession = "chef"  
template = t"Your name is {name} and you are a {profession}"  
result = build_template(template)  
print(result)  # "Your name is Bob and you are a chef"  
```
  

**Applying Conversions and Format Specs**
```python
from string.templatelib import convert  
  
def render_to_string(template: Template) -> str:  
    """Render template with conversions and format specs."""  
    values = []  
    for part in template:  
        if isinstance(part, str):  
            values.append(part)  
        else:  # Interpolation  
            # Apply conversion  
            converted = convert(part.value, conversion=part.conversion)  
             
            # Apply format spec if present  
            if part.format_spec:  
                formatted = format(converted, part.format_spec)  
            else:  
                formatted = str(converted)  
             
            values.append(formatted)  
    return "".join(values)  
  
product = "t-shirt"  
price = 10.0  
template = t"Product: {product}, Price: ${price:.2f}"  
result = render_to_string(template)  
print(result)  # "Product: t-shirt, Price: $10.00"  
```
  

**Advanced Processing Examples**

**1. Custom Processing Functions**

```python
def build_template_with_processor(template: Template, processor):  
    """Apply custom processing to interpolated values."""  
    values = []  
    for item in template:  
        if isinstance(item, str):  
            values.append(item)  
        else:  
            processed_value = processor(str(item.value))  
            values.append(processed_value)  
    return "".join(values)  
  
```
```
# URL encoding example  
import urllib.parse  
  
string_value = "This is an unescaped string"  
template = t"Website: myfantasticpage.com?query={string_value}"  
result = build_template_with_processor(template, urllib.parse.quote)  
print(result)  # URL-encoded output  
```
  

**2. HTML Escaping for Security**

```python
import html  
  
def safe_html_template(template: Template) -> str:  
    """Safely escape HTML in template interpolations."""  
    values = []  
    for item in template:  
        if isinstance(item, str):  
            values.append(item)  
        else:  
            escaped = html.escape(str(item.value))  
            values.append(escaped)  
    return "".join(values)  
  
user_input = "<script>alert('evil')</script>"  
template = t"<p>{user_input}</p>"  
safe_output = safe_html_template(template)  
print(safe_output)  # "<p><script>alert('evil')</script></p>"  
  

```
**3. Case Transformation**

```python
def lower_upper_template(template: Template) -> str:  
    """Render static parts lowercased and interpolations uppercased."""  
    parts = []  
    for item in template:  
        if isinstance(item, Interpolation):  
            parts.append(str(item.value).upper())  
        else:  
            parts.append(item.lower())  
    return "".join(parts)  
  
name = "world"  
result = lower_upper_template(t"HELLO {name}")  
print(result)  # "hello WORLD"  
  
```

**Manual Template Construction**

Templates can be created programmatically:

```python
from string.templatelib import Template, Interpolation  
  
# Manual construction  
cheese = 'Camembert'  
template = Template(  
    'Ah! We do have ',  
    Interpolation(cheese, 'cheese'),  
    '.'  
)  
print(list(template))  
# ['Ah! We do have ', Interpolation('Camembert', 'cheese', None, ''), '.']  
  
```

```python
# Multiple consecutive strings are concatenated  
template2 = Template('Hello ', 'World', '!')  
print(template2.strings)  # ('Hello World!',)  
```
  
```python
# Multiple consecutive interpolations get empty strings between them  
template3 = Template(  
    Interpolation('value1', 'expr1'),  
    Interpolation('value2', 'expr2')  
)  
print(template3.strings)  # ('', '', '')  
```
  

**Supported Syntax Features**

T-strings support the full f-string syntax:

**Expressions and Calculations**

```python
x = 5  
y = 3  
template = t"Result: {x + y}"  
print(template.values)  # (8,)  
```
  

**Conversions (!s, !r, !a)**

```python
value = "test"  
template = t"String: {value!s}, Repr: {value!r}, ASCII: {value!a}"  
for interp in template.interpolations:  
    print(f"Conversion: {interp.conversion}")  
# Conversion: s  
# Conversion: r   
# Conversion: a  
```
  

**Format Specifications**

```python
pi = 3.14159  
template = t"Pi: {pi:.2f}, Padded: {pi:08.3f}"  
for interp in template.interpolations:  
    print(f"Format spec: '{interp.format_spec}'")  
# Format spec: '.2f'  
# Format spec: '08.3f'  
```
  

**Nested Expressions**

```python
items = ['a', 'b', 'c']  
template = t"Length: {len(items)}, First: {items[0]}"  
```
  

**Pattern Matching Support**

```python
Interpolations support pattern matching:

template = t"{1.0 + 2.0:.2f}"  
interpolation = template.interpolations[0]  
  
match interpolation:  
    case Interpolation(value, expression, conversion, format_spec):  
        print(f"Value: {value}")        # 3.0  
        print(f"Expression: {expression}")  # '1.0 + 2.0'  
        print(f"Conversion: {conversion}")   # None  
        print(f"Format: {format_spec}")     # '.2f'  
  ```
**Use Cases and Applications**

**1. SQL Query Building (Safe)**

```python
def build_sql_query(template: Template) -> str:  
    """Build parameterized SQL queries safely."""  
    sql_parts = []  
    params = []  
     
    for item in template:  
        if isinstance(item, str):  
            sql_parts.append(item)  
        else:  
            sql_parts.append("?")  # Placeholder  
            params.append(item.value)  
     
    return "".join(sql_parts), params  
  
user_id = 123  
name = "Alice"  
template = t"SELECT * FROM users WHERE id = {user_id} AND name = {name}"  
query, params = build_sql_query(template)  
print(f"Query: {query}")    # "SELECT * FROM users WHERE id = ? AND name = ?"  
print(f"Params: {params}")  # [123, 'Alice']  
```
  

**2. Logging with Structured Data**

```python
def structured_log(template: Template, level: str = "INFO"):  
    """Create structured log entries."""  
    static_parts = " ".join(template.strings).strip()  
    dynamic_data = {  
        f"field_{i}": interp.value  
        for i, interp in enumerate(template.interpolations)  
    }  
     
    log_entry = {  
        "level": level,  
        "message_template": static_parts,  
        "data": dynamic_data,  
        "full_message": "".join(str(item) for item in template)  
    }  
    return log_entry  
  
user = "Alice"  
action = "login"  
timestamp = "2024-01-01T10:00:00"  
log = structured_log(t"User {user} performed {action} at {timestamp}")  
print(log)  
```
  

**3. Web Template Processing**

```python
def html_template(template: Template) -> str:  
    """Process HTML templates with automatic escaping."""  
    result = []  
    for item in template:  
        if isinstance(item, str):  
            result.append(item)  
        else:  
            # Auto-escape HTML  
            escaped = html.escape(str(item.value))  
            result.append(escaped)  
     
    return "".join(result)  
  
username = "<script>alert('xss')</script>"  
message = "Hello & welcome!"  
html_output = html_template(t"<div>User: {username}, Message: {message}</div>")  
```
  

**4. Internationalization (i18n)**

```python
def i18n_template(template: Template, translations: dict) -> str:  
    """Apply translations to static parts while preserving interpolations."""  
    result = []  
     
    for item in template:  
        if isinstance(item, str):  
            # Translate static text  
            translated = translations.get(item.strip(), item)  
            result.append(translated)  
        else:  
            # Keep interpolated values as-is  
            result.append(str(item.value))  
     
    return "".join(result)  
  
translations = {  
    "Hello": "Hola",  
    ", welcome!": ", ¡bienvenido!"  
}  
  
name = "Carlos"  
spanish = i18n_template(t"Hello {name}, welcome!", translations)  
print(spanish)  # "Hola Carlos, ¡bienvenido!"  
  
```

**Convert Helper Function**

The string.templatelib.convert() function applies f-string conversion semantics:

```python
from string.templatelib import convert  
  
obj = "test"  
print(convert(obj, conversion='s'))  # 'test' (str())  
print(convert(obj, conversion='r'))  # "'test'" (repr())  
print(convert(obj, conversion='a'))  # "'test'" (ascii())  
print(convert(obj, conversion=None)) # 'test' (unchanged)  
```
  

**Important Notes and Gotchas**

**1. Import Requirements**

```python
# For type annotations and manual construction  
from string.templatelib import Template, Interpolation  
  
# For helper functions  
from string.templatelib import convert  
```
  

**2. Difference from string.Template**

**DON'T CONFUSE** with the older string.Template class:

```python
# OLD string.Template (since Python 2.4)  
from string import Template  
old_template = Template('$name likes $food')  
result = old_template.substitute(name='Alice', food='pizza')  
  
# NEW t-strings (Python 3.14+)  
from string.templatelib import Template  
name = 'Alice'  
food = 'pizza'  
new_template = t'{name} likes {food}'  # Returns Template object  
```
  

**3. Raw String Support**

T-strings can be combined with raw strings:
```python
path = r"C:\Users\Alice"  
template = tr"Path: {path}"  # Raw t-string  
```
  

**4. No Combination with f-strings**

```python
# INVALID - cannot combine f and t prefixes  
# template = ft"Hello {name}"  # SyntaxError  
```
  

**5. Empty String Handling in Iteration**

```python
template = t"A{value1}{value2}B"  
print(template.strings)  # ('A', '', 'B')  
print(list(template))    # ['A', Interpolation(...), Interpolation(...), 'B']  
# Note: Empty string between interpolations is excluded from iteration  
```
  

**Performance Considerations**

1.      **Template Creation**: T-strings create Template objects, which have some overhead compared to direct string creation
2.     **Processing Cost**: Custom processing functions add computational cost
3.      **Memory Usage**: Template objects store more metadata than plain strings
4.     **Use When Needed**: Only use t-strings when you need the structural information; use f-strings for simple formatting

**Migration from F-strings**

```python
# Before (f-string)  
name = "Alice"  
greeting = f"Hello {name}!"  
  
# After (t-string with processing)  
def process_template(template: Template) -> str:  
    return "".join(str(item) for item in template)  
  
name = "Alice"  
greeting = process_template(t"Hello {name}!")  
  
``` 

