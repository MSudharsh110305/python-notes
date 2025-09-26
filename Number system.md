## Types of Numbers in Python



## Number Representation

- **Decimal** (base 10): `123`, `-456`
- **Binary** (base 2): Prefix `0b` or `0B`, e.g., `0b1010` (which is `10` in decimal)
- **Octal** (base 8): Prefix `0o` or `0O`, e.g., `0o12` (which is `10` in decimal)
- **Hexadecimal** (base 16): Prefix `0x` or `0X`, e.g., `0xA` (which is `10` in decimal)

##  Converting Between Number Systems

- **Decimal to Binary**: `bin(n)`
- **Decimal to Octal**: `oct(n)`
- **Decimal to Hexadecimal**: `hex(n)`
- **Binary/Octal/Hexadecimal to Decimal**: Use `int` with the appropriate base

```
int('1010', 2)  # Binary to Decimal
int('12', 8)    # Octal to Decimal
int('A', 16)    # Hexadecimal to Decimal
```

## Built-in Functions for Numbers

- **`abs(x)`**: Returns the absolute value of `x`.
- **`round(x[, n])`**: Rounds `x` to `n` decimal places.
- **`pow(x, y[, z])`**: Returns `(x ** y) % z` (optional).
- **`divmod(x, y)`**: Returns a tuple of quotient and remainder `(x // y, x % y)`.
- **`complex(real, [imag])`**: Creates a complex number.
- **`int(x[, base])`**: Converts `x` to an integer. The base is optional.
- **`float(x)`**: Converts `x` to a floating-point number.
- **`complex(x)`**: Converts `x` to a complex number.