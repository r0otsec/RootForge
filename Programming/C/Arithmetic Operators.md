---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["Arithmetic Operators", "C Math Operators", "C Calculations", "Integer Division", "Modulus Operator"]
---

## Overview

Arithmetic operators perform mathematical calculations on numeric values in C. The five basic operators are addition (`+`), subtraction (`-`), multiplication (`*`), division (`/`), and modulus (`%`). These operators work similarly to mathematical notation, but division and modulus have special behaviors you need to understand.

## Why Use Arithmetic Operators

Arithmetic operators are fundamental to any program that needs to perform calculations, from simple counters to complex scientific computations. Understanding how they work, especially integer division and modulus, is crucial for writing correct mathematical logic.

## The Five Basic Operators

| Operator | Name           | Example | Result | Notes                              |
| -------- | -------------- | ------- | ------ | ---------------------------------- |
| `+`      | Addition       | `5 + 3` | `8`    | Adds two operands                  |
| `-`      | Subtraction    | `5 - 3` | `2`    | Subtracts second from first        |
| `*`      | Multiplication | `5 * 3` | `15`   | Multiplies two operands            |
| `/`      | Division       | `5 / 3` | `1`    | Integer division truncates decimal |
| `%`      | Modulus        | `5 % 3` | `2`    | Returns remainder of division      |

## Code Example

```c
#include <stdio.h>

int main(void) {
    int a = 10;
    int b = 3;
    float x = 10.0;
    float y = 3.0;

    // Basic arithmetic with integers
    printf("Addition: %d + %d = %d\n", a, b, a + b);         // 13
    printf("Subtraction: %d - %d = %d\n", a, b, a - b);      // 7
    printf("Multiplication: %d * %d = %d\n", a, b, a * b);   // 30

    // Integer division truncates (no decimal part)
    printf("Integer division: %d / %d = %d\n", a, b, a / b); // 3 (not 3.333...)

    // Modulus gives remainder of division
    printf("Modulus: %d %% %d = %d\n", a, b, a % b);         // 1 (remainder)

    // Float division preserves decimal
    printf("Float division: %.2f / %.2f = %.2f\n", x, y, x / y); // 3.33

    return 0;
}
```

### Explanation

This code demonstrates the five arithmetic operators. Notice the crucial difference between integer division (10/3 = 3) which discards the fractional part, and float division (10.0/3.0 = 3.33) which keeps it. The modulus operator (`%`) returns the remainder after division, which is useful for tasks like determining if a number is even or checking if one number divides evenly into another.

## Integer vs Float Division

### Integer Division Behavior

When both operands are integers, division truncates toward zero:

```c
int result1 = 10 / 3;    // Result: 3
int result2 = -10 / 3;   // Result: -3
int result3 = 10 / 4;    // Result: 2
```

### Getting Decimal Results

To get decimal results, at least one operand must be a floating-point type:

```c
float result1 = 10.0 / 3;     // Result: 3.333...
float result2 = 10 / 3.0;     // Result: 3.333...
float result3 = (float)10 / 3; // Result: 3.333... (using type cast)
float result4 = 10 / 3;       // Result: 3.0 (still integer division!)
```

## Modulus Operator Use Cases

### Check if Number is Even/Odd

```c
if (number % 2 == 0) {
    printf("Even\n");
} else {
    printf("Odd\n");
}
```

### Cycle Through Values

```c
// Cycle through 0, 1, 2, 0, 1, 2, ...
int position = counter % 3;
```

### Get Last Digit

```c
int last_digit = number % 10;
```

## Operator Precedence

Multiplication, division, and modulus have higher precedence than addition and subtraction:

```c
int result1 = 2 + 3 * 4;      // 14 (not 20)
int result2 = (2 + 3) * 4;    // 20 (parentheses change order)
int result3 = 10 - 6 / 2;     // 7 (not 2)
```

## Important Considerations

### Integer Division Truncation

Integer division truncates toward zero, which surprises many beginners who expect 10/3 to give 3.33. If you need decimal results, at least one operand must be a floating-point type.

### Modulus with Integers Only

The modulus operator (`%`) only works with integer types - using it with floats will cause a compilation error. For floating-point remainder operations, use the `fmod()` function from `<math.h>`.

### Division by Zero

Dividing by zero causes undefined behavior and typically crashes the program. Always validate denominators before division:

```c
if (b != 0) {
    result = a / b;
} else {
    printf("Error: Division by zero\n");
}
```

### Negative Modulus

The result of modulus with negative numbers can be counterintuitive:

```c
int result1 = -10 % 3;   // Result: -1 (takes sign of dividend)
int result2 = 10 % -3;   // Result: 1
```

---

## Related Topics

- [[Variables]] - Understanding numeric data types used in arithmetic
- [[C Format Specifiers]] - Displaying calculation results
- [[C Math Functions]] - Advanced mathematical operations beyond basic arithmetic
