# Operators in Java

> **Revision goal:** Quickly identify operator categories, understand bitwise operations, distinguish logical operators and remember precedence.

## 1. Operator Categories

| Category | Operators | Purpose |
|---|---|---|
| Arithmetic | `+ - * / %` | Mathematical operations |
| Unary | `+ - ++ -- ! ~` | Operate on one operand |
| Relational | `== != > < >= <=` | Compare values |
| Logical | `&& || !` | Combine boolean conditions |
| Bitwise | `& | ^ ~` | Operate on individual bits |
| Shift | `<< >> >>>` | Shift bit patterns |
| Assignment | `= += -= *= /= %=` | Assign/update values |
| Conditional | `?:` | Short conditional expression |
| Type comparison | `instanceof` | Check object type compatibility |

## 2. Arithmetic Operators

```java
int a = 10;
int b = 3;

System.out.println(a + b); // 13
System.out.println(a - b); // 7
System.out.println(a * b); // 30
System.out.println(a / b); // 3
System.out.println(a % b); // 1
```

**Remember:**
- Integer division removes the fractional part.
- `%` returns the remainder.
- Division by zero with integer operands throws `ArithmeticException`.
- Floating-point division by zero follows IEEE 754 rules and can produce infinity or NaN.

## 3. Relational Operators

Return a boolean result.

```java
int age = 25;

System.out.println(age > 18);  // true
System.out.println(age == 25); // true
System.out.println(age != 20); // true
```

**Important:**
- `==` compares primitive values.
- For references, `==` checks whether both references point to the same object.
- Use `.equals()` when you want logical equality for objects such as `String`.

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);      // false
System.out.println(a.equals(b)); // true
```

## 4. Bitwise Operators

Bitwise operators work on individual bits of integral values.

Example:

```text
a = 5  → 0101
b = 3  → 0011
```

| Operator | Name | Result |
|---|---|---|
| `&` | AND | `0001` = 1 |
| `|` | OR | `0111` = 7 |
| `^` | XOR | `0110` = 6 |
| `~` | Complement | Inverts all bits |

### Truth table

| A | B | A & B | A \| B | A ^ B |
|---:|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |

**XOR properties:**
- Same bits → `0`
- Different bits → `1`
- `x ^ 0 = x`
- `x ^ x = 0`

## 5. Bit Manipulation — Common Patterns

Assume `n` is an integer and `i` is a bit position, starting from zero at the right.

| Task | Expression |
|---|---|
| Check bit `i` | `(n & (1 << i)) != 0` |
| Set bit `i` | `n \| (1 << i)` |
| Clear bit `i` | `n & ~(1 << i)` |
| Toggle bit `i` | `n ^ (1 << i)` |
| Remove lowest set bit | `n & (n - 1)` |
| Isolate lowest set bit | `n & -n` |

Example:

```text
n = 10 = 1010

Set bit 0:
1010
0001
----
1011 = 11
```

**Note:** These common patterns assume ordinary `int` operations and valid bit positions. Use `1L << i` when working with `long` bit patterns.

## 6. Shift Operators — In Depth

Java has three shift operators:

| Operator | Meaning | Behavior |
|---|---|---|
| `<<` | Left shift | Shifts bits left; fills right side with zero |
| `>>` | Signed right shift | Shifts right; preserves sign by copying sign bit |
| `>>>` | Unsigned right shift | Shifts right; fills left side with zero |

### A. Left Shift `<<`

```java
int n = 5;
System.out.println(n << 1); // 10
```

```text
5       = 00000101
5 << 1  = 00001010
          = 10
```

For ordinary positive values where overflow does not occur, shifting left by one is equivalent to multiplying by 2.

### B. Signed Right Shift `>>`

```java
int n = 20;
System.out.println(n >> 2); // 5
```

```text
20       = 00010100
20 >> 2  = 00000101
           = 5
```

For negative numbers, the sign bit is copied on the left.

### C. Unsigned Right Shift `>>>`

```java
int n = -8;

