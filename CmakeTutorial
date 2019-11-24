#Set minimum version of cmake required
	cmake_minimum_required(VERSION 3.13)

#Set project name and Languages used
	project(CursesWrapperTester LANGUAGES CXX)

#Setting cmake Variables
	set(${VARNAME} VALUE)
	
	#! These vars can be called on using ${} notation

#Set version of the languages used in the project
	set(CMAKE_CXX_STANDARD 14)


#Name the executable to be made
	add_executable(CursesWrapperTester
    	    main.cpp file1.cpp file1.hpp)

#Give the make file compiler options
	target_compile_options(CursesWrapperTester PRIVATE -Wall -Werror){DO NOT USE COMPILER OPTIMIZATIONS FOR DEBUG BUILDS}

#Directories 
	You can include whole directories to the working directory of Cmake with 
	include_directories(dir1 dir2)

#Building Libraries from source
	add_library(${LIB} STATIC ${SOURCE1, SOURCE2...}

	You may run into some problems when building purely from header files as cmake determines linker 
	language from the source files
	This problem can be fixed by specifying the target's languages via 

	set_target_properties(${TARGETNAME} PROPERTIES LINKER_LANGUAGE CXX)

#Linking Libraries 
    Static Lib
	target_link_libraries(${BINNAME} ${LIBNAME}

#NOTE
	if building a lib that is to be used later it must also be linked in the driver program
	

#Creating Lists in Cmake
	As the amount project files get larger or build trees diverge heavily it becomes impractical to just keeping adding filenames as agruments. This leads us to creating list variables that we can call to represent all the files we pass to it

	list(APPEND ${LISTNAME} "file1" "file2"...)
	
	The list name can be defined higher in the build tree, expanded, shortened, and called later in the build tree

#Setting a list variable for all source files in a directorya
	
	AUX_SOURCE_DIRECTORY({file} {VAR})
	
#I've had problems with AUX_SOURCE_DIRECTORY() before 
#Sometimes file(GLOB) works better
#does the same thing with better documentation
	file(GLOB ${VAR} "some unix wildcard expression")

#CMake Control Flow
	* as more files are added to the project we want to try to stay away from thigns that may repeat steps in the compilation process that we have already done
	* this leaves control flow something to be desired so we may find and skip certain compilation steps

	Cmake control flow goes as follows:
		if(${CMAKEVAR})
			${CMAKE Directives}...
		endif()
	Control flow bools can be augmented with logical keywords like {AND, NOT, OR,}
	
#Creating files and directories
Reading
  	file(READ <filename> <out-var> [...])
  	file(STRINGS <filename> <out-var> [...])
  	file(<HASH> <filename> <out-var>)
  	file(TIMESTAMP <filename> <out-var> [...])

Writing
  	file({WRITE | APPEND} <filename> <content>...)
 	file({TOUCH | TOUCH_NOCREATE} [<file>...])
  	file(GENERATE OUTPUT <output-file> [...])

Filesystem
	file({GLOB | GLOB_RECURSE} <out-var> [...] [<globbing-expr>...])
	file(RENAME <oldname> <newname>)
	file({REMOVE | REMOVE_RECURSE } [<files>...])
  	file(MAKE_DIRECTORY [<dir>...])
  	file({COPY | INSTALL} <file>... DESTINATION <dir> [...])
  	file(SIZE <filename> <out-var>)
	file(READ_SYMLINK <linkname> <out-var>)
	file(CREATE_LINK <original> <linkname> [...])
		

#CMake Loading Messages
	MESSAGE GLOBAL VARS:
		FATAL_ERROR: stops cmake procesing and generation
		SEND_ERROR: continue processing but skip generation
		WARNING: continue processing after error print
		STATUS: Messages displayed during processing and generation
		See Cmake docs for more info on more obscure message vars

	message(${GLOBAL VAR} "Message to be displayed ${USERVAR}")


#How to generate make file with Cmake

#Create Binary Directory
	(bash) mkdir {binary_dir}

#You will notice that cmake genreates a lot of ugly files in your working tree
#You may want to move these files into another directory to declutter the working tree

	(bash) cd ${cmake-files}
	(bash) cmake ../
#This will generate the files in this {cmake-files} directory
#This {cmake-files} directory will likely be your bin directory as i dont currently know how to specify where the cmake generated files will output to

#You may also want to specify where targets output to
	set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/lib)
	set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)

	where LIBRARY_OUTPUT_PATH, EXECUTABLE_OUTPUT_PATH, and CMAKE_SOURCE_DIR are global cmake variables
	lib and bin are directories specified by the writer


#Run cmake with --<generator-name> tag & --<path-to-build> tag
	(bash) cmake -G Unix\ Makefiles -{binary_dir}


#In binary_dir 
	CMakeCache.txt  CMakeFiles  cmake_install.cmake  Makefile <---

