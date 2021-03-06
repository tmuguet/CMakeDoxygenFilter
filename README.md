Introduction
------------

This project provides a Doxygen input filter to generate documented
C code from CMake script files, allowing you to use all features of
Doxygen to document your CMake macros and functions.

Build Instructions
------------------

You can build the executable in two ways:

  1. Build the project using CMake and either do a `make install`
     or use a `find_package(CMakeDoxygenFilter)` call in your own
     CMake based project (set CMakeDoxygenFilter_DIR to the binary
     build directory in advance to help find_package finding the
     config file).
    
  2. Copy the CMakeDoxygenFilter.cpp source file (or download it
     within your build system) and use the `try_compile` feature
     of CMake. You can use the CMake function in the supplied
     file FunctionCMakeDoxygenFilterCompile.cmake, which also
     briefly demonstrates how to document a CMake function.
    
The following compile-time configuration options are available by
using defines:

  * `DUSE_NAMESPACE=<your-namespace>`
    This will wrap all generated function declarations in the
    namespace `<your-namespace>` (you could use `-DUSE_NAMESPACE=CMake`)

Converting CMake expressions to C
---------------------------------

The filter executable recognizes the following CMake constructs:

  - Comments starting with `#!`
    The token `#!` will be replaced with `///` and the rest of the
    comment line is printed out unmodified.
  
  - Macro definitions
    A CMake macro is converted to a C function. For example, the CMake
    macro
    
    ```
    macro(MyCMakeMacro arg1 arg2)
    endmacro()
    ```
    
    will be converted to the C function
    
    ```
    MyCMakeMacro(arg1, arg2)
    ```
    
  - Function definitions
    A CMake function is converted to a C function. For example,
    the CMake function
  
    ```
    function(MyCMakeFunction arg1)
    endfunction()
    ```
    
    will be converted to the C function
  
    ```
    MyCMakeFunction(arg1)
    ```

Documenting CMake functions and macros
--------------------------------------

You can use any of Doxygens commands in your CMake script file, after
starting a Doxygen comment with `#!`.

Make sure to use Doxygens grouping features to structure your macros
and functions.

Doxygen configuration
---------------------

Here is an example Doxygen config file to get you started:

    OUTPUT_DIRECTORY = ~/DoxyDoc
    INPUT = ~/DirContainingLotsOfCMakeScripts
    RECURSIVE = YES
    # add for example *.cpp *.c *.h
    FILE_PATTERNS = *.cmake
    # allows you to display the source (i.e. the CMake scripts)
    SOURCE_BROWSER = YES
    # include undocumented entities
    EXTRACT_ALL = YES
    # use the CMake doxygen filter for .cmake files
    FILTER_PATTERNS = *.cmake=/PathToCMakeDoxygenFilterExecutable/CMakeDoxygenFilter_DIR
