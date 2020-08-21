# Man(ual) Pages

Google is indeed one of the most important tools of any modern software engineer, but generally, it is generally not the best place to start for specialized technical questions. Within specific areas of knowledge, there are often specialized references that are more context appropriate. For example, the canonical reference for web technologies is the [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Reference). In fact, [even Google links to these docs](https://blog.mozilla.org/blog/2017/10/18/mozilla-brings-microsoft-google-w3c-samsung-together-create-cross-browser-documentation-mdn/)! As such, most practicing web developers have this resource open at all times.

Systems programmers have a similarly canonical resource called `man pages`, short for manual pages. Additionally, just web documentation in expressed in the context of what it documents (a web page), systems documentation is expressed in the context of what is describes (an operating system). Specifically, this means interacting with the `man` utility. For whatever reason, many students seem to struggle to get beyond the initial learning curve of interacting with the documentation tools that come with Linux. This primer seeks to justify why the man page remains an essential tool, and help you get comfortable using it as a systems programming research tool.

# History

Programmers typically hate to write documentation! Shouldn’t the code be enough? These thoughts were shared by Dennis Ritchie and Ken Thompson when Bell Labs was readying to release the First Edition of their new operating system Unix. However, their manager, Douglas McIlroy, knew that for Unix to be adopted by normal users, it would have to include robust documentation comparable to that provided by computer manufacturers such as IBM or DEC. Dennis and Ken relented, and they wrote what became known as the _Unix Programmer’s Manual_). In doing so, Unix established itself with a reputation of having well-written and comprehensive documentation, particularly for a research project on a shoestring budget! This documentation was distributed digitally with Unix systems so that programmers could print their own manuals using a manual macros (called `man` in the terse Unix style). Expectations about excellent documentation became part of the Unix culture, and the culture of writing and updating man(ual) pages spread from Bell Labs Research Unix to modern descendants, such as Linux and BSD.

Once screen-based terminals became common, Unix users and systems programmers increasingly read this documentation directly from shell, which provided the additional advantage of ensuring that the documentation (now called a `man page`) was updated alongside the utility it documented. This guarantee holds today, so searching your local man-db (manual database) ensures there won’t be the sort of version mismatch that you might see when searching the web.

