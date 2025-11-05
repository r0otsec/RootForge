---
tags:
  - Technologies
  - C-Programming
date: 2025-11-05
aliases: ["C Read Files", "File Reading", "File Input", "fscanf", "fgets", "File I/O"]
---

## Overview

File reading in C involves opening an existing file, reading data from it using various input functions, and closing it properly. Key functions are: fopen() to open in read mode, fscanf() for reading formatted data, fgets() for reading lines safely, and fgetc() for reading character-by-character. After reading, fclose() releases file resources, and you should always check for NULL return from fopen() and EOF condition.

## Why Use It

File reading is essential for loading saved data, processing configuration files, analyzing log files, importing data from other programs, and restoring program state. It enables programs to work with data larger than memory by processing files incrementally and facilitates data exchange between applications.

Without file reading, programs would be isolated—unable to load existing data, process external files, or persist information across executions.

## Code Example

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LINE 256

struct Student {
    char name[50];
    int id;
    float gpa;
};

int main(void) {
    FILE *filePtr = NULL;

    /* Method 1: Read formatted data using fscanf() */
    filePtr = fopen("students.csv", "r");

    if (filePtr == NULL) {
        perror("Error opening students.csv");
        return EXIT_FAILURE;
    }

    char header[MAX_LINE];
    if (fgets(header, MAX_LINE, filePtr) != NULL) {
        printf("CSV Header: %s", header);
    }

    /* Read structured data */
    struct Student students[10];
    int count = 0;

    while (count < 10 &&
           fscanf(filePtr, "%49[^,],%d,%f\n",
                  students[count].name,
                  &students[count].id,
                  &students[count].gpa) == 3) {
        count++;
    }

    fclose(filePtr);

    printf("\nRead %d students from CSV:\n", count);
    for (int i = 0; i < count; i++) {
        printf("  %s (ID: %d, GPA: %.2f)\n",
               students[i].name, students[i].id, students[i].gpa);
    }

    /* Method 2: Read line by line using fgets() */
    filePtr = fopen("students.txt", "r");
    if (filePtr == NULL) {
        perror("Error opening students.txt");
        return EXIT_FAILURE;
    }

    char line[MAX_LINE];
    printf("\nReading students.txt:\n");

    while (fgets(line, MAX_LINE, filePtr) != NULL) {
        line[strcspn(line, "\n")] = '\0';  /* Remove newline */
        printf("  %s\n", line);
    }

    if (ferror(filePtr)) {
        perror("Error reading file");
    }

    fclose(filePtr);

    /* Method 3: Read character by character */
    filePtr = fopen("students.txt", "r");
    if (filePtr == NULL) {
        perror("Error opening file");
        return EXIT_FAILURE;
    }

    int charCount = 0, lineCount = 0;
    int ch;  /* Must be int, not char, to hold EOF */

    while ((ch = fgetc(filePtr)) != EOF) {
        charCount++;
        if (ch == '\n') {
            lineCount++;
        }
    }

    printf("\nFile statistics: %d characters, %d lines\n",
           charCount, lineCount);

    fclose(filePtr);

    return EXIT_SUCCESS;
}
```

### Explanation

The program demonstrates file reading techniques. It opens a CSV file and uses fscanf() with custom format string to parse comma-separated values into struct members, checking return value. It uses fgets() to read a text file line by line safely, checking for EOF and errors. It reads character-by-character using fgetc() to count characters and lines, demonstrating that fgetc() returns int to accommodate EOF.

## Important Considerations

### Always Check for NULL

fopen() returns NULL if file doesn't exist or can't be opened:
```c
FILE *fp = fopen("data.txt", "r");

// WRONG - no error checking
char buffer[100];
fgets(buffer, 100, fp);  // Crashes if fp is NULL!

// CORRECT - check before use
if (fp == NULL) {
    perror("Error opening file");
    return EXIT_FAILURE;
}

char buffer[100];
if (fgets(buffer, 100, fp) != NULL) {
    printf("Read: %s", buffer);
}

