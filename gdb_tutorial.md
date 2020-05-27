Before running gdb compile the binary with -g or --gstabs to save symbol table and labels

$gdb ./{executable}
    {--tui} will display the source code of the executable
        in --tui mode gdb defaults to focusing the source code
            (gdb) focus {window} 
                will change the focus of gdb to the called window
                    possible windows
                        src: source code
                        cmd: gdb shell
                        asm: disassembly code
            (gdb) refresh 
                will refresh the windows incase --tui mode looks fucked up
            (gdb) update 
                will update source windows and the current execution point
        You can change the window height using 
            (gdb) winheight {window} {+ or -}count

    {--args} will accept arguments that would follow the executable to make run command easier

(gdb) r(shorthand) || run  
    will run the program and stop at first break point

(gdb) b(shorthand) || break {line number or label} 
    will set a breakpoint in the debugger

(gdb) p(shorthand){\t} || print {arg}
    will print the value at the {arg}
    {/t} denotes the format you want print to display its default is hex
        x:integer
        d:decimal
        u:unsigned
        o:octal
        t:binary
        c:ascii char
        f:floating point

    in assembly objects must by cstyle type casted to the format you want to see it in
    arrays can be display as:
        (gdb) p/u array[0]@10
        will print the array from index 0 to ten
    
    all print statements can be used with cstyle operators such as *(dereference), &(address_of)

(gdb) watch {expression}
    sets a watchpoint that will break when {expr} is changed

(gdb) tty {filename}
    set the default run configuration of the r command with stdin redirection of {filename}

(gdb) d(shorthand) || delete {optional index}
    without {index} will ask to delete all break points  
    with {index} will delete breakpoint at {index}

(gdb) disable/enable {index}
    will enable/disable breakpoint at {index}

(gdb) s(shorthand) ||step
    steps a line

(gdb) c(shorthand) || continue
    continues until next breakpoint

(gdb) u(shorthand) || until
    continues until stackframe is deleted

(gdb) i(shorthand) || info {arg}
    gives information of {arg} at curent state of program
    Possible {args}
        symbol {addr}: prints the symbol the addres is stored at
        register: prints all registers
        args: prints args of the selected frame
        local: prints local vars
        catch: print all exception handellers in current stack frame
        breakpoints: prints all breakpoints
        frame: verbose information of the current stack frame
        mem: table of defined memory regions
        

(gdb) set {label} = {value}
    sets {label} = {value}
    
    
(gdb) display {expression/label}
    will print out {expression/label} after every step of the program


(gdb) call {function or subroutine}(ARGS)
	will call the equivalent written in the executable
	cannot call functions that have not been compiled with the origional binary
		-> cannot call vec.pop_back() if the function was not called anywhere else in the program
		   this is because gdb is not a compiler and since that function was not called anywhere else in the program
		   it likely was never compiled in the first place


$(bash) ulimit -c unlimited
	enables the generation of core files upon segfault

	core files are saved states of your program that can be look at and altered like snapshots 

$(bash) gdb ${EXECUTABLE} ${COREFILE}
	loads the saved core state of the program in gdb

(gdb) set use-coredump-filter on|off
	applies the core snapshot to the current gdb session


(gdb) gcore ${FILENAME}
		produces a core file at the current state of the gdb session

Similar to the .bashrc we can also use a .gdbinit to run gdb commands until
	This .gdbinit file can be declared in the working directory or globally in the user's $HOME directory
	simply write the commands in the gdbinit file as you would in the gdb shell and they will be run upon gdb startup


(gdb) record
	tells gdb to start recording what gdb commands have been called

	this enables reverse execution 

The following set of gdb commands require record has been invoked
NOTE that not all instructions may be supported by this functionality ie IO devices or IOstreams
startif

	(gdb) rc(shorthand) || reverse-continue
		reverses execution of the program back to the last breakpoint

	(gdb) rs(shorthand) || reverse-step
		like the step command but backward in execution

	(gdb) rn(shorthand) || reverse-next
		like the next command but backwards in execution

	(gdb) reverse-finish
		takes you back to the start of the current function execution

endif


We are also able to fully interface with the bash terminal that we invoked gdb from

(gdb) shell ${SHELL COMMAND} 