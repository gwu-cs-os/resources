# `xv6` hints and tricks

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

## Debugging with `gdb`

Please see [this section on xv6](https://github.com/gwu-cs-os/resources/blob/master/gdb.md#xv6-specific-stuff) in the gdb resources.
