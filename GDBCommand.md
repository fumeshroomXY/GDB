# `start`
- Compiles (if needed) and runs the program
- Automatically stops at the **first line of main()**

# `run` (or `r`)
- Runs the program from the beginning
- Restarts it if it already ran before

# `next` (or `n`)
- Executes the current line
- Does **NOT enter functions**
- Treats function calls as a single line.

# `step` (or `s`)
- Executes the current line
- Steps **INTO function calls**
- Goes line-by-line inside the function.

# `list` (or `l`)
- Shows source code around the current line

```bash
list           # around current line
list 20        # starting at line 20
list main      # function main
```

# `break` (or `b`)
- Sets a breakpoint
```c
break main
break foo
break file.c:42
break 42 // now in the file.c
```

# `tbreak`
- Sets a **temporary breakpoint**
- Automatically deleted after hit once

# `continue` (or `c`)
- Resume execution
- Stops at the **next breakpoint**

# `print`(or `p`)
- Inspect values in GDB

```bash
print x + 1
print a[i]
print *ptr
print ptr->field
print arr[3]
print my_struct
print my_struct.field
print *ptr   # 💥 segmentation fault if ptr == NULL
```

# Redirect the debugged program’s output
This affects what your program prints (`printf`, `puts`, etc.).

## Redirect to a file (most common)
```bash
# inside GDB (recommended)
run > output.txt
```

## Redirect program output to another terminal
```bash
# Open another terminal and find its tty
tty

# Example result
/dev/pts/3

# Inside GDB
tty /dev/pts/3
run
```
- Program output → other terminal
- GDB prompt → current terminal

This is **extremely useful** for interactive / noisy programs.


# `whatis`
- Shows the **type** of a variable or expression

# `ptype`
- Shows the **full type definition**
- Very useful for structs and typedefs
```bash
ptype my_struct
```

# `info scope`
- Shows variables visible in the **current scope**
- Includes locals, globals, statics

# `info breakpoint` (or `info break`)
- Lists all breakpoints
- Shows location, hit count, enabled/disabled state
