---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Switch Statements", "Switch Statement", "Case Statement", "Multi-way Branch"]
---

## Overview

A switch statement is a multi-way branch control structure that selects one of many code blocks to execute based on the value of an integer expression. It evaluates an expression once, then compares the result against a list of constant case values, jumping directly to the matching case label. Switch statements provide a more efficient and readable alternative to long if-else chains.

## Why Use It

Switch statements make code more readable and maintainable when dealing with multiple discrete values, particularly for menu systems, state machines, and command processing. They can be more efficient than if-else chains because compilers often optimize switches into jump tables.

When you have many conditions testing the same variable against different constant values, a switch statement is cleaner and more performant than a series of if-else statements.

## Code Example

```c
#include <stdio.h>

int main(void) {
    char grade;

    printf("Enter your grade (A, B, C, D, F): ");
    scanf(" %c", &grade);  /* Space before %c consumes whitespace */

    /* Basic switch with break statements */
    switch (grade) {
        case 'A':
            printf("Excellent! Score: 90-100\n");
            break;  /* Prevents fall-through to next case */

        case 'B':
            printf("Good! Score: 80-89\n");
            break;

        case 'C':
            printf("Average. Score: 70-79\n");
            break;

        case 'D':
            printf("Below average. Score: 60-69\n");
            break;

        case 'F':
            printf("Failing. Score: below 60\n");
            break;

        default:  /* Executed if no case matches */
            printf("Invalid grade entered.\n");
            break;
    }

    return 0;
}
```

### Explanation

The program evaluates a letter grade and prints the corresponding performance level, using break statements to prevent fall-through execution. Each case represents a distinct grade value, and if none match, the default case handles invalid input.

The space before `%c` in scanf is important - it tells scanf to skip any whitespace characters (like newlines) left in the input buffer from previous input operations.

## Important Considerations

### Integer Types Only

Switch expressions must be integer types (char, int, long, enums). You cannot use strings, floats, or other non-integer types:
```c
// This does NOT work:
switch (name) {  // Error: strings not allowed
    case "Alice": ...
}
```

### Constant Case Values

Each case must be a compile-time constant. Variables and function calls are not allowed:
```c
int x = 5;
switch (value) {
    case x:  // Error: x is not a constant
    case getValue():  // Error: function call not allowed
}
```

### Fall-Through Behavior

Without break, execution continues to the next case regardless of whether it matches. This is sometimes intentional but often a source of bugs:
```c
switch (grade) {
    case 'A':
    case 'B':
        printf("Above average\n");
        break;  // Both A and B execute this
    case 'C':
        printf("Average\n");
        // No break - falls through to D!
    case 'D':
        printf("Needs improvement\n");
        break;
}
```

### Break Statement Requirement

Break is not automatic in C like it is in some other languages. You must explicitly add it to prevent fall-through.

### Scope Issues

Variables declared in one case are visible in subsequent cases, which can cause unexpected behavior:
```c
switch (value) {
    case 1:
        int x = 10;  // Declared here
        break;
    case 2:
        x = 20;  // Still in scope, but may not be initialized
        break;
}
```

Wrap cases in braces `{}` if you need local variables.

---

## Related Topics

- [[C If Statements]] - Alternative conditional structure
- [[Variables]] - Understanding the switch expression
- [[User Input]] - Getting values to switch on
- [[C Nested If Statements]] - When switch isn't suitable
- [[C Logical Operators]] - Combining conditions in if statements
