# GCC Flags Reference

## Generally Used GCC flags and their uses

-   `-Wall` - all warnings
-   `-Wextra` - extra warnings
-   `-g` - debugging information
-   `-Wpedantic` - standard conformance
-   `-Werror` - turn warnings into errors
-   `-std=c17` - asks the compiler to use the C17 standard

# Basics and References

The GCC compiler is used to streamline code and speed it up. However, in the process it may render the code somewhat unreadable. Compiler optimizations will generally make the code faster but probably more difficult to understand.

In a development setting, different optimization levels affect compilation time and ease of debugging

In a production setting, different optimizations reflect a tradeoff between executable size and execution speed. For example, in the embedded domain, sometimes a smaller executable is more attractive than optimal code.

A list of GCC optimization options:

-   [A discussion on GCC optimization options](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)
-   [A dated article that explains the optimizations well](https://www.linuxjournal.com/article/7269)

## A brief overview of the optimization levels of GCC

A large variety of optimizations are provided by GCC at one of three
levels but some at multiple levels. Some optimizations reduce the size
of the resulting machine code while others reduce execution time.

Optimizations can be set by:

1. Implicitly enabling and disabling specific performance optimizations via a flag indicating a level of optimization that groups different performance flags in something easier to understand

2. Explicitly enabling and disabling performance optimizations with the -f switch

3. A combination of 1 and 2

For example, `gcc -O1 -fno-defer-pop -o test test.c` enables O1 level optimization but disable the defer-pop option. This option basically allows the compiler to let function arguments accumulate on the stack for several function calls and pops them all at once. By disabling this option, the compiler is forced to pop function arguments from the stack as soon as the function returns.

### Level 0 (`-O0`)

This level reduces compile time and make debugging produce expected results. If you are debugging with GDB and see `<optimized out>`, this is probably the option for you.

### Level 1 (`-O1`)

This level generally tries to reduce both size of the code and execution
time. The optimizations done at this level do not increase compile time
significantly.

### Level 2 (`-O2`)

At this level, generally all optimizations, supported by that
architecture, that does not involve a speed-size tradeoff are performed.
With these optimizations, compilation time will be significantly
increased.

### Level 2.5 (`-Os`)

This is a special level at which all level 2 optimizations are enabled
excepting the ones that increase code size – these are the alignment
options (align-loops, align-jumps, align-labels, and align-functions).
These optimizations skip space to align loops, jumps, labels and
functions to an address that is a power of 2 in an architecture
dependent manner. Skipping to these boundaries can increase speed of
execution but also increases code size. Furthermore, all reorder block
optimizations and prefetch-loop-arrays are also disabled.

### Level 3 (`-O3`)

At this level, the focus is completely on speed. Several optimizations,
further to the ones turned on at O2, are turned on that increase speed
of execution but increases code size significantly. Although, this level
produces fast code, the size of the image can be detrimental of
execution time. For example, if size of code image is greater than size
of instruction cache then severe performance penalties will be imposed.

### `-Ofast`

Disregard strict standard compliance. The program will not be standard
compliant anymore.

### `-Og`

This should be the standard choice for the edit-compile-debug cycle. This level only enables optimizations that will not hamper debugging. However, at times, variables can still be "optimized out." If this occurs, try toggling to `-O0` to see what works better.

## Additional Resources

-   `man gcc`
-   [Using the GNU Compiler Collection (GCC)](https://gcc.gnu.org/onlinedocs/gcc/)
-   [Godbolt Compiler Explorer](https://gcc.godbolt.org/), a tool to see the assembly different compilers and optimizations produce
