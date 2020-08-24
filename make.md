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

## `make` Rules

Makefiles are essentially shell scripts that track dependencies

They are composed of `rules`, which have:

-   A unique name

-   Dependencies on other rules or files.

-   A block of shell commands they execute, indented by a tab (Spaces cause a syntax error!)

```makefile
rule_name: dependency_a dependency_b
    commandA
    commandB
```

## `make` Examples

`make all` runs `build`, which compiles the `main.c` file:

```makefile
clean:
    rm -rf a.out

build: main.c
    @gcc main.c

all: build
```

### Dependency Tracking

When a rule executes, it first executes all dependencies from left to right. Consider the following example. As much as possible, a Makefile seeks to minimize unnecessary work. It does this by inspecting the last changed timestamps of files that the rule depends upon. Thus, the `build` rule only runs if `main.c` has changed since the Makefile was last run. This property is transitive.

```makefile
clean:
    rm -rf a.out

before:
    echo "Before"

after:
    echo "After"

silent:
    @echo "After"

build: main.c
    @gcc main.c

all: before build after silent
```

Assuming that `a.out` exists and `main.c` has had no changes since the execution of the Makefile, running `make all` would run as follows:

1.  Execute the before rule, printing "Before"
2.  Skip the build rule because `a.out` is already up to date
3.  Execute the after rule, printing "After"
4.  Execute the silent rule, but nothing is printed, as the `@` prepended to the command made it silent.

## References

-   [Gabe's YouTube Tutorial: Makefiles: 95% of what you need to know](https://www.youtube.com/watch?v=DtGrdB8wQ_8)
-   [GNU Make Manual](https://www.gnu.org/software/make/manual/make.pdf)
-   [Managing Projects with GNU Make](https://www.oreilly.com/openbook/make3/book/)
