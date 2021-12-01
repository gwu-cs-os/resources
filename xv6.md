# `xv6` hints and tricks

To run `xv6`, you need `qemu` installed.
If you aren't running Linux, then you might execute Linux in a virtual machine (e.g. using Virtual Box or VMWare Workstation), and `qemu` within Linux.
I believe others have gotten `qemu` executing in OSX as well (but it takes installing a cross-compiled ELF-version of `gcc`), so discuss on Piazza if you're interested in that.

```
$ git clone https://github.com/gparmer/gwu-xv6.git xv6
$ cd xv6
$ make qemu
```

Note that the `xv6` book has a great overview of the design of the *entire* system.
Please use it [as a resource](https://pdos.csail.mit.edu/6.828/2012/xv6/xv6-rev7.pdf) if you need it.
Note that since you don't own this repo, you cannot `git push` to it.
You have to fork it on `github` to become the owner of your own fork.
For this class, all assignments will be completed on repos that we provide for you.

**Some hints for understanding the code-base.**
All code that runs in user-level is in the files that correspond to the entries of [`UPROGS`](https://github.com/gparmer/gwu-xv6/blob/master/Makefile#L161-L176), and all of the header files that they include.
The rest of the code executes in the kernel.
Thus to add a new program, you only need to provide the `.c` file, and add it in a similar manner to `UPROGS`.

When you're looking at a `.c` or `.h` file, always make sure you understand if it is executing in user-space (thus must make a system call to enter the kernel), or is in the kernel (thus can directly call functions within the kernel).
Using a code indexing system will make it easier to walk through the source code.
`ggtags` or `ctags` for emacs does great, but there are corresponding technologies for nearly all editors.

## Adding new files

- When you want to add a user-level program, you simply implement it as a `.c` file, and add it, appropriately, to the list of `UPROGS` in the `Makefile`.
- When you want create a library (a `.c` file in `xv6`) that is compiled into each program, add it, appropriately, to the list of `ULIB` in the `Makefile`.
- If you want to add a new file -- for example, `new.txt` -- to the root file system (i.e. a file you can use to open, cat, etc...), you do the following (as another resource, see [this](https://stackoverflow.com/questions/47250441/add-a-generic-file-in-xv6-makefile)):

	1.
	   ```
	   fs.img: mkfs README new.txt $(UPROGS)
	   ./mkfs fs.img README new.txt $(UPROGS)
	   ```
	2.
	   ```
	   PRINT = runoff.list runoff.spec README new.txt toc.hdr toc.ftr $(FILES)
	   ```
	3.
	   ```
	   EXTRA=\
	   mkfs.c ulib.c user.h cat.c echo.c forktest.c grep.c kill.c\
	   ln.c ls.c mkdir.c rm.c stressfs.c usertests.c wc.c zombie.c\
	   printf.c umalloc.c\
	   README new.txt dot-bochsrc *.pl toc.* runoff runoff1 runoff.list\
	   .gdbinit.tmpl gdbutil
	   ```
     
## Running out of disk blocks to hold the programs and files

When you add more files (e.g. test files) into the system, you may run out of blocks.
You can increase the number of blocks in the system by increasing the `FSSIZE` variable.
Sometimes it is necessary to increase `BSIZE` as well.

## Debugging with `gdb`

Please see [this section on xv6](https://github.com/gwu-cs-os/resources/blob/master/gdb.md#xv6-specific-stuff) in the gdb resources.

## Executing main() in xv6

When executing a C program in a Linux system, an executable file (ELF) is created after compiling. In this file, there is a pointer to the `_start()` function which serves as the entry point for the program. This function calls `main()` and `exit()` once `main()` has returned after executing. This is why, in Linux, we can simply do: 
```
int main()
{
	return 0;
}
```
and our process will be terminated via the "hidden" `_start()` function. However, in xv6, we do not have this system setup. If we try to implement the above program in xv6, we may get an error like:

```
trap 14 err 5 on cpu 1 eip 0xffffffff addr 0xffffffff--kill proc
```

To address this, we have to terminate the process ourselves, like the `_start()` function typically does for us: 
```
int main()
{
	exit();
}
```

## Errors running `sign.pl`

If your `make` is breaking when trying to run `sign.pl`, try and run the command at which `make` breaks.

For example, if it breaks on:

```
...
./sign.pl bootblock
make: ./sign.pl: Command not found
...
```

...try running `./sign.pl bootblock`. 
If the error it reports has a `^M` in it, you have somehow copied some of the `xv6` files through Windows, which uses [CR/LF newlines](https://www.cyberciti.biz/faq/howto-unix-linux-convert-dos-newlines-cr-lf-unix-text-format/).
Linux (i.e. WSL) doesn't understand these newlines.
To update the `sign.pl` file to have proper Linux newlines, you can (install and) use the `dos2unix` program: `dos2unix sign.pl` which removes the `^M` newline.
