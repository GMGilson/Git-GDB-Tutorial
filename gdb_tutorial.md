# GDB Cheatsheet

Notes:
- Command prefaced with `~$` denote commands being executed in a POSIX compliant shell
- Commands prefaced with `(gdb)` denote valid gdb commands 
- This cheatsheet was created with GNU_ASMx86 in mind as it was written when I was taking a Systems Programming Class


## Prequisites
- [gdb](https://www.gnu.org/software/gdb/) (GNU Debugger)
- compiled binary with saved symbol table and labels, (compiled with -g or --gstabs flags)
  - preferably without any optimization flags

## Invoke GDB

    ~$ gdb ./{executable} [flags]
Flags:
- `--tui` flag will display the source code of the executable
  - `--tui` mode gdb defaults to focusing the source code
    - `(gdb) focus {window} ` will change the focus of gdb to the called window
      - possible windows:
        - `src`: source code
        - `cmd`: gdb shell
        - `asm`: disassembly code
    
 - `--args`
   - arguments to `$executable`

## Viewing Source Code
    (gdb) refresh 

 - Will refresh the windows incase --tui mode looks fucked up
 <!-- -  -->
    (gdb) update 
 - Will update source windows and the current execution point

## Interaction

    (gdb) r(un)  
 - Will run the program and stop at first break point
   
<!--  -->

    (gdb) b(reak) {line number or label} 
 - Will set a breakpoint in the debugger

<!--  -->

    (gdb) p(rint)){\t} {arg}
 - Will print the value at the $arg
    - `\t` denotes the format you want print to display its default is hex:
        - x: integer
        - d: decimal
        - u: unsigned
        - o: octal
        - t: binary
        - c: ascii char
        - f: floating point

- In assembly objects must by cstyle type casted to the format you want to see it in
- Arrays can be display as: `(gdb) p/u array[0]@10` printing the array from index 0 to 10
    
- All print statements can be used with cstyle operators such as *(dereference), &(address_of)

<!--  -->

    (gdb) watch {expression}
- sets a breakpoint that will trigger when `$expression` evaluates to true

<!--  -->

    (gdb) d(elete) {optional index}
- without `$index` will ask to delete all break points  
- with `$index` will delete breakpoint at `$index`

<!--  -->

    (gdb) disable/enable {index}
- will enable/disable breakpoint at `$index`

<!--  -->

    (gdb) s(tep)
 - execute a line of source code

<!--  -->

    (gdb) c(ontinue)
- continues execution until next breakpoint

<!--  -->

    (gdb) u(ntil)
- continues until stackframe is exited

<!--  -->

    (gdb) i(nfo) {arg}
- gives information of `$arg` at curent state of program
- Possible `$args`:
  - `$symbol ${addr}`: prints the symbol at the stored at `$addr` 
  - `$register`: prints all registers
  - `$args`: prints args of the selected frame
  - `$local`: prints local vars
  - `$catch`: print all exception handellers in current stack frame
  - `$breakpoints`: prints all breakpoints
  - `$frame`: verbose information of the current stack frame
  - `$mem`: table of defined memory regions
    
<!--  -->

    (gdb) set {label} = {value}
- sets `$label` = `$value`

<!--  -->
    
    (gdb) display {expression/label}
- Will print out `$expression/label` after every step of the program

<!--  -->

    (gdb) call {function or subroutine}(ARGS)
- will call the equivalent written in the executable
- Note: cannot call functions that have not been compiled with the original binary
  - ie) cannot call vec.pop_back() if the function was not called anywhere else in the program
  - this is because gdb is not a compiler and since that function was not called anywhere else in the program it likely was never compiled in the first place

## Advanced

    ~$ ulimit -c unlimited
- enables the generation of core files upon segfault

- core files are saved states of your program that can be look at and altered like snapshots 

<!--  -->

    ~$ gdb ${EXECUTABLE} ${COREFILE}
- loads the saved core state of the program in gdb

<!--  -->

    (gdb) set use-coredump-filter on|off
- applies the core snapshot to the current gdb session

<!--  -->

    (gdb) gcore ${FILENAME}
- produces a core file at the current state of the gdb session

## Scripting GDB (.gdbinit)

Similar to the .bashrc we can also use a .gdbinit to run gdb commands on invokation of gdb
	This .gdbinit file can be declared in the working directory or globally in the user's $HOME directory
	simply write the commands in the gdbinit file as you would in the gdb shell and they will be run upon gdb startup

## Reverse GDB

    (gdb) record
- tells gdb to start recording what gdb commands have been called

- enables reverse execution of programs where we can "reverse" stepping through a program

The following set of gdb commands require that record has been invoked.

**NOTE** that not all instructions may be supported by this functionality 
  - ie.) IO devices or IOstreams

<!--  -->

	(gdb) rc(reverse-continue)
- reverses execution of the program back to the last breakpoint

<!--  -->

	(gdb) rs(reverse-step)
- like the step command but backward in execution

<!--  -->

	(gdb) rn(reverse-next)
- like the next command but backwards in execution

<!--  -->

	(gdb) reverse-finish
- takes you back to the start of the current function execution


## Interfacing with your shell from GDB

    (gdb) shell ${shellCommand} 
    (gdb) ! ${shellCommand}

- execute `$ShellCommand`

## Invoking Make from GDB

    (gdb) make {args}

- call makefile in working directory with `$args`