---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Enums", "Enumeration", "Named Constants", "Enum Types", "Enumerators"]
---

## Overview

An enumeration (enum) in C is a user-defined type consisting of a set of named integer constants, providing a way to assign meaningful names to related constant values. By default, the first enumerator has value 0, and each subsequent enumerator increments by 1, though explicit values can be assigned. The syntax is `enum tag_name { ENUMERATOR1, ENUMERATOR2, ... };`

## Why Use It

Enums make code self-documenting and more maintainable by replacing magic numbers with descriptive names, improving readability and reducing errors. They're essential for representing states (like file modes, connection states, game states), options, error codes, and any fixed set of related constants.

## Code Example

```c
#include <stdio.h>

/* Basic enum - days of the week */
enum Weekday {
    SUNDAY,      // 0
    MONDAY,      // 1
    TUESDAY,     // 2
    WEDNESDAY,   // 3
    THURSDAY,    // 4
    FRIDAY,      // 5
    SATURDAY     // 6
};

/* Enum with explicit values - useful for bit flags */
enum FileMode {
    READ = 1,      // 0001 in binary
    WRITE = 2,     // 0010 in binary
    EXECUTE = 4    // 0100 in binary
};

/* Using typedef with enum for cleaner declarations */
typedef enum {
    STATE_IDLE,
    STATE_RUNNING,
    STATE_PAUSED,
    STATE_STOPPED
} ProgramState;

const char* getWeekdayName(enum Weekday day) {
    switch (day) {
        case SUNDAY:    return "Sunday";
        case MONDAY:    return "Monday";
        case TUESDAY:   return "Tuesday";
        case WEDNESDAY: return "Wednesday";
        case THURSDAY:  return "Thursday";
        case FRIDAY:    return "Friday";
        case SATURDAY:  return "Saturday";
        default:        return "Invalid day";
    }
}

int main(void) {
    enum Weekday today = WEDNESDAY;
    ProgramState state = STATE_RUNNING;

    printf("Today is: %s (value: %d)\n", getWeekdayName(today), today);

    /* Using enums in switch statements */
    switch (today) {
        case MONDAY:
        case TUESDAY:
        case WEDNESDAY:
        case THURSDAY:
        case FRIDAY:
            printf("It's a weekday - time to work!\n");
            break;
        case SATURDAY:
        case SUNDAY:
            printf("It's the weekend!\n");
            break;
    }

    /* Bit flags with enums */
    enum FileMode permissions = READ | WRITE;
    if (permissions & READ) {
        printf("File is readable\n");
    }
    if (permissions & WRITE) {
        printf("File is writable\n");
    }
    if (!(permissions & EXECUTE)) {
        printf("File is not executable\n");
    }

    /* Program state management */
    printf("\nProgram state: ");
    switch (state) {
        case STATE_IDLE:    printf("Idle\n"); break;
        case STATE_RUNNING: printf("Running\n"); break;
        case STATE_PAUSED:  printf("Paused\n"); break;
        case STATE_STOPPED: printf("Stopped\n"); break;
    }

    return 0;
}
```

### Explanation

The program demonstrates comprehensive enum usage. It defines `Weekday` with default sequential values, `FileMode` with explicit power-of-2 values for bit flags, and `ProgramState` using typedef. The program shows how to declare enum variables, use them in switch statements, combine enum values as bit flags using bitwise operators, and check individual flags.

## Important Considerations

### Not Type-Safe

C enums provide limited type safety - you can assign any integer to an enum variable:
```c
enum Color { RED, GREEN, BLUE };

enum Color c = RED;    // OK
c = 100;               // Legal but semantically wrong!
c = GREEN + BLUE;      // Legal but probably meaningless

// No warnings from compiler!
```

In C++, this is more restricted, but in C, enums are just integers with named constants.

### Global Scope

Enumerators share namespace with other identifiers, which can cause name collisions:
```c
enum Status { ACTIVE, INACTIVE };
enum State { ACTIVE, WAITING };  // Error: ACTIVE redefined!

// Solution: Use prefixes
enum Status { STATUS_ACTIVE, STATUS_INACTIVE };
enum State { STATE_ACTIVE, STATE_WAITING };
```

### Underlying Type

Enumerators are of type `int` by default:
```c
enum Values { A, B, C };

printf("Size of enum: %zu\n", sizeof(enum Values));  // Usually 4 bytes
printf("Size of int: %zu\n", sizeof(int));          // Usually 4 bytes
```

The actual size is implementation-defined but typically matches `int`.

### Bit Flag Patterns

For combinable flags, use powers of 2 (1, 2, 4, 8, 16, ...):
```c
enum Permissions {
    PERM_READ    = 1,      // 0001
    PERM_WRITE   = 2,      // 0010
    PERM_EXECUTE = 4,      // 0100
    PERM_DELETE  = 8       // 1000
};

// Combine flags with bitwise OR
enum Permissions userPerms = PERM_READ | PERM_WRITE;

// Check flags with bitwise AND
if (userPerms & PERM_READ) {
    printf("Can read\n");
}

// Remove flag with bitwise AND-NOT
userPerms &= ~PERM_WRITE;  // Remove write permission

// Toggle flag with bitwise XOR
userPerms ^= PERM_EXECUTE;  // Toggle execute permission
```

