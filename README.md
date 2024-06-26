A collection of reusable pure POSIX `sh` functions with optional external
utility calls.

## Functions List:

- [CCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ccount): Count the times a character appears in a string
- [CHAR_EVEN()](https://github.com/mscalindt/shell-glossary/blob/main/src/char_even): Test if the sum of a character in a string is even
- [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary/blob/main/src/confirm_cont): Ask for confirmation to continue
- [COUNT_DDIRS()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_ddirs): Count hidden (.*) directories in a directory
- [COUNT_DFILES()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dfiles): Count hidden (.*) file objects in a directory
- [COUNT_DIRS()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dirs): Count visible directories in a directory
- [COUNT_DOBJ()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dobj): Count all hidden (.*) objects in a directory
- [COUNT_FILES()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_files): Count visible file objects in a directory
- [COUNT_OBJ()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_obj): Count all visible objects in a directory
- [ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/err): Print formatted text to stderr
- [ERRF()](https://github.com/mscalindt/shell-glossary/blob/main/src/errF): Print red-colored formatted text to stderr and exit
- [ESC_SED()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_sed): Escape a string to be used literally as a POSIX `sed` pattern
- [ESC_STR()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_str): Backslash escape character(s) in a string
- [FILE_PRELOAD()](https://github.com/mscalindt/shell-glossary/blob/main/src/file_preload): Read a file raw
- [FLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/fline): Get a specific line in a file
- [INFO()](https://github.com/mscalindt/shell-glossary/blob/main/src/info): Print formatted text
- [MAP_FLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_fline): Process each line of a file with a function
- [MAP_PSEG()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pseg): Process all segments of a path with a function
- [MAP_VLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_vline): Process all populated lines of a variable (string) with a function
- [MASK_CHECK()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_check): Check a bit, the power of 2, in a pseudo-bitmask shell variable
- [MASK_SET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_set): Set a bit, the power of 2, in a pseudo-bitmask shell variable
- [MASK_UNSET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_unset): Unset a bit, the power of 2, in a pseudo-bitmask shell variable
- [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/num_to_char): Convert N to that amount of a character
- [PATH_SPEC()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec): Normalize a path into an assertable path specification
- [REPLCHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replchar): Replace a specific character with character(s) in a string
- [SQ_ARG()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg): Get N single-quoted argument in a single-quote-escaped string
- [SQ_ARG_EXTRACT()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg_extract): Extract a single-quoted argument in a single-quote-escaped string
