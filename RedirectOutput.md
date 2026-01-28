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
