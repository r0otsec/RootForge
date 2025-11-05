---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C For Loops", "For Loop", "Counter Loop", "Iteration", "Array Traversal"]
---

## Overview

A for loop is a control structure optimized for counter-based iteration, combining initialization, condition checking, and increment/decrement into a single, compact statement. The syntax `for (initialization; condition; update)` executes initialization once before the loop starts, checks the condition before each iteration, and executes the update statement after each iteration completes.

## Why Use It

For loops are the preferred choice when iterating a specific number of times or traversing arrays with index-based access. They reduce errors by keeping all loop control logic in one visible location at the top of the loop, making it immediately clear what controls the iteration. For loops are ubiquitous for array processing, numerical computations, and any task requiring a predictable number of iterations.

The compact syntax prevents common mistakes like forgetting to update the counter variable, which can easily happen with while loops.

## Loop Structure

```
for (initialization; condition; update) {
    // Loop body
}
```

**Execution order:**
1. Initialization executes once (before first iteration)
2. Condition is evaluated (if false, loop terminates)
3. Loop body executes
4. Update statement executes
5. Return to step 2

## Code Example

```c
#include <stdio.h>

int main(void) {
    /* Example 1: Basic for loop - count from 0 to 4 */
    printf("Basic counting:\n");
    for (int i = 0; i < 5; i++) {
        printf("i = %d\n", i);
    }

    /* Example 2: Array processing */
    int numbers[] = {10, 20, 30, 40, 50};
    int arraySize = 5;
    int sum = 0;

    printf("\nArray elements and sum:\n");
    for (int i = 0; i < arraySize; i++) {
        printf("numbers[%d] = %d\n", i, numbers[i]);
        sum += numbers[i];
    }
    printf("Sum: %d\n", sum);

    /* Example 3: Nested loops - multiplication table */
    printf("\nMultiplication table (3x3):\n");
    for (int row = 1; row <= 3; row++) {
        for (int col = 1; col <= 3; col++) {
            printf("%4d", row * col);
        }
        printf("\n");
    }

    /* Example 4: Counting backwards */
    printf("\nCountdown:\n");
    for (int i = 5; i > 0; i--) {
        printf("%d ", i);
    }
    printf("Liftoff!\n");

    return 0;
}
```

### Explanation

Example 1 shows basic syntax counting 0 to 4 - the loop variable `i` is initialized to 0, checked against condition `i < 5`, and incremented after each iteration. Example 2 uses a for loop to traverse an array, using counter `i` as the index to access elements and accumulate the sum - this is the most common array processing pattern. Example 3 demonstrates nested loops creating a multiplication table - the outer loop controls rows, the inner loop controls columns. Example 4 counts backwards using decrement (`i--`).

## Important Considerations

### Loop Variable Scope (C99+)

In C99 and later, variables declared in the initialization are scoped to the loop:
```c
for (int i = 0; i < 5; i++) {
    printf("%d\n", i);
}
// printf("%d\n", i);  // ERROR in C99+: i doesn't exist here

// In older C (C89), declare i before loop:
int i;
for (i = 0; i < 5; i++) {
    printf("%d\n", i);
}
// i still exists here in C89
```

### Equivalence to While Loop

Every for loop can be rewritten as a while loop:
```c
// For loop
for (int i = 0; i < 5; i++) {
    printf("%d\n", i);
}

// Equivalent while loop
int i = 0;           // Initialization
while (i < 5) {      // Condition
    printf("%d\n", i);
    i++;             // Update
}
```

Use for loops when you have clear initialization, condition, and update steps.

### Off-by-One Errors

The most common mistake - arrays of size N have indices 0 to N-1:
```c
int numbers[5] = {10, 20, 30, 40, 50};

// Wrong: accesses index 5 (out of bounds!)
for (int i = 0; i <= 5; i++) {  // <= is wrong
    printf("%d\n", numbers[i]);  // Undefined behavior when i=5
}

// Correct: stops at index 4
for (int i = 0; i < 5; i++) {  // < is correct
    printf("%d\n", numbers[i]);
}
```

Use `<` for exclusive upper bounds (arrays), `<=` for inclusive bounds (counting).

### Avoid Floating-Point Counters

Rounding errors can prevent loop termination:
```c
// Dangerous: floating-point counter
for (double d = 0.0; d != 1.0; d += 0.1) {
    printf("%.15f\n", d);
}
// May never reach exactly 1.0 due to rounding errors!

// Better: use integer counter
for (int i = 0; i < 10; i++) {
    double d = i * 0.1;
    printf("%.15f\n", d);
}
```

### Don't Modify Counter in Loop Body

Modifying the loop counter inside the body is confusing and error-prone:
```c
// Bad: modifying i inside loop
for (int i = 0; i < 10; i++) {
    printf("%d ", i);
    if (i == 5) {
        i = 8;  // Confusing! Hard to follow loop flow
    }
}

// Better: use break or adjust loop condition
for (int i = 0; i < 10; i++) {
    if (i == 6) {
        break;  // Clear intent to exit
    }
    printf("%d ", i);
}
```

### Unsigned Countdown Warning

Unsigned integers wrap around, causing infinite loops:
```c
// Dangerous: infinite loop with unsigned
for (unsigned int i = 5; i >= 0; i--) {
    printf("%u\n", i);
}
// When i becomes 0 and decrements, it wraps to UINT_MAX!

// Solution 1: use signed int
for (int i = 5; i >= 0; i--) {
    printf("%d\n", i);
}

// Solution 2: count up and adjust logic
for (unsigned int i = 0; i <= 5; i++) {
    printf("%u\n", 5 - i);
}
```

### Empty Loop Components

Any component can be empty (but semicolons required):
```c
// All components present
for (int i = 0; i < 5; i++) {
    printf("%d\n", i);
}

// No initialization (i declared earlier)
int i = 0;
for (; i < 5; i++) {
    printf("%d\n", i);
}

// No update (manual control)
for (int j = 0; j < 5;) {
    printf("%d\n", j);
    j += 2;  // Manual update
}

// Infinite loop (no condition)
for (;;) {  // Equivalent to while(1)
    printf("Press Ctrl+C to exit\n");
}
```

### Nested Loop Performance

Nested loops multiply iteration counts:
```c
// Outer loop: 1000 iterations
// Inner loop: 1000 iterations per outer iteration
// Total: 1,000,000 iterations
for (int i = 0; i < 1000; i++) {
    for (int j = 0; j < 1000; j++) {
        // This code runs 1 million times
    }
}
```

Be mindful of performance with deeply nested loops over large ranges.

### Break and Continue

Control loop execution with break and continue:
```c
// break - exit loop immediately
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;  // Exit loop when i reaches 5
    }
    printf("%d ", i);
}
// Prints: 0 1 2 3 4

// continue - skip to next iteration
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue;  // Skip even numbers
    }
    printf("%d ", i);
}
// Prints: 1 3 5 7 9
```

In nested loops, break only exits the innermost loop.

---

## Related Topics

- [[While Loops]] - Condition-based iteration
- [[Variables]] - Loop counter variables
- [[Arithmetic Operators]] - Loop counter updates
- [[C Logical Operators]] - Complex loop conditions
- [[C If Statements]] - Conditional logic in loops
- [[Variable Scope]] - Loop variable lifetime
