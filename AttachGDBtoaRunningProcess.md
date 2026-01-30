# Attach GDB to a Running Process
Suppose we are running an infinite process in one terminal:
```c
#include <stdio.h>
#include <unistd.h>

int main(){
    int counter =0;
    while(1){
        printf("counter = %d\n", counter);
        sleep(1);
        counter++;
    }
    return 0;
}
```

We can attach GDB to the process and moniter how it runs in another terminal.
```bash
ps aux | grep prog  # the program's name is called prog.

# Result to get PID
fumeshr+     747  0.0  0.0   2684  1536 pts/0    S+   10:24   0:00 ./prog
fumeshr+     786  0.0  0.0   4092  1920 pts/2    S+   10:25   0:00 grep --color=auto prog

# attach GDB
gdb -p 747

# use the root user if permission is denied
sudo gdb -p 747
```
Once we attaches GDB, the process will stop.


Now we can use common GDB commands to monitor the process:
```bash
(gdb) backtrace
(gdb) print counter
(gdb) layout src
(gdb) detach       # Detach GDB from the process, the process continues
```