One of the negative cultural byproducts of writing and maintaining excellent documentation is that experienced systems programmers have often gotten frustrated when asked simple questions that they believe an individual should have been able to easily look up. _Isn’t this a solved problem? Why is this whipper-snapper wasting our time?_ Older developers telling younger developers to read the manual became so common on Internet bulletin boards, that it devolved into a toxic meme called RTFM or Read the Fantastic Manual. The millennial or Gen Z equivalent of this is probably something like LMGTFY or [Let Me Google that for You](https://www.lmgtfy.com/).

In either case, the trope expression of condescension towards people at the beginning of their educational journey is quite abhorrent! Given the natural frictions of learning and self improvement, these attitudes are responsible for much of the toxic and unwelcoming culture around various programming subcultures, and as such, they have become a common target of Codes of Conduct in programming communities.

It is a skill to use an Internet search engine effectively or know how to search and lookup a `man page`. Asking questions is usually not reflective of sloth and a desire to be spoonfed answers. It’s a reflection of the person’s need to improve this particular skill set.

Unix-based systems have excellent documentation, and learning how to search and interact with man pages is a tremendous productivity boost and a sign of a mature software craftsman. There is a natural friction to this because the syntax of the `man` command has a bit of learning curve, but as aspiring systems programmers, we should embrace the power of the man pages and help guide and mentor others towards being able to research solutions to their systems programming problems.

## Review the DESCRIPTION section of the `man` utility.

As a first step, let’s use `man` to look at its own manual page.

`man man`

Look at the DESCRIPTION and EXAMPLES sections, and notice that the man pages essentially retain [the structure of the original _Unix Programmer’s Manual_ written by Dennis Ritchie and Ken Thompson](https://www.bell-labs.com/usr/dmr/www/manintro.pdf).

The remainder of this primer will investigate these different sections and introduce the commands needed to search and read these resources.

## Exploring Section 1: The Shell Environment

Section 1 of the Programmer’s Manual is focused on the shell environment, which includes the shell interpreter itself and the numerous programs available to run in this environment. This is the place to look up documentation for the numerous compiler and utilities we use in day-to-day systems programming:

_Skim the DESCRIPTION section on the man pages of each of the following tools._

-   `man cpp`
-   `man gcc`
-   `man as`
-   `man ld`

_How do these components work together to produce an executable?_

The instructional team assumes that you possess a basic understanding of how source code in a languages such as C is transformed into Assembly that the computer can run. If this is something that is unfamiliar to you, consider looking at [this excellent GeeksForGeeks explanation](https://www.geeksforgeeks.org/compiling-a-c-program-behind-the-scenes/)

## Section 2: System Calls

System calls are a topic that we will repeatedly revisit in the course. In a lab in the near future, you will write your own system call. Read the introduction of this section to get a general idea of what this is.

`man intro.2`

Notice that the `.2` explicitly specifies which section’s intro we mean. Often times, different sections of the Programmer’s Manual have entries with the same name. We disambiguate this by making the section explicit.

Now let’s look at fork, one of the most important system calls.

`man fork`

Try to answer the following questions:

-   Which C headers do you need to include to make this call?

-   What are the possible return values?

-   What `errno` values can be set? Open `man errno` to get an idea of how this works

## Section 3: Standard Libraries

Section 3 covers the C standard library (called `libc`), the standardized POSIX library common to all Unix variants, and the various Linux extensions. Working with these library functions is a key part of systems programming, so this section is particularly useful to identify or refresh your knowledge of these many APIs.

Let’s go through a semi-contrived example to illustrate how you might use this. let’s imagine that we are looking at C source code, and we see use of a library function named `strlen`. It sounds familiar, but we can’t recall what it does exactly.

A good first step might be lookup up a one line description of the command:

`whatis strlen`

However, we often likely want additional information and want to pull up the full man page.

`man strlen`

Notice that `whatis` returned the contents of the NAME section.

We’ve discussed some of the sections of a manpage before, but scroll down to the SEE ALSO section. This section discusses related manual pages that we might be interested. In this case, there is a very similar command `strnlen`. Let’s try to get a better understanding of the differences.

`man strnlen`

-   What are the differences between these functions?

-   What might be the advantage of `strnlen` over `strlen`?

## Aside on `man-db`

Thus far, we’ve used `whatis` and `man` to look at data on various entries, but we haven’t addressed how any of this works behind the scenes. Manual pages are powered by a database called `man-db`. This database includes quickly searchable metadata and manual page entries and information about where to find the complete manual pages on the filesystem. Normally, this database is fully transparent to users, but frequently, when you install a package from a package manager, you’ll see a log messages that look like this:

```
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
```

This indicates that the package included a new manual page, which it registered with `man-db`.

## Sections 4-8: Grab Bag

Sections 4-8 are less frequently used by systems programmers, but we can browse and search the metadata of our `man-db` using the `apropos` utility to get an idea of what they contain. If you’re curious, this utility is named after a somewhat archaic English word meaning “with regards to.”

Typically, `apropos` is used for a keyword search, but we can also use it to list all manual pages by searching for the `.` character, which matches on everything.

`apropos .`

We can use a flag to list all entries in a particular section. List all manual pages in section 7, “Miscellaneous.”

`apropos –section 7 .`

Scrolling through, it does indeed seem like Section 7 has quite a lot of miscellaneous content.

However, there are some useful tutorials in there as well.

For example, if you feel like you need additional content after completing the git primer associated with this lab, you might find it useful to review one or more of the following:

-   `man gittutorial.7`
-   `man giteveryday.7`
-   `man gitworkflows.7`

## Keyword Search Examples

We’ve seen that `apropos` can be used to list content, but typically this utility is more useful to search man pages by keyword. Just as with the web, unless you already know the unique identifier with the content you want, searching with apropos is often a good first step.

Let’s run through some example scenarios of how this can be useful in a day-to-day scenario. Imagine that you’re writing an algorithm in C, and you need to deal with complex numbers. This is an unusual topic, and likely, you won’t know the relevant library functions off the top of your head.

Because we know we are searching for a C standard library function, we can narrow our search to section 3. The syntax is similar to the previous example where we listed all pages in section 7. A good first pass at a search might be as follows:

`apropos –section 3 complex number`

Unfortunately, the results don’t seem too useful! By default, apropos performs an OR search, meaning the results include matches that contain “complex”, “number”, or both. A lot of things mention “number,” so our signal to noise ratio is weak. One possible improvement is that we can perform a strict search on the phrase “complex number”

```
apropos –section 3 “complex number”
```

This works great here, but this might now work for other queries. Imagine for example that we want to find functions that deal with string length. We’d likely want to match phrases such as “length of a string” as well, so we really want to perform an AND search for text that contains both words.

```
apropos –and string length
```

With complex numbers, we knew that we were searching for standard library functionality, but often, things are a bit more ambiguous with systems programming because we might be interested in directly calling system calls as well. Thus, we want to be able to search multiple sections at the same time.

Imagine that we are searching for operations involving a `pipe`.

```
apropos –section 3,2 pipe
```

The takeaway is that `apropos` is able to handle very complex queries, include wildcard globbing and regular expressions. As with most everything on a Unix-based system, to learn how to use these options, consult the man page:

```
man apropos
```

## Full-text keyword search

The `apropos` utility only searches the metadata entry contained in the `man-db`. This essentially corresponds to the NAME section of a man page or what is returned by the `whatis` utility. Usually this suffices, but sometimes we might want to search the full text of man pages. Historically, this was quite slow, but modern processors can do this relatively quickly.

For example, many manual pages attribute the authors that wrote the utility. We can search for man pages that mention Linus Torvalds.

`man –global-apropos “Linus Torvalds”`

## Review

-   What is the name of the 1971 Unix publication from which modern man pages are derived?

-   What are the major section that man pages are organized into?

-   What is the command to view a one line description of a utility?

-   What is the command to perform an AND (union) search of multiple terms across sections 2 and 3?

-   What are the major sections of an individual man page?

-   What are the man pages that specifically attribute [Richard Stallman](https://en.wikipedia.org/wiki/Richard_Stallman) as author?
