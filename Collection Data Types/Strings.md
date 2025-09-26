# Python Strings Cheatsheet

## Literals and basics
```
s1 = 'single'
s2 = "double"
s3 = '''multi
line'''
s4 = r'raw\path\win'        # raw string (no escape processing)
s5 = f'hello {2+3}'         # f-string
empty = ""                   # empty string
```

## Built-in operators on str
# length
```
n = len("hello")                 # 5
```

# indexing and slicing
```
s = "abcdef"
a = s[0]                         # 'a'
tail = s[1:]                     # 'bcdef'
mid = s[2:5]                     # 'cde'
rev = s[::-1]                    # 'fedcba'
```

# concatenation and repetition
```
t = "ab" + "cd"                  # 'abcd'
u = "ha" * 3                     # 'hahaha'
```

# membership
```
ok = "py" in "python"            # True
no = "z" not in "abc"            # True
```

# comparison (lexicographic by code point)
```
"abc" < "abd"                    # True
"abc" == "abc"                   # True
```

## Built-in functions commonly used with str
```
s = str(123)                     # '123'
r = repr("café")                 # "'café'"
t = format(3.14159, ".2f")       # '3.14'
c = chr(9731)                    # '☃'
o = ord('A')                     # 65
n = len("hello")                 # 5
tp = type("x")                   # <class 'str'>
```

## Encoding/decoding
```
b = "café".encode("utf-8")       # b'caf\xc3\xa9'
s = b.decode("utf-8")            # 'café'
```

## Formatting: f-strings, str.format, format()
```
name, x = "Ada", 3.14159
f1 = f"{name:^10s} π≈{x:.2f}"    # center name, 2 decimals
f2 = "{n:^10s} π≈{v:.2f}".format(n=name, v=x)
f3 = format(x, "08.3f")          # '0003.142'

```
## Percent-style formatting (printf-style)
```
"Hello, %s" % "Ada"                  
"%d %x %o" % (255, 255, 255)         
"%(name)s: %(val).2f" % {"name":"pi","val":3.14159}  
```

## Partitioning and splitting
```
s = "key=value=more"
a, sep, b = s.partition("=")     # ('key', '=', 'value=more')
a2, sep2, b2 = s.rpartition("=") # ('key=value', '=', 'more')

" a b  c ".split()               # ['a', 'b', 'c']
"1,2,,3".split(",")              # ['1', '2', '', '3']
"1,2,3".rsplit(",", 1)           # ['1,2', '3']
"line1\nline2".splitlines()      # ['line1', 'line2']
```

## Joining
```
items = ["a", "b", "c"]
",".join(items)                  # 'a,b,c'
"".join(["<li>", "x", "</li>"])  # '<li>x</li>'
```

## Search and count
```
t = "banana"
t.find("na")                     # 2
t.rfind("na")                    # 4
t.index("na")                    # 2
t.rindex("na")                   # 4
t.count("na")                    # 2
t.startswith(("ba", "ca"))       # True
t.endswith("na")                 # True

```
## Replace and translate
```
"banana".replace("na", "NA")             # 'baNANA'
tbl = str.maketrans({"a": "4", "e": "3"})
"peace".translate(tbl)                   # 'p34c3'

remove = {ord(c): None for c in "aeiouAEIOU"}
"Beautiful Day".translate(remove)        # 'Btfl Dy'

"\tcol1\tcol2".expandtabs(4)    # '    col1    col2'

```
## Strip and pad
```
"  hi  ".strip()                # 'hi'
"--hi--".strip("-")             # 'hi'
"  hi  ".lstrip()               # 'hi  '
"  hi  ".rstrip()               # '  hi'

"hi".center(6, "*")             # '**hi**'
"hi".ljust(5, ".")              # 'hi...'
"hi".rjust(5, ".")              # '...hi'
"42".zfill(5)                   # '00042'
"-42".zfill(5)                  # '-0042'

```
## Case operations
```
"Hello WORLD".lower()           # 'hello world'
"Hello WORLD".upper()           # 'HELLO WORLD'
"hello world".title()           # 'Hello World'
"hello world".capitalize()      # 'Hello world'
"HeLLo".swapcase()              # 'hEllO'
"Straße".casefold()             # 'strasse'

```
## Character classification (bool)
```
"abc".isalpha()                 # True
"abc123".isalnum()              # True
"123".isdecimal()               # True
"Ⅳ".isdigit()                   # True
"10²".isnumeric()               # True
"  \t\n".isspace()              # True
"abc".islower()                 # True
"ABC".isupper()                 # True
"Hello World".istitle()         # True
"π".isascii()                   # False
"var_1".isidentifier()          # True
"Hi!\n".isprintable()           # False
```

