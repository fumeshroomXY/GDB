# `-g`
Tells the compiler to **embed debug information** into the binary. Binary gets larger. 

This info maps:
- machine code ↔ source lines
- registers ↔ variables
- stack frames ↔ functions


But it does **NOT change program behavior** and **NOT affect runtime performance**.

## What debug info enables
With `-g`, tools like `gdb` can:
- show source code
- print variable values
- step line-by-line
- show function names instead of raw addresses

```c
// without -g
0x0000000000401136

// with -g
main at main.c:7
```

### Example: 
```c
#include <stdio.h>

int main(void) {
    int x = 42;
    printf("%d\n", x);
    return 0;
}

// Compile:
gcc main.c -g -o main

// Debug:
gdb ./main

// Inside gdb:
break main
run
print x
```
You’ll see `x = 42`. Without `-g` → x is “optimized out” or invisible.


# `-O0`
```c
gcc main.c -O2 -g -o main
```
No optimization for clean debugging.

# `-O2`
```c
gcc main.c -O2 -g -o main
```
- variables may be optimized away
- stepping may “jump around”
- some lines execute out of order



# `-Wall`
Turn on most common warnings.  
It’s **not all warnings**, but it enables the important ones:
- unused variables
- missing return statements
- implicit function declarations
- suspicious comparisons
- uninitialized variables

## Example: 
```c
int main(void) {
    int x;
    return 0;
}

gcc -Wall main.c

warning: unused variable ‘x’
```

# `-Wconversion`
Warn about implicit type conversions.
## Example:
```c
int main(void) {
    int x = 300;
    char c = x;
    return 0;
}

warning: conversion from ‘int’ to ‘char’ may change value
```

# `-Werror`
Warnings are errors. Any warning → **compile fails**

## Example:
```c
gcc -Wall -Werror main.c
error: unused variable ‘x’ [-Werror=unused-variable]
```


# `-DDEBUG`
Define a macro named DEBUG at compile time.
```c
#define DEBUG 1  // Equivalent to writing at the top of your code
```

## Example: 
```c
#include <stdio.h>

int main(void) {
#ifdef DEBUG
    printf("Debug mode enabled\n");
#endif
    printf("Hello\n");
    return 0;
}

// Compile with -DDEBUG:
gcc main.c -DDEBUG -o main

// Output:
Debug mode enabled
Hello

// Compile without -DDEBUG:
gcc main.c -o main

// Output:
Hello
```

- `-DDEBUG` → logs enabled
- no `-DDEBUG` → compiled out completely (zero runtime cost)

## Why this is better than `if (debug)`
```c
if (debug) {
    printf("...");
}
```

With `#ifdef DEBUG`:
- code does **not exist** in the final binary
- optimizer never sees it
- no extra branches



