# The CS1010 I/O Library

To help students get started with C programming without worrying too much about the details and pitfalls of using `printf` and `scanf`, we provide a simple-to-use library to read and write integers, floating point numbers, and strings.  

# Installing the Library

1. To get an updated copy of the library, clone it from its git repo on GitHub with the command:

```
git clone https://github.com/nus-cs1010-1819-s1/libcs1010.git
```

It is recommended you do this in your home directory.

You should see an output similar to:
```
Cloning into 'libcs1010'...
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
```

After that, you should see a subdirectory `libcs1010` created in your current directory.  Inside, there should be a file called `Makefile`, and two subdirectories called `include` and `src`.  

2. To compile the library, run

```
make
```

This should compile the file `src/cs1010.c` and create a static C library named `libcs1010.a` under the `lib` directory.

# Using the Library

## Header

To use the CS1010 I/O library, you should `#include` the file `cs1010.h`, like this:

```C
#include "cs1010.h"
```

at the top of your C program.

## Linking

The CS1010 I/O library is provided as the file `libcs1010.a`.  To link to the library, you need to compile with `-lcs1010`.  Usually, you need to specify where you can find `cs1010.h` with the `-I` flag, and `libcs1010.a` with the `-L` flag.  Assuming that you are compiling in another subdirectory under your home and `libcs1010` are located under your home directory, the header file and the library file are in `../libcs1010/include` and `../libcs1010/lib` respectively.

So you compile using the command line:

```
clang -I../libcs1010/include -L../libcs1010/lib hello.c -lcs1010
```

It is a long string to type.  We suggest that you put this in a shell script so that you just need to run the shell script to compile the program.

## Reading of a Single Value

The CS1010 library supports reading of `long` value, `double` value, and strings (both space-separated words and newline-separated lines) from the standard input.  For `long` and `double`. The relevant methods are:

- `long cs1010_read_long()`<br>
Returns a `long` value from the standard input.  An error message will be printed (to `stderr`) if the input sequence is not a valid `long` value -- in which case the value `LONG_MAX` will be returned.

- `double cs1010_read_double()`<br>
Returns a `double` value from the standard input.  An error message will be printed (to `stderr`) if the input sequence is not a valid `double` value -- in which case the value `DBL_MAX` will be returned.

- `char* cs1010_read_word()`<br>
Returns a `char *` pointing to the next white-space-separated string from the standard input.  A white-space character is defined based on the standard C function `isspace()` and includes the space ` `, tab `\t`, and newline `\n` character.  Returns NULL if there is an error.  If the returned value is non-NULL, the caller is responsible for freeing the memory allocated by calling `free`.  

- `char* cs1010_read_line()`<br>
Returns a `char *` pointing to the next new-line-separated string from the standard input.   The string returns from `cs1010_read_line()` includes the newline character (if one is found).
Returns NULL if there is an error.  If the returned value is non-NULL, the caller is responsible for freeing the memory allocated by calling `free`.

## Reading of Multiple Values

The CS1010 library also supports reading of multiple values.  

- `long* cs1010_read_long_array(int k)`<br>
Returns `k` numbers of `long` values read from the standard input stored in an array.  An error message will be printed (to `stderr`) for each input that is not a valid `long` value -- in which case the value `LONG_MAX` will be populated in the corresponding array element.  Returns NULL if there is a memory allocation error.  If the returned value is non-NULL, the caller is responsible for freeing the memory allocated by calling `free`.

- `double* cs1010_read_double_array(int k)`<br>
Returns `k` numbers of `double` values read from the standard input stored in an array.  An error message will be printed (to `stderr`) for each input that is not a valid `double` value -- in which case the value `DBL_MAX` will be populated in the corresponding array element.  
Returns NULL if there is a memory allocation error.  If the returned value is non-NULL, the caller is responsible for freeing the memory allocated by calling `free`.

- `char** cs1010_read_word_array(int k)`<br>
Returns `k` white-space-separated words read from the standard input stored in an array.  The notion of "word" is the same to `cs1010_read_word()`.  
Returns NULL if there is a memory allocation error.  If the returned value is non-NULL, the caller is responsible for freeing the memory allocated for each word and for the whole array by calling `free`.

- `char** cs1010_read_line_array(int k)`<br>
Returns `k` new-line-separated words read from the standard input stored in an array.  The notion of line is the same to `cs1010_read_line()`.  
Returns NULL if there is a memory allocation error.  If the returned value is non-NULL, the caller is responsible for freeing the memory allocated for each line and for the whole array by calling `free`.

## Printing of a Single Value

The CS1010 library provide a few convenince functions to format and print `long` and `double` values to the standard output.

- `void cs1010_print_long(long value)` and `void cs1010_println_long(long value)`<br>
Print `value` to the standard output (with printf format `%ld`).  
The `cs1010_println_long` version prints a newline after the value.

- `void cs1010_print_double(double value)` and `void cs1010_println_double(double value)`<br>
Print `value` to the standard output (with printf format `%.4f`).
The `cs1010_println_double` version prints a newline after the value.

- `void cs1010_print_string(char *str)` and `void cs1010_println_string(char *str)`<br>
Print a given string `str` to the standard output.  These functions are provided for completeness and is a simple wrapper around `printf(str)` and `printf("%s\n", str)` repsectively.
