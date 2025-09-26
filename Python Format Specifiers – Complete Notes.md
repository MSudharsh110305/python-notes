

Python format specifiers allow precise control over how values are displayed using **f-strings** (`f"{value:specifier}"`) or the **`str.format()`** method. These specifiers help format numbers, strings, and floating-point values efficiently.

---

## **📌 Common Format Specifiers:**

|**Specifier**|**Description**|
|---|---|
|`b`|Binary format (base 2)|
|`c`|Character (converts an integer to a Unicode character)|
|`d`|Decimal format (base 10)|
|`o`|Octal format (base 8)|
|`x`|Hexadecimal format (lowercase `a-f`)|
|`X`|Hexadecimal format (uppercase `A-F`)|
|`e`|Exponential notation (scientific notation, lowercase `e`)|
|`E`|Exponential notation (uppercase `E`)|
|`f`|Fixed-point decimal notation (specific number of decimal places)|
|`F`|Same as `f`, but `NaN` and `Infinity` appear in uppercase|
|`g`|General format (fixed-point or scientific notation based on value)|
|`G`|General format (uppercase `E` in scientific notation)|
|`n`|Number formatting (same as `d`, but follows locale settings)|
|`%`|Percentage (multiplies by 100 and appends `%`)|

---

## **📌 Numeric Formatting Examples**

```python
num = 255
print(f"{num:b}")  # Binary: 11111111
print(f"{num:o}")  # Octal: 377
print(f"{num:d}")  # Decimal: 255
print(f"{num:x}")  # Hexadecimal (lowercase): ff
print(f"{num:X}")  # Hexadecimal (uppercase): FF
```

```python
pi = 3.1415926535
print(f"{pi:e}")  # Exponential: 3.141593e+00
print(f"{pi:E}")  # Exponential (uppercase): 3.141593E+00
print(f"{pi:f}")  # Fixed-point: 3.141593
print(f"{pi:F}")  # Fixed-point (uppercase NaN/Inf): 3.141593
print(f"{pi:.2f}")  # Two decimal places: 3.14
```

```python
ratio = 0.75
print(f"{ratio:%}")  # Percentage: 75.000000%
print(f"{ratio:.2%}")  # Rounded percentage: 75.00%
```

---

## **📌 Precision & Width Specifiers**

|**Specifier**|**Description**|
|---|---|
|`.nf`|Rounds floating-point numbers to `n` decimal places|
|`w`|Specifies minimum width for numbers|
|`0w`|Pads numbers with zeros to `w` width|

#### **Examples:**

```python
# Fixed-width integers
print(f"{42:5d}")   # Output: '   42' (5 characters wide, right-aligned)
print(f"{42:05d}")  # Output: '00042' (padded with zeros)

# Floating-point formatting with width & precision
pi = 3.1415926535
print(f"{pi:10.3f}")  # '      3.142' (10-width, 3 decimal places)
```

---

## **📌 Alignment & Padding**

|Symbol|Meaning|
|---|---|
|`<`|Left-align (default for strings)|
|`>`|Right-align (default for numbers)|
|`^`|Center-align|
|`0`|Pad with zeros|

#### **Examples:**

```python
print(f"{42:<5}")  # '42   ' (left-aligned)
print(f"{42:>5}")  # '   42' (right-aligned)
print(f"{42:^5}")  # ' 42  ' (center-aligned)
```

---

## **📌 Advanced Tricks & Hacks**

### **1️⃣ Convert a Number to Different Bases**

```python
num = 42
print(f"Binary: {num:b}, Octal: {num:o}, Hex: {num:x}")
```

### **2️⃣ Print Locale-Based Numbers**

```python
import locale
locale.setlocale(locale.LC_ALL, '')  
print(f"{1234567:n}")  # Locale-based formatting (e.g., '1,234,567')
```

### **3️⃣ Format Numbers with Thousands Separator**

```python
print(f"{1000000:,}")  # 1,000,000
```

### **4️⃣ Truncate Strings to a Fixed Length**

```python
text = "HelloWorld"
print(f"{text:.5}")  # 'Hello' (first 5 characters)
```

### **5️⃣ Display Memory Address of a Variable**

```python
x = 10
print(f"{id(x):#x}")  # Memory address in hexadecimal
```

---
