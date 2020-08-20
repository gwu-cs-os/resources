# Using `make` and Makefiles

Makefiles facilitate consistent and reliable compilation of complex projects that are dependent on a number of source files and libraries. While a compiler can be invoked directly from the command line, compilation may require the input of a large number of paths, references, or multiple commands. A `Makefile` allows a programmer to manage and maintain the complex set of dependencies for a program in a script and to invoke the potentially complex, multiphase build process through the one word command `make`.

You will need at least a basic understanding of `Makefile` syntax to succeed in this course. The syntax used in a `Makefile` can be quite sophisticated and variable, and there are a number of approaches that may be used to achieve the same goals. We will begin exercising with a very basic `Makefile` however, we will quickly transition to a much more complex `Makefile` to build XV6 as even a basic operating system is sufficiently complex. It will be difficult to anticipate what you will need to understand in terms of `Makefile` syntax in advance, so be prepared to spend some time analyzing the XV6 `Makefile` when XV6 is introduced in lab.

## `make` Terminology

-   Building
-   Cleaning

## `make` Commands

`make` commands are subject to definitions within the `Makefile`. The `make` command alone will build the project, but parameters that may follow the `make` command are entirely dependent on what is defined in the `Makefile`. Any parameters used in the following list are generally standardized approaches, but their implementations are subject to the design of the `Makefile`.

-   `make`
-   `make clean`

## References

-   [Gabe's YouTube Tutorial: Makefiles: 95% of what you need to know](https://www.youtube.com/watch?v=DtGrdB8wQ_8)
-   [GNU Make Manual](https://www.gnu.org/software/make/manual/make.pdf)
-   [Managing Projects with GNU Make](https://www.oreilly.com/openbook/make3/book/)
