# Debug by using core dump files
## Check current limits
```bash
ulimit -a
```
This shows all resource limits for the current shell.
```bash
core file size          (blocks, -c) 0             # If it is 0, core dumps are disabled.
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 515199
max locked memory       (kbytes, -l) 65536
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1048576
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 515199
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

## Enable core dumps
```bash
ulimit -c unlimited
```
- allow core dumps of any size
- only affects the **current shell**
- must run this before starting ./prog

```bash
core file size          (blocks, -c) unlimited
```

## Run the program (let it crash)
```bash
./prog
```
If the program crashes (e.g. segmentation fault), the OS will create a file like:
```bash
core
core.17321  # The number is usually the PID.
```
## Open the core dump in GDB
```bash
gdb ./prog core.17321
```
Then we can use GDB commands to debug:
```bash
bt
info locals
info args
list
```
