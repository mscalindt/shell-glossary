A collection of reusable pure POSIX `sh` functions with optional external
utility calls.

## Functions List:

- [CCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ccount)
- [CHARS_EVEN()](https://github.com/mscalindt/shell-glossary/blob/main/src/chars_even)
- [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary/blob/main/src/confirm_cont)
- [ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/err)
- [ERRF()](https://github.com/mscalindt/shell-glossary/blob/main/src/errF)
- [ESC_STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_str)
- [FCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/fcount)
- [FILE_PRELOAD()](https://github.com/mscalindt/shell-glossary/blob/main/src/file_preload)
- [FLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/fline_parse)
- [GET_FPATH()](https://github.com/mscalindt/shell-glossary/blob/main/src/get_fpath)
- [INFO()](https://github.com/mscalindt/shell-glossary/blob/main/src/info)
- [LSTRIP()](https://github.com/mscalindt/shell-glossary/blob/main/src/lstrip)
- [LTL_SUBSTR0()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltl_substr0)
- [LTL_SUBSTR1()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltl_substr1)
- [LTR_SUBSTR0()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltr_substr0)
- [LTR_SUBSTR1()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltr_substr1)
- [MASK_CHECK()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_check)
- [MASK_SET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_set)
- [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/num_to_char)
- [PATH_SPEC()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec)
- [PLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/pline)
- [PLINE_FD1()](https://github.com/mscalindt/shell-glossary/blob/main/src/pline_fd1)
- [REMCHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/remchars)
- [REMSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/remstr)
- [REPLCHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/replchars)
- [REPLSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replstr)
- [RSTRIP()](https://github.com/mscalindt/shell-glossary/blob/main/src/rstrip)
- [SQ_ARG()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg)
- [STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/str)
- [STR_FD1()](https://github.com/mscalindt/shell-glossary/blob/main/src/str_fd1)
- [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/str_to_chars)
- [VLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/vline_parse)

## Brief Descriptions:

### [CCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ccount)
Count the times a character appears in a string

### [CHARS_EVEN()](https://github.com/mscalindt/shell-glossary/blob/main/src/chars_even)
Test if the character(s) count (combined), in a given string, is even

### [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary/blob/main/src/confirm_cont)
Ask for confirmation to continue

### [ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/err)
Print formatted text to stderr

### [ERRF()](https://github.com/mscalindt/shell-glossary/blob/main/src/errF)
Print red-colored formatted text to stderr and exit

### [ESC_STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_str)
Escape shell-defined meta characters in a string

### [FCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/fcount)
Count files and directories in a directory

### [FILE_PRELOAD()](https://github.com/mscalindt/shell-glossary/blob/main/src/file_preload)
Read a file raw into $_file

### [FLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/fline_parse)
Process each line of a file using a specified function

### [GET_FPATH()](https://github.com/mscalindt/shell-glossary/blob/main/src/get_fpath)
Convert relative path to absolute path

### [INFO()](https://github.com/mscalindt/shell-glossary/blob/main/src/info)
Print formatted text to stdout

### [LSTRIP()](https://github.com/mscalindt/shell-glossary/blob/main/src/lstrip)
Strip character(s) from left of string

### [LTL_SUBSTR0()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltl_substr0)
Get positional substring, (left of) (N)character(s), in a string

### [LTL_SUBSTR1()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltl_substr1)
Get positional substring, (left of, up to) (N)character(s), in a string

### [LTR_SUBSTR0()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltr_substr0)
Get positional substring, (right of) (N)character(s), in a string

### [LTR_SUBSTR1()](https://github.com/mscalindt/shell-glossary/blob/main/src/ltr_substr1)
Get positional substring, (right of, up to) (N)character(s), in a string

### [MASK_CHECK()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_check)
Check a bit, the power of 2, in a pseudo-bitmask shell variable

### [MASK_SET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_set)
Set a bit, the power of 2, for a pseudo-bitmask shell variable

### [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/num_to_char)
Convert N to that amount of a character

### [PATH_SPEC()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec)
Mangle and assert a path in a specified way

### [PLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/pline)
Print specific line in file

### [PLINE_FD1()](https://github.com/mscalindt/shell-glossary/blob/main/src/pline_fd1)
Print specific line in stdin

### [REMCHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/remchars)
Remove specific character(s) in string

### [REMSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/remstr)
Remove a positional substring of a string

### [REPLCHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/replchars)
Replace specific character(s) with character(s) in string

### [REPLSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replstr)
Replace a substring of a string with character(s)

### [RSTRIP()](https://github.com/mscalindt/shell-glossary/blob/main/src/rstrip)
Strip character(s) from right of string

### [SQ_ARG()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg)
Get a single-quoted argument in a single-quote-escaped string

### [STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/str)
Check the existence/position of a substring in string

### [STR_FD1()](https://github.com/mscalindt/shell-glossary/blob/main/src/str_fd1)
Check the existence/position of a substring in stdin

### [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/str_to_chars)
Convert a string to whitespace-separated characters

### [VLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/vline_parse)
Process each populated line of a variable (string) using a specified function
