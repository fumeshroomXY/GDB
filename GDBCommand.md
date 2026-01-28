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


## Conditional breakpoints
```bash
# Condition is written in C/C++ syntax.
break main if argc > 1
break foo if x == 42
break file.c:120 if i >= 100
```

Example:
```c
for (int i = 0; i < 1000000; i++) {
    work(i);
}

// GDB:
break work if i == 999999
```
- Only stops once. Huge time saver.

# `tbreak`
- Sets a **temporary breakpoint**
- Automatically deleted after hit once

# `info breakpoint` (or `info break`)
- Lists all breakpoints
- Shows location, hit count, enabled/disabled state

Output Example: 
```text
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000000000401136 in foo at test.c:1
        stop only if x > 10
        breakpoint already hit 2 times
2       breakpoint     keep y   0x000000000040114a in bar at test.c:5
        stop only if x > 10
3       breakpoint     del  y   0x0000000000401120 in main at test.c:8
```

## `enable` and `disable`
- Enable or Disable ONE breakpoint

```bash
disable 1  # Num == 1

# Result
Enb = n

# Disable MULTIPLE breakpoints
disable 1 2

# Disable ALL breakpoints
disable      
```
## `delete`
- Remove breakpoint permanently

```bash
delete 2   # Num == 2
```

## `save` and `source`
- Save current breakpoints 

```bash
# Writes only breakpoint commands into a text file.
save breakpoints breakpoints.gdb

# breakpoints.gdb
break foo
break file.c:42
break bar if x > 10
watch x
```

- Load them later

```bash
source breakpoints.gdb
```


# `watch`
- Stop when a value is written

```c
// GDB:
watch x

int x = 0;
x = 1;   // ← triggers watch x, GDB stops right at the write.
```

# `rwatch`
- Stop when a value is read

```c
// GDB:
rwatch x

printf("%d\n", x);  // ← triggers rwatch x, stops execution when x is read.
```

# `continue` (or `c`)
- Resume execution
- Stops at the **next breakpoint**

# `advance`
- Continues execution
- Stops when execution reaches `LOCATION`
- Does **not stop at other breakpoints** on the way (unless unavoidable)

```bash
advance 120          # run until line 120
advance foo          # run until function foo
advance file.c:88
```

# `until`
## Run until you leave the current block / loop
Stops when: 
- The current loop iteration finishes, or
- Control reaches a line **greater than the current one**


Perfect for **skipping the rest of a loop body**.

Example 1: 
```c
10  for (int i = 0; i < 3; i++) {
11      printf("A\n");
12      printf("B\n");   // ← you are here, run until
13      printf("C\n");
14  }
15  printf("done\n");
```
- Line 13 → greater than 12? ❌ (still inside loop, but same iteration)
- Line 14 → greater than 12? ❌ (loop end, still same iteration)
- Loop jumps back to line 10 → line number decreases ❌
- Loop runs again…
- Eventually loop exits → execution reaches line 15
- 15 > 12 ✅ → **STOP**

Example 2:
```c
20  if (x > 0) {
21      foo();
22      bar();    // ← you are here
23      baz();
24  }
25  after();
```
- line 23 → still inside block ❌
- line 24 → end of block ❌
- line 25 → 25 > 22 ✅ → **STOP**

## Run until a specific line
```bash
until 150
```
Stops when execution reaches line 150.

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

# `display` and `undisplay`
- Automatically printing expressions **every time the program stops(breakpoint, step, watchpoint)**.
- `print x` → show once
- `display x` → show every stop
- `undisplay` → stop showing it

Example: 
```bash
display x

# Result, 1 == display ID
1: x = 42

# Display expressions
display i
display *ptr
display arr[3]
display s.field
display x + y

# List current displays
info display

# Result
Auto-display expressions:
1: x
2: i
3: *ptr

# Remove ONE display
undisplay 1
```

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


# `gdb --silent`
- Suppresses the startup banner and most messages
- Drops you straight into the GDB prompt
- Less noise when scripting or when you already know what you’re doing

# `gdb --tui`
- Launches **Text User Interface** mode
- Splits the terminal into panes:
  - Source code
  - Assembly (optional)
  - Registers (optional)
  - GDB command windowc
 
# `layout`
- Switching layouts via commands

```bash
layout src     # source only
layout asm     # assembly only
layout regs    # registers
```

# `focus`
- Switch focus between windows
```bash
focus cmd  # Command window
focus src  # Source window
```

# `Ctrl + L`
- Redraws the entire screen
- Works in:
  - --tui mode
  - normal terminal mode
  - after `printf` spam or window resize
 


