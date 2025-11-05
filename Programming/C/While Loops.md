---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C While Loops", "While Loop", "Condition-Based Loop", "Pre-Test Loop"]
---

## Overview

A while loop is a control structure that repeatedly executes a block of code as long as a specified condition remains true. Before each iteration, the condition is evaluated; if true, the loop body executes; if false, the loop terminates. Unlike for loops, while loops don't have built-in initialization or increment statements - you must manage the loop control variable manually.

## Why Use It

While loops are ideal when you don't know in advance how many iterations will be needed - the loop continues until a specific condition is met. Common use cases include processing input until a sentinel value is entered, waiting for user confirmation, game loops that run until the player quits, reading files until end-of-file, and validation loops that repeat until valid input is provided.

While loops make the termination condition explicit and prominent, which can improve code clarity when the iteration count is inherently unknown.

## While vs For Loops

| Aspect | While Loop | For Loop |
| ------ | ---------- | -------- |
| **Best For** | Unknown iteration count | Known iteration count |
| **Control Variable** | Manual management | Built-in init/update |
| **Typical Use** | Input validation, file reading | Array traversal, counting |
| **Syntax** | `while (condition) { }` | `for (init; condition; update) { }` |

## Code Example

```c
#include <stdio.h>

int main(void) {
    /* Example 1: Countdown with condition-based control */
    int count = 5;

    printf("Countdown:\n");
    while (count > 0) {  /* Loop continues while count is positive */
        printf("%d\n", count);
        count--;  /* Decrement must be done manually */
    }
    printf("Liftoff!\n\n");

    /* Example 2: Input validation - unknown number of iterations */
    int userInput;
    printf("Enter a positive number (negative to quit): ");
    scanf("%d", &userInput);

    while (userInput >= 0) {  /* Loop while input is non-negative */
        printf("You entered: %d\n", userInput);
        printf("Enter a positive number (negative to quit): ");
        scanf("%d", &userInput);
    }
    printf("Exiting.\n\n");

    /* Example 3: Accumulator pattern */
    int sum = 0;
    int i = 1;

    printf("Sum of 1-10:\n");
    while (i <= 10) {
        sum += i;
        printf("i = %d, sum = %d\n", i, sum);
        i++;
    }
    printf("Final sum: %d\n", sum);

    return 0;
}
```

### Explanation

The first example counts down from 5 to 1 - the loop continues while `count > 0` is true, and count is manually decremented each iteration. The second example shows input validation, repeatedly prompting until a negative number is entered - the iteration count isn't known in advance, making while more natural than for. The third example demonstrates the accumulator pattern, calculating the sum of numbers 1 through 10.

## Important Considerations

### Pre-Test Loop

The condition is checked before each iteration, including the first:
```c
int x = 10;
while (x < 5) {  // False immediately
    printf("This never executes\n");
}
printf("Loop skipped entirely\n");
```

If the condition is false initially, the loop body never executes.

### Manual Control Variable Management

You must initialize and update control variables manually:
```c
// Correct: initialize before loop, update inside loop
int i = 0;
while (i < 5) {
    printf("%d\n", i);
    i++;  // Must manually increment
}

// Common mistake: forgetting to update
int j = 0;
while (j < 5) {
    printf("%d\n", j);
    // Forgot j++ - infinite loop!
}
```

Forgetting to update the control variable causes infinite loops.

### Infinite Loops

Forgetting to modify the loop condition creates infinite loops:
```c
// Infinite loop - condition never becomes false
int count = 5;
while (count > 0) {
    printf("%d\n", count);
    // Forgot count-- - loops forever!
}

// Intentional infinite loop (must use break to exit)
while (1) {  // or while (true) in C99+
    printf("Enter command (quit to exit): ");
    char command[20];
    scanf("%19s", command);

    if (strcmp(command, "quit") == 0) {
        break;  // Exit loop
    }

    printf("Processing: %s\n", command);
}
```

Use Ctrl+C to terminate programs stuck in infinite loops.

### Off-by-One Errors

Carefully consider boundary conditions:
```c
// Prints 1-9 (off by one!)
int i = 1;
while (i < 10) {
    printf("%d ", i);
    i++;
}

// Correct: prints 1-10
int i = 1;
while (i <= 10) {  // Use <= for inclusive upper bound
    printf("%d ", i);
    i++;
}
```

### Break and Continue

Control loop execution with break and continue:
```c
// break - exit loop immediately
int count = 0;
while (count < 10) {
    if (count == 5) {
        break;  // Exit loop when count reaches 5
    }
    printf("%d ", count);
    count++;
}
// Prints: 0 1 2 3 4

// continue - skip to next iteration
int i = 0;
while (i < 10) {
    i++;
    if (i % 2 == 0) {
        continue;  // Skip even numbers
    }
    printf("%d ", i);
}
// Prints: 1 3 5 7 9
```

### scanf in Loops Warning

Check scanf return value to avoid infinite loops on input failure:
```c
// Bad: infinite loop if non-integer entered
int value;
while (value != -1) {
    printf("Enter number: ");
    scanf("%d", &value);  // If user enters "abc", scanf fails repeatedly
}

// Good: check scanf return value
int value;
printf("Enter number (-1 to quit): ");
while (scanf("%d", &value) == 1 && value != -1) {
    printf("You entered: %d\n", value);
    printf("Enter number (-1 to quit): ");
}

// Clear input buffer if scanf fails
if (value != -1) {
    printf("Invalid input\n");
    while (getchar() != '\n');  // Clear buffer
}
```

### Do-While Alternative

Use do-while when loop body must execute at least once:
```c
// while: condition checked first (may not execute)
int x = 10;
while (x < 5) {
    printf("While: never executes\n");
}

// do-while: body executes first, then condition checked
int y = 10;
do {
    printf("Do-while: executes once (y = %d)\n", y);
} while (y < 5);

// Common use: menu loops
int choice;
do {
    printf("1. Option A\n");
    printf("2. Option B\n");
    printf("3. Quit\n");
    printf("Choice: ");
    scanf("%d", &choice);
} while (choice != 3);  // Always show menu at least once
```

### Converting While to For

Any while loop can be rewritten as a for loop:
```c
// While loop
int i = 0;
while (i < 5) {
    printf("%d\n", i);
    i++;
}

// Equivalent for loop
for (int i = 0; i < 5; i++) {
    printf("%d\n", i);
}
```

Choose while when the loop isn't counter-based, for when it is.

---

## Related Topics

- [[For Loops]] - Counter-based iteration
- [[C If Statements]] - Conditions in loop bodies
- [[C Logical Operators]] - Complex loop conditions
- [[User Input]] - Common in validation loops
- [[C Functions]] - Loops within functions