## Subsequence, iteration, immutability
```
for ch in "abc": pass           # iterate
parts = []
for i in range(3):
    parts.append(str(i))
result = ",".join(parts)        # '0,1,2'

```
## f-string conversions and specs
```
x = "café"
f"{x!s}"         # 'café'
f"{x!r}"         # "'café'"
f"{x!a}"         # 'caf\\xe9'

n = 255
f"{n:#06x}"      # '0x00ff'
f"{3.14159:8.2f}"# '    3.14'
f"{'hi':^6}"     # '  hi  '
```

## Format-spec mini-language cheats
```
f"{'hi':*<6}"    # 'hi****'
f"{'hi':*>6}"    # '****hi'
f"{'hi':*^6}"    # '**hi**'
f"{1234:+08d}"   # '+0001234'
f"{1234567:,d}"  # '1,234,567'
f"{1234567:_d}"  # '1_234_567'
f"{3.14159:.3f}" # '3.142'
f"{255:#x}"      # '0xff'
f"{255:#08b}"    # '0b11111111'
f"{0.00123:.2e}" # '1.23e-03'
f"{-42:=+06d}"   # '-00042'
```

## Raw, bytes, escapes
```
path = r"C:\new\folder"
u = "\u03C0 = \u2248 3.14"     # 'π = ≈ 3.14'
b = b"hello\x20world"
s = b.decode("utf-8")
out = s.encode("utf-8")
```

## str.maketrans variants
```
tbl1 = str.maketrans({"a":"@", "e":"3"})
tbl2 = str.maketrans("abc", "123")
tbl3 = str.maketrans("", "", "aeiou")
"xpeacex".translate(tbl1)       # 'xp3@c3x'
"abc".translate(tbl2)           # '123'
"Beautiful".translate(tbl3)     # 'Btfl'
```

## Bytes boundary helpers
```
s = "Ω"
b = s.encode("utf-8")
s2 = b.decode("utf-8")
```

## string module quick picks
```
import string
string.ascii_letters     # 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
string.digits            # '0123456789'
string.hexdigits         # '0123456789abcdefABCDEF'
string.octdigits         # '01234567'
string.punctuation
string.whitespace
string.printable

from string import Template
tpl = Template("$name scored $pts")
tpl.substitute(name="Lin", pts=42)        # 'Lin scored 42'
tpl.safe_substitute(name="Lin")           # 'Lin scored $pts'
```

## Quick recipes
```
rev = "abc"[::-1]                         
sub = "straße"; hay = "STRASSE"
ok = sub.casefold() in hay.casefold()      
head, sep, tail = "a=b=c".partition("=")   
path = "README.md"
path.lower().endswith((".md", ".rst"))     
```

## Method name checklist (call signatures)

s.find(sub[, start[, end]]); s.rfind(sub[, start[, end]])
s.index(sub[, start[, end]]); s.rindex(sub[, start[, end]])
s.count(sub[, start[, end]])
s.startswith(prefix[, start[, end]]); s.endswith(suffix[, start[, end]])
s.split(sep=None, maxsplit=-1); s.rsplit(sep=None, maxsplit=-1)
s.splitlines(keepends=False)
sep.join(iterable)
s.partition(sep); s.rpartition(sep)
s.replace(old, new[, count])
str.maketrans(x[, y[, z]]); s.translate(table)
s.strip([chars]); s.lstrip([chars]); s.rstrip([chars])
s.center(width[, fillchar]); s.ljust(width[, fillchar]); s.rjust(width[, fillchar])
s.zfill(width); s.expandtabs(tabsize=8)
s.lower(); s.upper(); s.title(); s.capitalize(); s.swapcase(); s.casefold()
s.isalpha(); s.isalnum(); s.isascii(); s.isdecimal(); s.isdigit(); s.isnumeric()
s.isspace(); s.isprintable(); s.islower(); s.isupper(); s.istitle(); s.isidentifier()
s.format(*args, **kwargs); s.format_map(mapping); s.encode(encoding='utf-8', errors='strict')
