A collection of reusable pure POSIX `sh` functions with optional external
utility calls.

## Functions List:

- [CCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ccount)
- [CHARS_EVEN()](https://github.com/mscalindt/shell-glossary/blob/main/src/chars_even)
- [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary/blob/main/src/confirm_cont)
- [COUNT_DDIRS()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_ddirs)
- [COUNT_DFILES()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dfiles)
- [COUNT_DIRS()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dirs)
- [COUNT_DOBJ()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dobj)
- [COUNT_FILES()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_files)
- [COUNT_OBJ()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_obj)
- [ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/err)
- [ERRF()](https://github.com/mscalindt/shell-glossary/blob/main/src/errF)
- [ESC_SED()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_sed)
- [ESC_STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_str)
- [FILE_PRELOAD()](https://github.com/mscalindt/shell-glossary/blob/main/src/file_preload)
- [FLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/fline)
- [FLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/fline_parse)
- [INFO()](https://github.com/mscalindt/shell-glossary/blob/main/src/info)
- [MASK_CHECK()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_check)
- [MASK_SET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_set)
- [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/num_to_char)
- [PATH_SPEC()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec)
- [REMSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/remstr)
- [REPLCHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replchar)
- [REPLSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replstr)
- [SQ_ARG()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg)
- [STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/str)
- [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/str_to_chars)
- [VLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/vline_parse)

## Brief Descriptions:

### [CCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ccount)
Count the times a character appears in a string

### [CHARS_EVEN()](https://github.com/mscalindt/shell-glossary/blob/main/src/chars_even)
Test if the sum of a character in a string is even

### [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary/blob/main/src/confirm_cont)
Ask for confirmation to continue

### [COUNT_DDIRS()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_ddirs)
Count all hidden (.*) directories in a directory

### [COUNT_DFILES()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dfiles)
Count all hidden (.*) file objects in a directory

### [COUNT_DIRS()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dirs)
Count all visible directories in a directory

### [COUNT_DOBJ()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dobj)
Count all hidden (.*) objects in a directory

### [COUNT_FILES()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_files)
Count all visible file objects in a directory

### [COUNT_OBJ()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_obj)
Count all visible objects in a directory

### [ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/err)
Print formatted text to stderr

### [ERRF()](https://github.com/mscalindt/shell-glossary/blob/main/src/errF)
Print red-colored formatted text to stderr and exit

### [ESC_SED()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_sed)
Escape a string to be used literally as a POSIX `sed` pattern

### [ESC_STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_str)
Escape meta characters in a string

### [FILE_PRELOAD()](https://github.com/mscalindt/shell-glossary/blob/main/src/file_preload)
Read a file raw

### [FLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/fline)
Get a specific line in a file

### [FLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/fline_parse)
Process each line of a file using a specified function

### [INFO()](https://github.com/mscalindt/shell-glossary/blob/main/src/info)
Print formatted text to stdout

### [MASK_CHECK()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_check)
Check a bit, the power of 2, in a pseudo-bitmask shell variable

### [MASK_SET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_set)
Set a bit, the power of 2, for a pseudo-bitmask shell variable

### [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/num_to_char)
Convert N to that amount of a character

### [PATH_SPEC()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec)
Mangle and assert a path in a specified way

### [REMSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/remstr)
Remove a positional substring of a string

### [REPLCHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replchar)
Replace specific character(s) with character(s) in string

### [REPLSTR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replstr)
Replace a substring of a string with character(s)

### [SQ_ARG()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg)
Get a single-quoted argument in a single-quote-escaped string

### [STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/str)
Check the existence/position of a substring in string

### [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary/blob/main/src/str_to_chars)
Convert a string to whitespace-separated characters

### [VLINE_PARSE()](https://github.com/mscalindt/shell-glossary/blob/main/src/vline_parse)
Process each populated line of a variable (string) using a specified function
