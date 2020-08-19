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