System.out.println(n >> 1);
System.out.println(n >>> 1);
```

- `>>` preserves the negative sign.
- `>>>` fills with zeros and can produce a large positive result.

### Shift comparison

```text
<<   Left shift:          0101 → 1010
>>   Signed right shift:  1010 → 1101  (for a negative signed pattern)
>>>  Unsigned right shift:1010 → 0101  (illustrative bit pattern)
```

**Important rules:**
- Shift operations promote `byte`, `short` and `char` operands to `int`.
- For an `int`, only the lowest 5 bits of the shift distance are used.
- For a `long`, only the lowest 6 bits are used.
- Shifting can discard bits and cause overflow.
- `>>>` does not mean mathematical division for every signed value.

## 7. Logical Operators and Short-Circuiting

| Operator | Meaning | Short-circuit? |
|---|---|---|
| `&&` | Logical AND | Yes |
| `||` | Logical OR | Yes |
| `!` | Logical NOT | Not applicable |
| `&` | Boolean AND or bitwise AND | No short-circuit for boolean operands |
| `|` | Boolean OR or bitwise OR | No short-circuit for boolean operands |

### `&&` vs `&`

```java
int a = 10;

if (a > 5 && a < 20) {
    System.out.println("Valid");
}
```

With `&&`:
- If the first condition is false, Java skips the second condition.
- This is short-circuit evaluation.

With boolean `&`:
- Both operands are evaluated.

```java
int x = 0;

boolean result1 = (x != 0) && (10 / x > 1);
// Safe: second condition is skipped.

boolean result2 = (x != 0) & (10 / x > 1);
// Throws ArithmeticException: second condition is evaluated.
```

### Can We Use `&` and `|` Instead of `&&` and `||`?

Yes, when both operands are boolean expressions, but their evaluation behavior differs.

**Use:**
- `&&` and `||` for normal logical conditions.
- `&` and `|` when you deliberately want both boolean operands evaluated.
- `&`, `|` and `^` for bitwise operations on integral values.

## 8. Assignment Operators

```java
int x = 10;

x += 5; // x = x + 5
x -= 2; // x = x - 2
x *= 3; // x = x * 3
x /= 2; // x = x / 2
x %= 4; // x = x % 4
```

Other assignment operators include:
- `&=`
- `|=`
- `^=`
- `<<=`
- `>>=`
- `>>>=`

## 9. Increment and Decrement

```java
int a = 5;

System.out.println(a++); // 5 (use, then increment)
System.out.println(++a); // 7 (increment, then use)
```

| Operator | Meaning |
|---|---|
| `a++` | Post-increment |
| `++a` | Pre-increment |
| `a--` | Post-decrement |
| `--a` | Pre-decrement |

## 10. Operator Precedence

Higher-precedence operators are evaluated before lower-precedence operators.

| Priority (high → low) | Operators |
|---|---|
| 1 | Postfix: `expr++`, `expr--` |
| 2 | Unary: `++`, `--`, `+`, `-`, `!`, `~`, cast |
| 3 | Multiplicative: `*`, `/`, `%` |
| 4 | Additive: `+`, `-` |
| 5 | Shift: `<<`, `>>`, `>>>` |
| 6 | Relational: `<`, `>`, `<=`, `>=`, `instanceof` |
| 7 | Equality: `==`, `!=` |
| 8 | Bitwise AND: `&` |
| 9 | Bitwise XOR: `^` |
| 10 | Bitwise OR: `|` |
| 11 | Logical AND: `&&` |
| 12 | Logical OR: `||` |
| 13 | Ternary: `?:` |
| 14 | Assignment: `=`, `+=`, `-=`, etc. |

Example:

```java
int result = 10 + 5 * 2;
// 20, because multiplication happens first
```

Use parentheses to make complex expressions easier to understand:

```java
int result = (10 + 5) * 2; // 30
```

## 11. Quick Revision

- Arithmetic → calculations.
- Relational → comparison, returns boolean.
- Bitwise → works on individual bits.
- Logical → combines boolean expressions.
- `&&` and `||` → short-circuit.
- `&` and `|` with booleans → evaluate both operands.
- `<<` → left shift.
- `>>` → signed right shift.
- `>>>` → unsigned right shift.
- `++a` → increment, then use.
- `a++` → use, then increment.
- Parentheses are the clearest way to control evaluation order.

## 12. Interview Questions

1. Difference between `&` and `&&`?
2. Difference between `|` and `||`?
3. What is short-circuit evaluation?
4. Difference between `>>` and `>>>`?
5. What happens when we perform `5 << 1`?
6. How do you check whether a number is odd using bitwise operators?
7. How do you set, clear and toggle a bit?
8. What is the difference between `++a` and `a++`?
9. What is operator precedence?
10. Why should parentheses be used in complex expressions?

## Related Files

- [Type Conversion](../03-Type-Conversion/type-conversion-and-promotion.md)
- [Flow Control](../05-Flow-Control/flow-control-and-loops.md)