---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Arrays and User Input", "Array Input", "Reading Arrays", "Input Validation", "scanf Array"]
---

## Overview

Reading user input into arrays combines scanf/input functions with array indexing and loops. The process involves iterating through array indices and storing each input value at the corresponding position. Critical considerations include validating input (checking scanf's return value), preventing buffer overflows, and handling input errors gracefully.

## Why Use It

Interactive programs often need to collect multiple related values from users - grades for calculation, coordinates for drawing, menu selections, etc. Arrays provide structured storage for this data, while loops make collection efficient. Proper input validation is crucial for program robustness.

Without arrays, collecting multiple user inputs would require individual variables for each value, making the code unmaintainable and preventing flexible data processing.

## Code Example

```c
#include <stdio.h>

int main(void) {
    const int MAX_STUDENTS = 5;
    int grades[MAX_STUDENTS];
    int count = 0;  // Track how many valid grades entered

    printf("Enter up to %d student grades (or -1 to stop early):\n", MAX_STUDENTS);

    // Loop to read grades into array
    for (int i = 0; i < MAX_STUDENTS; i++) {
        printf("Grade %d: ", i + 1);

        // Check if scanf successfully read an integer
        if (scanf("%d", &grades[i]) != 1) {
            // Input was not an integer - handle error
            printf("Invalid input! Please enter a number.\n");

            // Clear the input buffer to prevent infinite loop
            int c;
            while ((c = getchar()) != '\n' && c != EOF) {
                // Discard characters until newline
            }

            i--;  // Retry this index
            continue;
        }

        // Allow early termination
        if (grades[i] == -1) {
            printf("Stopping input early.\n");
            break;
        }

        // Validate grade range
        if (grades[i] < 0 || grades[i] > 100) {
            printf("Grade must be between 0 and 100. Try again.\n");
            i--;  // Retry this index
            continue;
        }

        count++;  // Successfully added a valid grade
    }

    // Process the collected data
    if (count == 0) {
        printf("\nNo valid grades entered.\n");
        return 0;
    }

    printf("\nYou entered %d grade(s):\n", count);
    int sum = 0;
    for (int i = 0; i < count; i++) {
        printf("  Student %d: %d\n", i + 1, grades[i]);
        sum += grades[i];
    }

    double average = (double)sum / count;
    printf("\nAverage grade: %.2f\n", average);

    return 0;
}
```

### Explanation

This program demonstrates robust array input handling by collecting student grades with comprehensive validation. It checks: (1) Is input a valid integer? (2) Is it the sentinel value -1? (3) Is it within valid range (0-100)? If input is invalid, the program clears the input buffer using getchar() to prevent infinite loops.

The separate `count` variable tracks how many valid entries were added, allowing the program to handle early termination correctly.

## Important Considerations

### Always Check scanf Return Value

Ignoring scanf's return value leads to undefined behavior and hard-to-debug errors:
```c
// Bad: no error checking
int numbers[5];
for (int i = 0; i < 5; i++) {
    scanf("%d", &numbers[i]);  // What if user enters "abc"?
}

// Good: check return value
int numbers[5];
for (int i = 0; i < 5; i++) {
    printf("Enter number %d: ", i + 1);

    if (scanf("%d", &numbers[i]) != 1) {
        printf("Invalid input!\n");
        // Handle error (clear buffer, retry, etc.)
        while (getchar() != '\n');  // Clear buffer
        i--;  // Retry this index
    }
}
```

scanf returns the number of successfully read items, so `scanf("%d", &var)` returns 1 on success.

### Buffer Clearing Essential

When scanf fails, invalid input remains in the buffer, causing infinite loops:
```c
// Without buffer clearing: INFINITE LOOP
int value;
while (scanf("%d", &value) != 1) {
    printf("Invalid input!\n");
    // Input still in buffer - scanf fails again immediately!
}

// With buffer clearing: CORRECT
int value;
while (scanf("%d", &value) != 1) {
    printf("Invalid input!\n");

    // Clear input buffer
    int c;
    while ((c = getchar()) != '\n' && c != EOF) {
        // Discard characters
    }
}
```

Always clear the buffer after scanf fails.

### Bounds Checking Prevents Overflows

Never let user input determine array access without validation:
```c
// Dangerous: user can cause buffer overflow
int arr[10];
int index;
printf("Enter index: ");
scanf("%d", &index);
arr[index] = 100;  // What if index is 1000? Or -5?

// Safe: validate bounds
int arr[10];
int index;
printf("Enter index (0-9): ");

if (scanf("%d", &index) == 1 && index >= 0 && index < 10) {
    arr[index] = 100;
} else {
    printf("Invalid index!\n");
    while (getchar() != '\n');  // Clear buffer
}
```

### Separate Counter from Index

Track valid entries independently for early termination:
```c
const int MAX_SIZE = 100;
int data[MAX_SIZE];
int count = 0;  // Separate counter

// Fill array until user enters -1 or array is full
for (int i = 0; i < MAX_SIZE; i++) {
    printf("Enter value (or -1 to stop): ");

    if (scanf("%d", &data[i]) != 1) {
        printf("Invalid input!\n");
        while (getchar() != '\n');
        i--;
        continue;
    }

    if (data[i] == -1) {
        break;  // Early exit
    }

    count++;  // Track actual number of entries
}

// Process only valid entries (0 to count-1)
for (int i = 0; i < count; i++) {
    printf("%d ", data[i]);
}
```

### Handle EOF Properly

Input from files may hit end-of-file:
```c
int numbers[100];
int count = 0;

while (count < 100 && scanf("%d", &numbers[count]) == 1) {
    count++;
}

if (feof(stdin)) {
    printf("Reached end of file\n");
} else if (ferror(stdin)) {
    printf("Input error occurred\n");
} else {
    printf("Array filled\n");
}

printf("Read %d numbers\n", count);
```

### Never Use gets()

gets() is dangerous and removed from C11 - use fgets() instead:
```c
// NEVER DO THIS:
char name[50];
gets(name);  // Buffer overflow if input > 49 chars!

// Use fgets instead:
char name[50];
printf("Enter name: ");
if (fgets(name, sizeof(name), stdin) != NULL) {
    // Remove trailing newline if present
    size_t len = strlen(name);
    if (len > 0 && name[len-1] == '\n') {
        name[len-1] = '\0';
    }
    printf("Hello, %s!\n", name);
}
```

### Common Input Patterns

**Reading integers with validation:**
```c
int readInt(const char *prompt, int min, int max) {
    int value;
    while (1) {
        printf("%s", prompt);

        if (scanf("%d", &value) != 1) {
            printf("Invalid input!\n");
            while (getchar() != '\n');
            continue;
        }

        if (value < min || value > max) {
            printf("Must be between %d and %d\n", min, max);
            continue;
        }

        return value;
    }
}

// Usage
int scores[5];
for (int i = 0; i < 5; i++) {
    scores[i] = readInt("Enter score: ", 0, 100);
}
```

**Reading until sentinel:**
```c
int data[100];
int count = 0;

printf("Enter numbers (0 to stop):\n");
while (count < 100) {
    int value;
    if (scanf("%d", &value) != 1) {
        printf("Invalid input!\n");
        while (getchar() != '\n');
        continue;
    }

    if (value == 0) break;

    data[count++] = value;
}
```

**Reading strings into array:**
```c
char names[5][50];  // Array of 5 strings, each max 49 chars

for (int i = 0; i < 5; i++) {
    printf("Enter name %d: ", i + 1);

    if (fgets(names[i], sizeof(names[i]), stdin) != NULL) {
        // Remove newline
        size_t len = strlen(names[i]);
        if (len > 0 && names[i][len-1] == '\n') {
            names[i][len-1] = '\0';
        }
    }
}
```

---

## Related Topics

- [[Arrays]] - Array fundamentals
- [[User Input]] - Basic input operations
- [[For Loops]] - Iterating to collect input
- [[While Loops]] - Input validation loops
- [[Break and Continue]] - Early exit and retry patterns
- [[C If Statements]] - Input validation conditions
