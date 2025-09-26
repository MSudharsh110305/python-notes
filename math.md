## Number-Theoretic and Representation Functions

<span style="color:rgb(255, 255, 0)">math.ceil(x)</span>: Returns the smallest integer greater than or equal to x.
```
math.ceil(4.3) # 5
```
<span style="color:rgb(255, 255, 0)">math.comb(n, k)</span>:Returns the number of ways to choose k items from n items without repetition and without order.
```
math.comb(5, 2) #10
```
<span style="color:rgb(255, 255, 0)">math.copysign(x, y)</span>: Returns a float with the magnitude of x and the sign of y.
```
math.copysign(2.5, -1) # -2.5
```
<span style="color:rgb(255, 255, 0)">math</span><span style="color:rgb(255, 255, 0)">.</span><span style="color:rgb(255, 255, 0)">fabs</span><span style="color:rgb(255, 255, 0)"><span style="color:rgb(255, 255, 0)">(<span style="color:rgb(255, 255, 0)">x</span></span>)</span>: Returns the absolute value of x. always returns floating point
```
math.fabs(-5.5) # 5.5
```
<span style="color:rgb(255, 255, 0)">math.factorial(n)</span>: Returns the factorial of n.
```
math.factorial(5) # 120
```
<span style="color:rgb(255, 255, 0)">math.floor(x)</span>: Returns the largest integer less than or equal to x.
```
math.floor(4.8) # 4
```
<span style="color:rgb(255, 255, 0)">math.fmod(x, y)</span>: Returns x % y as per C library, which maintains the sign of x.
```
math.fmod(10.5, 3) # 1.5
```
<span style="color:rgb(255, 255, 0)">math.frexp(x)</span>: Returns the mantissa and exponent of x as (m, e).
```
math.frexp(8) # (0.5, 4)
```
<span style="color:rgb(255, 255, 0)">math.fsum(iterable)</span>: Returns an accurate floating point sum of values in iterable.
```
math.fsum([0.1, 0.2, 0.3]) # 0.6
```
<span style="color:rgb(255, 255, 0)">math</span><span style="color:rgb(255, 255, 0)">.</span><span style="color:rgb(255, 255, 0)">gcd</span>(*integers)*: Returns the greatest common divisor of specified integers.
```
math.gcd(60, 48) # 12
```
<span style="color:rgb(255, 255, 0)">math.isclose(a, b</span>, *, rel_tol=1e-09, abs_tol=0.0)*: Returns True if a and b are close to each other.
```
math.isclose(1.00000001, 1.00000002) # True
```
<span style="color:rgb(255, 255, 0)">math.isfinite(x)</span>: Returns True if x is neither infinity nor NaN.
```
math.isfinite(10) # True
```
<span style="color:rgb(255, 255, 0)">math.isinf(x)</span>: Returns True if x is infinity.
```
math.isinf(float('inf')) # True
```
<span style="color:rgb(255, 255, 0)">math.isnan(x)</span>: Returns True if x is NaN.
```
math.isnan(float('nan')) # True
```
<span style="color:rgb(255, 255, 0)">math.isqrt(n)</span>: Returns the integer square root of n.
```
math.isqrt(16) # 4
```
<span style="color:rgb(255, 255, 0)">math.lcm</span><span style="color:rgb(255, 255, 0)">(</span>*integers)*: Returns the least common multiple of specified integers.
```
math.lcm(12, 15) # 60
```
<span style="color:rgb(255, 255, 0)">math.ldexp(x, i)</span>: Returns x * (2i), the inverse of frexp().
```
math.ldexp(0.5, 3) # 4.0
```
<span style="color:rgb(255, 255, 0)">math.modf(x)</span>: Returns the fractional and integer parts of x as (frac, int).
```
math.modf(5.5) # (0.5, 5.0)
```
<span style="color:rgb(255, 255, 0)">math.nextafter(x, y, steps=1)</span>: Returns the next floating-point value after x towards y.
```
math.nextafter(1.0, 2.0) # 1.0000000000000002
```
<span style="color:rgb(255, 255, 0)">math.perm(n, k=None)</span>: Returns the number of permutations of n items taken k at a time.
```
math.perm(5, 2) # 20
```
<span style="color:rgb(255, 255, 0)">math</span>.<span style="color:rgb(255, 255, 0)">prod</span><span style="color:rgb(255, 255, 0)">(</span><span style="color:rgb(255, 255, 0)">iterable</span>, *, start=1)*: Returns the product of iterable.
```
math.prod([1, 2, 3, 4]) # 24
```
<span style="color:rgb(255, 255, 0)">math.remainder(x, y)</span>: Returns the IEEE 754-style remainder of x with respect to y.
```
math.remainder(10, 3) # 1.0
```
<span style="color:rgb(255, 255, 0)">math.sumprod(p, q)</span>: Returns the sum of products of values from two iterables.
```
math.sumprod([1, 2], [3, 4]) # 11
```
<span style="color:rgb(255, 255, 0)">math.trunc(x)</span>: Returns the integer part of x, truncating towards zero.
```
math.trunc(4.7) # 4
```
<span style="color:rgb(255, 255, 0)">math.<span style="color:rgb(255, 255, 0)">ulp</span>(x)</span>: Returns the value of the least significant bit of x.
```
math.ulp(1.0) # 2.220446049250313e-16
```

## Power and Logarithmic Functions

<span style="color:rgb(255, 192, 0)">math.cbrt(x)</span>: Returns the cube root of x.
```
math.cbrt(8) # 2
```
<span style="color:rgb(255, 192, 0)">math.exp(x)</span>: Returns e raised to the power x.

<span style="color:rgb(255, 192, 0)">math.exp2(x)</span>: Returns 2 raised to the power x.

<span style="color:rgb(255, 192, 0)">math.expm1</span><span style="color:rgb(255, 192, 0)">(x)</span>: Returns e raised to the power x, minus 1.

<span style="color:rgb(255, 192, 0)">math.log(</span><span style="color:rgb(255, 192, 0)">x</span>[, base]<span style="color:rgb(255, 192, 0)">)</span>: Returns the natural logarithm of x or the logarithm of x to the given base.

<span style="color:rgb(255, 192, 0)">math.log1p(x)</span>: Returns the natural logarithm of 1+x.

<span style="color:rgb(255, 192, 0)">math.log2(x)</span>: Returns the base-2 logarithm of x.

<span style="color:rgb(255, 192, 0)">math.log10(x)</span>: Returns the base-10 logarithm of x.

<span style="color:rgb(255, 192, 0)">math.pow(x, y)</span>: Returns x raised to the power y.

<span style="color:rgb(255, 192, 0)">math.sqrt(x</span><span style="color:rgb(255, 192, 0)">)</span>: Returns the square root of x.

