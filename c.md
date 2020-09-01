# C Modus Operandi

There are unwritten rules about writing C code, and a set of conventions we must follow for the class.
They include:

-   Your `.h` files should never include global variables.
    This can cause linking errors when two `.c` files include the same `.h` file, and are linked together.
-   If your `.h` files include function bodies (definitions), then the function must be `static inline`.
-   Your variables and functions should have the most restrictive, appropriate visibility.
    Global variables and functions that are only used within a `.c` file, should be `static`.
    Generally watch [this](https://www.youtube.com/watch?v=P8g4B9c0i8A&t=490s) if you have questions about visibility.
-   You should _never_ `#include` anything other than a `.h` file.
    `.c` files should be compiled separately, and linked together.
-   If you're writing a library, and it includes both `.c` and `.h` files, you must expect that a client will write their own `.c` file (e.g. `main.c`) that contains a `main` function, and link that file with your `.c` library file, and include your `.h` file.
-   You should include header guards in all `.h` files (again, see the youtube video).
-   If any file relies on some `struct`, function, or variable, that file _depends_ on the declaration (and type signature) of that to compile.
    Thus, each file must `#include` a complete set of header files that include all of those declarations/prototypes.
    For example, if your file calls `malloc`, you had better `#include <stdlib.h>`.
    Your tests _might_ compile without including that file explicitly (maybe because they include another file, `foo.h`, that includes `stdlib.h`), but this is _not_ resilient design.
    If a client includes `foo.h` after the prototype for `malloc` is required, this will result in compiler errors.
    In short, you must write your code such that the _order_ of includes in client code should _never matter_ (i.e. never result in compilation failures).
    
## Style

Code style matters *a lot*, especially in laissez faire languages like C.
It is *very easy* to write **bad** C code.
Adhering to a uniform style helps you stay "within the rails".
Every serious software engineering group has a style and a process to ensure uniformity of the code in their project.
For a discussion how to think about reading and writing code, and on a justification for the following style, see the [Composite Style Guide (CSG)](https://github.com/gwsystems/composite/blob/ppos/doc/style_guide/composite_coding_style.pdf).
Please note that people do not *naturally* write good code.
We write crap code, get it working, then go back and simplify.
The sooner you buy into this notion of [code craftsmanship](https://www2.seas.gwu.edu/~gparmer/posts/2016-03-07-code-craftsmanship.html), the sooner you make your way from learning CS, to becoming a software engineer.

The most direct style specification:

- Comments should be unform. Use `/* comment here */` for single line comments, and 
    ```c
    /*
     * comment here
     * and here
     */
    ```
    for multiline comments.
    Yes this takes up more space.
    Luckily space is free.
- Use `/* FIXME: ... */` to document something that needs fixing, `/* TODO: ... */` for a feature to be done in the future (but not a bugfix).
- Use 8 space indentation, or use tabs visibly displayed as 8 spaces.
    This promotes writing code that optimizes for short-term memory limitatons (see the CSG).
- The use of `{}` is nuanced.
    
    - If a block is a single line, you can omit the `{}`, for example `if (x) return x;`.
    - If a block is multiple lines, keep the `{` on the same line as the associated statement, for example:
        ```c
        if (x) {
                body_of_block();
                more_computation();
        }
        ```
    - If `if`, `else if`, `else` sequences are used, compacting keywords with brackets is preferred, for example:
        ```
        if (a) {
                foo();
        } else if (b) {
                bar();
        } else {
                baz();
        }
        ```
    - If the block correspond to the function scope, make the function type easy to scan by placing the `{` on a separate line.
        For example:
        ```
        static int
        foo(int a)
        {
                return a + body_of_foo();
        }
        ```
    - The return type and function attributes should be on separate lines in function implementations.
        For example, see above.
        This makes it much easier to scan for the function names as they are all column-aligned.
- Use `snake_case` function, type, and variable names.
    I know this feels strange, coming from Java.
    This is C, not Java, and you adhere to the common styles of the language you're using.
- Place a space around all binary operators, and no space around unary operators.
    Some examples:
    
    - `a + b`
    - `a++`
    - `-a`
    - `foo(a)`
    - `sizeof(b)`
    - `a + foo(-a + sizeof(b))`
- As conditionals and loops are *not* operators, add spaces between them and the condition body as in: `if (a)...` and `while (b) ...`.
- `if` should use spaces after `;` but not before.
    For example: `if (a; b; c)...`.
    There's no justification for this, we just have to choose a style.
    This is a style well supported by the llvm auto-formatter.
- Be very careful with error handling (release locks previously taken in the function, free memory, etc...).
    See the CSG for guidance on how to use "local exception handling".
- Only use `typedef` on values that fit into a single word (not on large structs, or arrays).
    When looking at a type, the user should know if they can pass it by value or by reference.
    This rule means that if you see a typedef, yes, you can pass it by value.
- Avoid using macros.
    If you do, they should include all caps, and can break words with `_`.
- Please see the CSG for details on naming, constants, switch syntax, and many other additional details.

Note that `xv6` doesn't adhere to all of these rules.
When you're coding in a specific code base, try and adhere to the rules of that code-base.
    