### Enum vs #define

Enums are preferred over `#define` for several reasons:
```c
// Using #define (old style)
#define RED 0
#define GREEN 1
#define BLUE 2

// Using enum (preferred)
enum Color { RED, GREEN, BLUE };
```

**Why enums are better:**
1. **Debugger support**: Enums appear with names in debuggers
2. **Scope**: Enums respect scope rules, defines do not
3. **Type information**: Compiler knows enum is a type
4. **Sequential values**: Enums auto-increment

### Explicit Value Assignment

You can assign specific values to enumerators:
```c
enum HttpStatus {
    HTTP_OK = 200,
    HTTP_CREATED = 201,
    HTTP_BAD_REQUEST = 400,
    HTTP_UNAUTHORIZED = 401,
    HTTP_NOT_FOUND = 404,
    HTTP_SERVER_ERROR = 500
};

enum Priority {
    LOW = 1,
    MEDIUM = 5,
    HIGH = 10,
    CRITICAL = 100
};
```

Subsequent values continue incrementing from the last assigned value:
```c
enum Values {
    A = 10,   // 10
    B,        // 11
    C,        // 12
    D = 20,   // 20
    E,        // 21
    F         // 22
};
```

### Using Enums in Switch Statements

Enums work perfectly with switch statements and enable compiler warnings:
```c
enum Status { PENDING, APPROVED, REJECTED, CANCELLED };

void handleStatus(enum Status s) {
    switch (s) {
        case PENDING:
            printf("Waiting for approval\n");
            break;
        case APPROVED:
            printf("Approved!\n");
            break;
        case REJECTED:
            printf("Rejected\n");
            break;
        // Deliberately omitting CANCELLED
    }
    // Some compilers warn: "enumeration value not handled in switch"
}
```

Use `-Wswitch` compiler flag to enable these warnings.

### Anonymous Enums

Enums without a tag name, used just for constants:
```c
enum {
    BUFFER_SIZE = 1024,
    MAX_USERS = 100,
    TIMEOUT_SECONDS = 30
};

char buffer[BUFFER_SIZE];  // Using the constant
```

### Typedef with Enums

Two common patterns:

**Separate typedef:**
```c
enum Color { RED, GREEN, BLUE };
typedef enum Color Color;

Color c = RED;  // Can omit 'enum' keyword
```

**Combined typedef (more common):**
```c
typedef enum {
    RED,
    GREEN,
    BLUE
} Color;

Color c = RED;  // Clean syntax
```

### Best Practices

1. **Use ALL_CAPS or Prefixes**: Make enumerators stand out
```c
enum Color { COLOR_RED, COLOR_GREEN, COLOR_BLUE };
```

2. **Document Range and Meaning**:
```c
/* HTTP status codes as per RFC 7231 */
enum HttpStatus {
    HTTP_OK = 200,
    HTTP_NOT_FOUND = 404
};
```

3. **Group Related Constants**: Don't mix unrelated values
```c
// Good - related concepts
enum FileMode { READ, WRITE, APPEND };

// Bad - unrelated concepts
enum MixedBag { READ, BLUE, TUESDAY };
```

4. **Use Enums for State Machines**:
```c
typedef enum {
    STATE_INIT,
    STATE_CONNECTING,
    STATE_CONNECTED,
    STATE_ERROR,
    STATE_CLOSED
} ConnectionState;

ConnectionState state = STATE_INIT;
```

5. **Consider Sentinel Values**:
```c
enum Color {
    RED,
    GREEN,
    BLUE,
    COLOR_COUNT  // Number of colors, useful for loops
};

for (int i = 0; i < COLOR_COUNT; i++) {
    // Process all colors
}
```

### Iterating Through Enums

Since enums are integers, you can iterate (if values are sequential):
```c
enum Day { MON, TUE, WED, THU, FRI, SAT, SUN, DAY_COUNT };

for (enum Day d = MON; d < DAY_COUNT; d++) {
    printf("Day %d\n", d);
}
```

### Common Use Cases

**Error Codes:**
```c
typedef enum {
    ERR_NONE = 0,
    ERR_FILE_NOT_FOUND,
    ERR_PERMISSION_DENIED,
    ERR_OUT_OF_MEMORY,
    ERR_INVALID_ARGUMENT
} ErrorCode;
```

**Boolean Alternative (pre-C99):**
```c
enum { FALSE, TRUE };  // Before stdbool.h existed
```

**Menu Options:**
```c
enum MenuOption {
    MENU_NEW = 1,
    MENU_OPEN,
    MENU_SAVE,
    MENU_EXIT
};
```

---

## Related Topics

- [[Switch Statements]] - Perfect pairing with enums
- [[Typedef]] - Creating cleaner enum types
- [[Variables]] - Understanding constant values
- [[C Logical Operators]] - Working with bit flag enums
- [[C If Statements]] - Conditional logic with enum values
