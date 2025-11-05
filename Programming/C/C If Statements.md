---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C If Statements", "If Statement", "Conditional Statements", "If-Else", "Control Flow"]
---

## Overview

An if statement is a conditional control structure that executes a block of code only when a specified condition evaluates to true (non-zero). The condition is evaluated as a boolean expression, and if it results in any non-zero value, the code block executes; otherwise, it's skipped. This is the most fundamental decision-making construct in C programming.

## Why Use It

If statements allow programs to make decisions and respond differently based on varying conditions, enabling dynamic behavior rather than linear execution. They are essential for implementing logic such as validation, error checking, different execution paths, and responding to user input.

Without conditional statements, programs could only execute instructions in a fixed sequence, making them unable to adapt to different situations or handle user choices.

## Code Example

```c
#include <stdio.h>

int main(void) {
    int age;

    printf("Enter your age: ");
    scanf("%d", &age);

    /* Simple if statement - executes only if condition is true */
    if (age >= 18) {
        printf("You are an adult.\n");
    }

    /* if-else statement - provides alternative path */
    if (age >= 65) {
        printf("You qualify for senior discount.\n");
    } else {
        printf("Regular pricing applies.\n");
    }

    /* if-else if-else chain - multiple conditions checked sequentially */
    if (age < 0) {
        printf("Invalid age entered.\n");
    } else if (age < 13) {
        printf("You are a child.\n");
    } else if (age < 18) {
        printf("You are a teenager.\n");
    } else {
        printf("You are an adult.\n");
    }

    return 0;
}
```

### Explanation

The program demonstrates three forms of if statements. It first prompts the user to enter their age, then uses a simple if statement to check if they're 18 or older. Next, it uses an if-else structure to determine discount eligibility. Finally, it uses an if-else if-else chain to categorize the age into one of four mutually exclusive groups.

The conditions use comparison operators like `>=` and `<` to evaluate the age variable against specific values.

## Important Considerations

### Assignment vs Comparison

Using `if (x = 5)` (assignment) instead of `if (x == 5)` (comparison) is a frequent error. The assignment operator `=` sets x to 5 and evaluates as true, while `==` tests for equality. This bug is hard to spot because it's syntactically valid.

### Braces Are Optional But Dangerous

For single statements, braces are optional but error-prone:
```c
if (x > 0)
    printf("Positive\n");
    printf("This always executes!\n");  // Not part of if statement
```

Always use braces `{}` to avoid confusion and prevent bugs when code is modified later.

### Truthiness in C

Any non-zero value is true, and zero is false. This means `if (5)` is always true, and `if (0)` is always false. Uninitialized variables can contain random values, leading to unpredictable behavior.

### Dangling Else Problem

An else always binds to the nearest unmatched if, which can cause confusion with nested statements:
```c
if (condition1)
    if (condition2)
        statement1;
else  // This belongs to if (condition2), not if (condition1)
    statement2;
```

### Floating-Point Comparisons

Never use exact equality with floats due to precision issues. Instead of `if (x == 0.1)`, use a tolerance threshold:
```c
if (fabs(x - 0.1) < 0.0001)
```

---

## Related Topics

- [[Variables]] - Understanding variables used in conditions
- [[C Logical Operators]] - Combining multiple conditions
- [[C Nested If Statements]] - Creating complex decision trees
- [[Switch Statements]] - Alternative for multiple discrete values
- [[Arithmetic Operators]] - Operators used in conditional expressions