fclose(fp);
```

Common reasons fopen() fails:
- File doesn't exist
- Insufficient permissions
- Invalid path
- File locked by another process

### File Mode for Reading

Files must exist for "r" mode:
```c
// "r" - Read mode (file must exist)
FILE *fp1 = fopen("data.txt", "r");
// Returns NULL if file doesn't exist

// "r+" - Read and write mode (file must exist)
FILE *fp2 = fopen("data.txt", "r+");
// Can both read and write, but file must exist

// "rb" - Read binary mode
FILE *fp3 = fopen("image.png", "rb");
// For binary files (images, executables, etc.)

// "r+b" - Read/write binary mode
FILE *fp4 = fopen("data.bin", "r+b");
```

### Understanding Return Values

Different functions return different values:
```c
FILE *fp = fopen("data.txt", "r");

// fgets() returns NULL at EOF or error
char line[100];
while (fgets(line, 100, fp) != NULL) {
    printf("%s", line);
}

// fscanf() returns number of items successfully read
int num;
if (fscanf(fp, "%d", &num) == 1) {
    printf("Read number: %d\n", num);
} else {
    printf("Failed to read number\n");
}

// fgetc() returns EOF (not NULL!) at end or error
int ch;
while ((ch = fgetc(fp)) != EOF) {
    putchar(ch);
}

fclose(fp);
```

### EOF Detection

Use feof() to distinguish EOF from error:
```c
FILE *fp = fopen("data.txt", "r");
char buffer[100];

while (fgets(buffer, 100, fp) != NULL) {
    printf("%s", buffer);
}

// Why did loop end? EOF or error?
if (feof(fp)) {
    printf("Reached end of file\n");
} else if (ferror(fp)) {
    printf("Error reading file\n");
}

fclose(fp);
```

### Buffer Size with fgets()

Always use size parameter to prevent overflow:
```c
char buffer[50];

// SAFE - reads at most 49 characters plus null terminator
fgets(buffer, 50, fp);
// Reads at most sizeof(buffer)-1 characters
// Always null-terminates the string

// SAFER - use sizeof to avoid magic numbers
fgets(buffer, sizeof(buffer), fp);

// DANGEROUS - buffer overflow if line > 49 chars
// gets(buffer);  // NEVER USE gets() - removed from C11!
```

How fgets() works:
```c
char buffer[10];
// File contains: "Hello World\n"

fgets(buffer, 10, fp);
// Reads: "Hello Wor" (9 chars + '\0')
// Buffer: {'H','e','l','l','o',' ','W','o','r','\0'}

// Next call:
fgets(buffer, 10, fp);
// Reads: "ld\n" (3 chars + '\0')
// Buffer: {'l','d','\n','\0'}
```

### char vs int for fgetc()

fgetc() returns int, not char, to accommodate EOF:
```c
// WRONG - char can't hold EOF on some systems
char ch;
while ((ch = fgetc(fp)) != EOF) {  // May infinite loop!
    putchar(ch);
}

// CORRECT - use int
int ch;  // Must be int!
while ((ch = fgetc(fp)) != EOF) {
    putchar(ch);
}
```

Why int is required:
- EOF is typically -1
- On some systems, char is unsigned (0-255)
- Unsigned char can never equal -1
- Result: infinite loop or missed EOF

### fgets() Includes Newline

fgets() keeps the newline character in the buffer:
```c
char line[100];
fgets(line, 100, fp);

// If file line is "Hello\n", buffer contains: "Hello\n\0"
printf("Line: %s", line);  // Prints with newline

// To remove newline:
line[strcspn(line, "\n")] = '\0';
printf("Line: %s\n", line);  // Prints without extra newline

// Or manually:
size_t len = strlen(line);
if (len > 0 && line[len-1] == '\n') {
    line[len-1] = '\0';
}
```

### Infinite Loop Prevention

WRONG way to check for EOF:
```c
// WRONG - can infinite loop on error
while (!feof(fp)) {
    fgets(buffer, 100, fp);
    printf("%s", buffer);
}
// Problem: feof() is true AFTER reading past EOF
// Last line may be printed twice or garbage printed

// CORRECT - check return value
while (fgets(buffer, 100, fp) != NULL) {
    printf("%s", buffer);
}
// Loop exits when fgets() returns NULL (EOF or error)
```

### fscanf() Format Specifiers

fscanf() stops at whitespace by default:
```c
FILE *fp = fopen("data.txt", "r");

