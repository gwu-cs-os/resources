# Ubuntu Essentials

We work in Ubuntu throughout this course, and as such, ou will need to develop a working knowledge of GNU/Linux in order to succeed. This section is intended to inform you of the most basic GNU/Linux concepts and operations that you must become proficient with. The instructional team assumes previous working proficiency of a Unix shell environment, either via GNU/Linux or another Unix variant, such as Mac OS.

## Bash

We will work from the the [Bash](https://en.wikipedia.org/wiki/Bash_%28Unix_shell%29) shell, the default shell environment for GNU/Linux.

_Note: The terms “shell”, “console”, “terminal”, and “command line” may be used interchangeably throughout this course)._

Nearly all servers in modern hyper-scale cloud providers run GNU/Linux, and generally, these systems run “headless” and lack the ability to render to full GUI interface. Working on such systems is typically limited to using `ssh` to tunnel into a shell environment, and then issuing commands to accomplish your goals. As such, mastering the GNU/Linux shell is an essential skill for modern software professionals. The earlier you adopt and practice working with the console, the faster you will develop in all aspects of this course and the more successful you will be overall. You will not be able to successfully complete the course work if you are dependent on graphical interfaces!

The instructional team assumes that you are already possess a working proficiency of this environment.

## Package Management

In the process of setting up your Ubuntu environment, you will need to install several pieces of software. Ubuntu uses the Advanced Packaging Tool (`apt`) to install software. To install a package, use the `apt-get` command. The syntax for installing a package is as follows:

```
sudo apt install <packagename>
```

To get help information on `apt`, refer to the man pages on `apt` or `apt-get` or use the following command:

```
apt –help
```

### To install `gcc` and `make` on Ubuntu

```
sudo apt install build-essential
```

### To install `git` on Ubuntu

```
sudo apt install git
```

### To install `emacs` and `vim` on Ubuntu

```
sudo apt install emacs vim
```

## Terminal-based Text Editors

There are a number of text editors available for GNU/Linux. The two main terminal-based text editors focused on software development are `vim` and `emacs`. These two programs have comparable features, but they use very different interfaces, and, similar to “tabs versus spaces,” they have been a perennial source of disagreement between developers.

-   [xkcd on vim versus emacs](https://xkcd.com/378/)
-   [HBO’s Silicon Valley on vim versus emacs](https://www.youtube.com/watch?v=3r1z5NDXU3s)

Even if you prefer graphical editors, such as VSCode, it is essential that you become sufficiently proficient in either `vim` or `emacs` to be productive in a headless environment.

## Review Questions

1.  What is a path?

2.  In a path, what do the following mean?

    1.  `.`
    2.  `..`
    3.  `~`

3.  How does a relative path differ from an absolute path?

4.  What are wildcards?

5.  What does the shell command `ls` do?

6.  What does the shell command `cd` do?

7.  What does the shell command `mkdir` do?

8.  What does the shell command `rm` do?

9.  Why should you be extremely careful using the `rm` command with wildcards?

10. What does the shell command `cp` do?

11. What does the shell command `mv` do?

12. What does the shell command `grep` do?

13. What does the shell command `cat` do?

14. What does the shell command `sudo` do?

15. What do either of the shell commands `vi` or `emacs` do?

16. What is a package manager?

17. What is the command to install vim on Ubuntu?

## Reference Cards

-   [Shell Commands](https://files.fosswire.com/2007/08/fwunixref.pdf)
-   [emacs](https://www.gnu.org/software/emacs/refcards/pdf/refcard.pdf)
-   [vi](http://web.mit.edu/merolish/Public/vi-ref.pdf)
-   [VSCode](https://code.visualstudio.com/docs/getstarted/keybindings)
