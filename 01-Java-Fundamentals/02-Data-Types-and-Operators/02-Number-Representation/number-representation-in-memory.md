# Number Representation in Java Memory

> **Revision goal:** Understand how Java stores positive and negative integers, how floating-point numbers are represented, and how the JVM interprets their bits.

## 1. How Are Integers Stored?

Java uses **binary representation** internally.

Example:

```text
Decimal 5 = Binary 00000101  (8-bit illustration)
```

For signed integral types, Java uses **Two's Complement** representation.

## 2. Why Two's Complement?

Two's Complement makes addition and subtraction work using the same binary arithmetic hardware.

Benefits:
- One representation for zero.
- No separate positive-zero and negative-zero representations.
- Addition and subtraction are simpler to implement.
- The most significant bit indicates the sign for signed integer types.

## 3. How to Represent a Negative Number?

Example: Represent `-5` using 8 bits.

```text
Step 1: +5 in binary

00000101

Step 2: Invert all bits (One's Complement)

11111010

Step 3: Add 1

11111010
       +1
---------
11111011

Result: -5
```

**Rule:** Negative number = invert positive binary bits + 1.

## 4. How Does Java Know a Number Is Negative?

For signed integer types:
- The most significant bit (MSB) is the sign bit.
- `0` means non-negative.
- `1` means negative.
- The bits are interpreted as a Two's Complement value.

Example, using 8-bit `byte`:

```text
+5  = 00000101
-5  = 11111011
       ^
       MSB = 1 → negative
```

The JVM does not need a separate sign marker. The bit pattern itself represents the signed value.

## 5. Byte Range — Why -128 to 127?

A byte contains 8 bits.

- Total possible bit patterns = 2⁸ = 256.
- Signed range = -2⁷ to 2⁷ - 1.
- Therefore: -128 to 127.

```text
byte: 8 bits

00000000 = 0
01111111 = 127
10000000 = -128
11111111 = -1
```

**Interview trap:** The positive maximum is one less than the magnitude of the negative minimum.

## 6. How Are Float and Double Stored?

Java uses the IEEE 754 floating-point standard.

| Type | Total bits | Sign | Exponent | Fraction |
|---|---:|---:|---:|---:|
| `float` | 32 | 1 | 8 | 23 |
| `double` | 64 | 1 | 11 | 52 |

```text
FLOAT (32 bits)

┌─────────┬────────────────┬─────────────────────────┐
│ Sign 1  │ Exponent 8     │ Fraction / Mantissa 23  │
└─────────┴────────────────┴─────────────────────────┘
```

```text
DOUBLE (64 bits)

┌─────────┬──────────────────┬──────────────────────────┐
│ Sign 1  │ Exponent 11      │ Fraction / Mantissa 52   │
└─────────┴──────────────────┴──────────────────────────┘
```

## 7. Steps: How Is a Floating-Point Number Stored?

Use `5.75` as an example.

### Step 1: Convert to binary

```text
5  = 101
0.75 = 0.11

5.75 = 101.11
```

### Step 2: Normalize

Move the binary point until one non-zero digit remains before it:

```text
101.11 = 1.0111 × 2²
```

### Step 3: Determine the sign bit

- Positive number → `0`
- Negative number → `1`

For `5.75`, sign bit = `0`.

### Step 4: Calculate the biased exponent

For `float`:
- Exponent bits = 8
- Bias = 127
- Actual exponent = 2
- Stored exponent = 2 + 127 = 129
- 129 in binary = `10000001`

### Step 5: Store the fraction

From normalized `1.0111`:
- The leading `1` is implicit for normal numbers.
- Store the bits after the binary point: `0111...`

### Final layout (float)

```text
Sign       Exponent       Fraction
  0        10000001       01110000000000000000000
```

## 8. Why Is Bias Used?

The exponent can be positive or negative.

Instead of storing a signed exponent separately, IEEE 754 uses a biased exponent.

```text
Stored Exponent = Actual Exponent + Bias
Actual Exponent = Stored Exponent - Bias
```

Benefits:
- Exponents can be stored as unsigned bit patterns.
- Comparison and ordering of normal exponent fields are simplified.
- A consistent representation is used for positive and negative exponents.

## 9. Why Is the Float Bias 127?

For an 8-bit exponent:

```text
Bias = 2^(k-1) - 1

k = 8

Bias = 2^7 - 1
     = 128 - 1
     = 127
```

For `double`, exponent width is 11 bits:

```text
Bias = 2^10 - 1 = 1023
```

**Remember:**
- `float` bias = 127
- `double` bias = 1023

## 10. How Is a Floating-Point Number Retrieved?

For a normal IEEE 754 number:

```text
Value = (-1)^Sign × 1.Fraction × 2^(StoredExponent - Bias)
```

For our example:

```text
Sign = 0
Stored exponent = 129
Bias = 127
Fraction = .0111...

Value = (-1)^0 × 1.0111₂ × 2^(129 - 127)
      = 1.0111₂ × 2²
      = 101.11₂
      = 5.75
```

**Important:** This formula describes normal values. Zero, subnormal values, infinity and NaN have special exponent/fraction patterns.

## 11. Why Can Floating-Point Calculations Be Inaccurate?

Some decimal fractions cannot be represented exactly in a finite binary fraction.

```java
public class FloatingPointExample {
    public static void main(String[] args) {
        System.out.println(0.1 + 0.2);
    }
}
```

The result may be:

```text
0.30000000000000004
```

**Why?**
- `0.1` and `0.2` have repeating binary representations.
- They are stored as approximations.
- Arithmetic operates on those approximations.

For financial calculations, consider `BigDecimal` and use appropriate rounding.

## 12. Quick Revision

| Topic | Remember |
|---|---|
| Integer representation | Two's Complement |
| Negative conversion | Invert bits + 1 |
| Byte width | 8 bits |
| Byte range | -128 to 127 |
| Float standard | IEEE 754 |
| Float layout | 1 + 8 + 23 bits |
| Double layout | 1 + 11 + 52 bits |
| Float bias | 127 |
| Double bias | 1023 |
| Stored exponent | Actual exponent + bias |
| Floating-point issue | Precision approximation |

## 13. Interview Questions

1. Why does Java use Two's Complement?
2. Why is the byte range -128 to 127 and not -127 to 127?
3. How does Java identify a negative integer?
4. What is the difference between One's Complement and Two's Complement?
5. Why does floating-point representation need an exponent?
6. What is the purpose of exponent bias?
7. Why is the float bias 127?
8. Why is `0.1 + 0.2` not exactly `0.3`?
9. What is the difference between float and double?
10. What are NaN and infinity?

## Related Files

- [Data Types](../01-Data-Types-and-Variables/data-types-variables-and-keywords.md)
- [Type Conversion](../03-Type-Conversion/type-conversion-and-promotion.md)