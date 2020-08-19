# GNU Debugger (GDB) Reference

## Compiling for use with GDB

### Debugging Unoptimized Code

```c
gcc -g -Og main.c
gdb a.out
```

### Debugging Optimized Code

```c
gcc -g -O3 main.c
gdb --args a.out arg1 arg2
```

## GDB Shell Basics

- Getting to the Shell
  - Ctrl+C pauses execution and escapes to the shell
  - `continue` resumes program execution
  - `run <args>` reruns the program
- Figuring out commands
  - `help` browses documentation.
  - `apropos <search_term>` searches
  - Hitting tab after typing a partial command or argument expands to a list of possible commands or arguments
- Saving keystrokes
  - Hitting enter re-runs the last command
  - GDB only requires only as many characters as are needed to uniquely identify a command.
  - This reference shows the terse forms of commands

## Exploring the System

### Exploring Globals

- `i va <regex>` - list global and static variables
- `i fu <regex>` - lists functions

### Inspecting State

- `p <variable_name>` - get value of variable
- `p array[0]` - show element in array (or array decayed to pointer)
- `p my_ptr` - shows address pointer points to
- `p *my_ptr` - shows contents of the address pointed to
- `p *my_ptr@len` - show array that has decayed into a pointer

So for this example:

```c
size_t my_array_len = 10;
int *my_array = (int *) malloc(my_array_len * sizeof(int));
```

You would enter:

```
p *my_array@10
```

### Inspecting Types

- `wha <variable_name>` - get type of variable
- `pt <type_name>` - inspect layout of complex types like structs

Note: To view structs cleanly, run `set print pretty`

## Working with Breakpoints

### Setting and Clearing Breakpoints

- `b` - List Breakpoints
- `b <function_name>|<filename:line_number>|*<address>` - Set breakpoints
- `cle <function_name>|<filename:line_number>|*<address>` - Clear breakpoint

Examples

- `b *0x7ffff70871c0`
- `b printf`
- `b printf.c:20`

### Inspecting the Stack

- `bt` - Display a stack trace
- `fr <n>` - Select a frame in the stack trace
- `i frame` - Display info on the selected frame
- `i args` - Display arguments to the selected frame
- `i lo` - Display local variables in the selected frame
- `i reg` - Display registers as of the selected frame

### Advancing from Breakpoint

- `next <n>` - step forward n lines in the current function
- `step` - step into a function

### Enabling TUI: a Curses based Terminal UI

- Useful if using `next` and `step` to follow control flow:
- ctrl-x ctrl-a to show
- ctrl-x ctrl-a to hide
- PGUP and PGDOWN to navigate

## Watchpoints

- `watch <variable_name>` - Break on change to a variable

When the variable changes, a breakpoint fires, and the old and new state of the variable is printed

## Threads

- `info threads` - show all threads
- `thread <thread_no>` - switch debugger to a specific thread

## xv6 Specific stuff

### Debugging the xv6 kernel

1. Run `make qemu-nox-gdb`
2. Then in a new tab run `gbd kernel`
3. When ready to boot xv6, run `continue`
4. The xv6 shell is locked when the GDB console is active and visa versa.
5. To stop xv6 execution and open the GDB console, hit `ctrl+c`
6. Inspect state and set breakpoints
7. To resume xv6 execution, enter `continue`
8. Repeat steps 5-7 until debugging complete
9. Quit GDB by entering `quit` at the GDB console.
10. To quit the qemu session running xv6 by selecting the xv6 terminal and entering `ctrl+a` followed by x

### Debugging xv6 Shell Programs

By default, GDB is configured to debug the xv6 kernel. The process for debugging programs that run on the xv6 shell is a bit more involved. Here is an example with the program `cat`. Be sure that the Makefile is configured to add the debugging symbols to these programs.

1. Run `make qemu-nox-gdb`
2. Then in a new tab run `gbd kernel`
3. When ready to boot xv6, run `continue`
4. The xv6 shell is locked when the GDB console is active and visa versa.
5. To stop xv6 execution and open the GDB console, hit `ctrl+c`
6. Load the debugging symbol for the userspace program that you want to debug. Generally these are prefixed with an underscore, so to debug `cat`, you enter `file _cat`
7. Set a breakpoint on cat, `b cat`
8. Resume xv6 execution, enter `continue`
9. Execute `cat` in the xv6 shell
10. The breakpoint you set should have been caught, throwing you to the GDB shell. Perform debugging as needed.
11. Repeat steps 7-10 as needed
12. When done with cat, you can resume debugging the kernel by entering `file kernel`

## Additional Resources

- Good YouTube tutorial https://www.youtube.com/watch?v=bWH-nL7v5F4&t=55s
- Debugging with GDB Book https://sourceware.org/gdb/onlinedocs/gdb/