// File contains: "John Doe 25"

char first[20], last[20];
int age;

// Reads first name, last name, age (whitespace-separated)
fscanf(fp, "%s %s %d", first, last, &age);
// first = "John", last = "Doe", age = 25

// To read entire line (including spaces):
char name[50];
fscanf(fp, "%49[^\n]", name);  // Read until newline
// name = "John Doe"

// Always specify width limit to prevent overflow:
fscanf(fp, "%49s", name);  // Max 49 chars + null terminator
```

Advanced fscanf() patterns:
```c
// Read CSV format
fscanf(fp, "%49[^,],%d,%f", name, &id, &gpa);
// Reads: "Alice,12345,3.75"

// Skip whitespace and read
fscanf(fp, " %c", &ch);  // Space before %c skips whitespace

// Read until specific character
fscanf(fp, "%49[^:]", buffer);  // Read until ':'
```

### Binary vs Text Mode Mismatch

Reading binary file in text mode corrupts data:
```c
// Binary data written with fwrite()
int numbers[5] = {1, 2, 3, 4, 5};
FILE *fp = fopen("data.bin", "wb");
fwrite(numbers, sizeof(int), 5, fp);
fclose(fp);

// WRONG - reading binary as text
fp = fopen("data.bin", "r");  // Missing 'b'!
// On Windows: \n converted to \r\n, data corrupted

// CORRECT - read binary as binary
fp = fopen("data.bin", "rb");
int buffer[5];
fread(buffer, sizeof(int), 5, fp);
fclose(fp);
```

### Buffer Overflow Prevention

Always specify width limits:
```c
char name[50];

// DANGEROUS - buffer overflow if input > 49 chars
fscanf(fp, "%s", name);

// SAFE - reads max 49 chars
fscanf(fp, "%49s", name);

// SAFER - use sizeof
fscanf(fp, "%49s", name);  // 50-1 for null terminator

// Even safer with fgets + sscanf:
char line[100];
fgets(line, sizeof(line), fp);
sscanf(line, "%49s %d", name, &age);
```

### Validate Input

Real-world files may have unexpected format:
```c
FILE *fp = fopen("data.csv", "r");

int id;
float gpa;
char name[50];

// Check return value to validate format
if (fscanf(fp, "%49[^,],%d,%f", name, &id, &gpa) == 3) {
    printf("Valid: %s, %d, %.2f\n", name, id, gpa);
} else {
    printf("Invalid format\n");
    // Handle error - skip line, log error, etc.
}

// Always check:
// - Return value matches expected
// - Values in valid range
// - No NULL pointers
```

### Reading Unknown File Size

Process files of any size:
```c
FILE *fp = fopen("large.txt", "r");
char line[256];

// Read until EOF
while (fgets(line, sizeof(line), fp) != NULL) {
    // Process each line
    printf("%s", line);
}

// Or for character-by-character:
int ch;
while ((ch = fgetc(fp)) != EOF) {
    putchar(ch);
}

fclose(fp);
```

### Common Reading Pattern

Robust file reading template:
```c
FILE *fp = fopen("data.txt", "r");

if (fp == NULL) {
    perror("Error opening file");
    return EXIT_FAILURE;
}

char line[256];
int line_num = 0;

while (fgets(line, sizeof(line), fp) != NULL) {
    line_num++;

    // Remove newline
    line[strcspn(line, "\n")] = '\0';

    // Process line
    printf("Line %d: %s\n", line_num, line);
}

// Check why loop ended
if (ferror(fp)) {
    fprintf(stderr, "Error reading file at line %d\n", line_num);
}

fclose(fp);
```

---

## Related Topics

- [[Write Files]] - Writing data to files
- [[Structs]] - Reading structured data
- [[Arrays of Structs]] - Loading collections from files
- [[While Loops]] - Reading until EOF
- [[For Loops]] - Processing fixed-size data
- [[C Pointers]] - File pointers and dynamic buffers
- [[C Format Specifiers]] - Understanding fscanf formats
