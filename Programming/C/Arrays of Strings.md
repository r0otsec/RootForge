---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Arrays of Strings", "String Arrays", "Multiple Strings", "2D Char Array", "Array of String Pointers"]
---

## Overview

An array of strings in C is implemented as a 2D character array where each row represents one string. The declaration `char names[5][20]` creates storage for 5 strings, each with a maximum length of 19 characters (plus null terminator). Alternatively, you can use an array of pointers to char (`char *strings[]`), where each pointer points to a string literal or dynamically allocated memory.

## Why Use It

Arrays of strings are crucial for handling multiple text items such as command-line arguments, menu options, lists of names, dictionary words, or configuration settings. The 2D array approach provides contiguous storage suitable for fixed-size string collections, while the pointer array approach offers flexibility for variable-length strings.

## Code Example

```c
#include <stdio.h>
#include <string.h>

int main(void) {
    /* Method 1: 2D character array - all strings stored in contiguous memory */
    /* Each string can be up to 19 characters (20 - 1 for null terminator) */
    char days[7][20] = {
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday"
    };

    /* Method 2: Array of pointers to string literals */
    /* Each pointer points to a read-only string in memory */
    char *months[] = {
        "January", "February", "March", "April",
        "May", "June", "July", "August",
        "September", "October", "November", "December"
    };

    /* Accessing strings using array indices */
    printf("First day: %s\n", days[0]);      /* Prints "Monday" */
    printf("Third month: %s\n", months[2]);   /* Prints "March" */

    /* Iterate through array of strings */
    printf("\nAll days of the week:\n");
    for (int i = 0; i < 7; i++) {
        printf("%d. %s\n", i + 1, days[i]);
    }

    /* Modify a string in 2D array (allowed - we own this memory) */
    strcpy(days[0], "Mon");  /* Shortened Monday to Mon */
    printf("\nModified first day: %s\n", days[0]);

    /* CANNOT modify string literals pointed to by months array */
    /* strcpy(months[0], "Jan");  <-- UNDEFINED BEHAVIOR! */

    /* String length operations */
    printf("\nLength of 'Wednesday': %zu characters\n", strlen(days[2]));

    return 0;
}
```

### Explanation

The program demonstrates two methods for storing multiple strings. Method 1 uses a 2D character array where each row is a separate string with its own storage space, allowing modification. Method 2 uses an array of pointers where each pointer references a string literal in read-only memory. The program accesses individual strings, iterates through the array, and demonstrates string modification on the 2D array while noting the danger of modifying string literals.

## Important Considerations

### Memory Differences

Understanding the memory implications of each approach:

**2D Character Array:**
```c
char names[3][10];
// Allocates 30 bytes (3 rows × 10 columns)
// All memory allocated upfront, whether used or not
// Strings are modifiable
```

**Array of Pointers:**
```c
char *names[] = {"Alice", "Bob", "Charlie"};
// Allocates space for 3 pointers + string literals
// On 64-bit system: 24 bytes (3 pointers × 8 bytes) + string storage
// Strings are READ-ONLY (string literals)
```

### String Literals Are Read-Only

Attempting to modify string literals causes undefined behavior:
```c
char *names[] = {"Alice", "Bob"};

// DANGEROUS - undefined behavior!
names[0][0] = 'a';  // Trying to change 'A' to 'a'
strcpy(names[0], "Alicia");  // CRASH or corruption!

// Safe alternative: use modifiable array
char names[2][20] = {"Alice", "Bob"};
names[0][0] = 'a';  // OK - we own this memory
strcpy(names[0], "Alicia");  // OK
```

### Null Terminator Space

Each string needs room for the null terminator:
```c
char names[3][10];
// Can store strings up to 9 visible characters
// names[0] = "ABCDEFGHI" (9 chars + '\0' = 10 bytes) - OK
// names[0] = "ABCDEFGHIJ" (10 chars + '\0' = 11 bytes) - OVERFLOW!
```

Always account for `'\0'` when calculating string size requirements.

### Buffer Overflows

Writing beyond array bounds causes serious security vulnerabilities:
```c
char buffer[5][10];

// Dangerous - no bounds checking
scanf("%s", buffer[0]);  // User inputs 20 characters - OVERFLOW!

// Safer - limit input length
scanf("%9s", buffer[0]);  // At most 9 chars + '\0'

// Best - use fgets
fgets(buffer[0], 10, stdin);  // Reads at most 9 chars + '\0'
```

### When to Use Each Method

**Use 2D Character Array when:**
- Strings need to be modified
- All strings have similar lengths
- Want contiguous memory (cache-friendly)
- Need to pass entire array to functions

**Use Pointer Array when:**
- Strings are literals or constants
- Strings have varying lengths (saves memory)
- Need flexibility to point to different locations
- Working with string constants

### Function Parameters

Different approaches for passing to functions:

**Passing 2D Array:**
```c
// Must specify column size
void printStrings(char strings[][20], int count) {
    for (int i = 0; i < count; i++) {
        printf("%s\n", strings[i]);
    }
}

char days[7][20] = {"Monday", "Tuesday", ...};
printStrings(days, 7);
```

**Passing Pointer Array:**
```c
// More flexible - no size restriction
void printStrings(char *strings[], int count) {
    for (int i = 0; i < count; i++) {
        printf("%s\n", strings[i]);
    }
}

char *months[] = {"January", "February", ...};
printStrings(months, 12);
```

### Searching and Sorting

Common operations on string arrays:
```c
#include <string.h>

// Find string in array
int findString(char *strings[], int count, const char *target) {
    for (int i = 0; i < count; i++) {
        if (strcmp(strings[i], target) == 0) {
            return i;  // Found at index i
        }
    }
    return -1;  // Not found
}

// Sort strings alphabetically (bubble sort)
void sortStrings(char strings[][50], int count) {
    char temp[50];
    for (int i = 0; i < count - 1; i++) {
        for (int j = 0; j < count - i - 1; j++) {
            if (strcmp(strings[j], strings[j + 1]) > 0) {
                strcpy(temp, strings[j]);
                strcpy(strings[j], strings[j + 1]);
                strcpy(strings[j + 1], temp);
            }
        }
    }
}
```

### Command-Line Arguments

The `main()` function's `argv` parameter is an array of strings:
```c
int main(int argc, char *argv[]) {
    printf("Program name: %s\n", argv[0]);

    printf("Arguments:\n");
    for (int i = 1; i < argc; i++) {
        printf("  argv[%d] = %s\n", i, argv[i]);
    }

    return 0;
}
```

### Dynamic Allocation Alternative

For maximum flexibility, allocate memory dynamically:
```c
#include <stdlib.h>

char **names = malloc(5 * sizeof(char*));  // Array of 5 pointers
for (int i = 0; i < 5; i++) {
    names[i] = malloc(50 * sizeof(char));  // Each string up to 49 chars
}

// Use the strings
strcpy(names[0], "Alice");

// Free memory when done
for (int i = 0; i < 5; i++) {
    free(names[i]);
}
free(names);
```

---

## Related Topics

- [[2D Arrays]] - Understanding two-dimensional arrays
- [[Arrays]] - Fundamental array concepts
- [[User Input]] - Reading strings from keyboard
- [[For Loops]] - Iterating through string arrays
- [[C Functions]] - Passing string arrays to functions
