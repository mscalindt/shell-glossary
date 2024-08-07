## Collection 1: General POSIX `sh` Functions

### Overview

A collection of reusable pure POSIX `sh` functions with optional external
utility calls.

### Functions List:

- [ARG_SAVE()](https://github.com/mscalindt/shell-glossary/blob/main/src/arg_save): Single-quote append a string to a single-quote-escaped shell variable
- [CCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ccount): Count the times a character appears in a string
- [CHAR_EVEN()](https://github.com/mscalindt/shell-glossary/blob/main/src/char_even): Test if the sum of a character in a string is even
- [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary/blob/main/src/confirm_cont): Ask for confirmation to continue
- [COUNT_DIR()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dir): Count visible directories in a directory
- [COUNT_DIR_DOT()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_dir_dot): Count `.*` (dot/hidden) directories in a directory
- [COUNT_FILE()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_file): Count visible file objects in a directory
- [COUNT_FILE_DOT()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_file_dot): Count `.*` (dot/hidden) file objects in a directory
- [COUNT_OBJ()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_obj): Count all visible objects in a directory
- [COUNT_OBJ_DOT()](https://github.com/mscalindt/shell-glossary/blob/main/src/count_obj_dot): Count all `.*` (dot/hidden) objects in a directory
- [ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/err): Print formatted text to stderr
- [ERRF()](https://github.com/mscalindt/shell-glossary/blob/main/src/errF): Print red-colored formatted text to stderr and exit
- [ESC_SED()](https://github.com/mscalindt/shell-glossary/blob/main/src/esc_sed): Escape a string to be used literally as a POSIX `sed` string
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

## Collection 2: POSIX `sh` Functions for Shell Option Parsing

### Overview

A collection of reusable pure POSIX `sh` functions to make shell option parsing
a breeze.

### Functions List:

- [ASSERT()](https://github.com/mscalindt/shell-glossary/blob/main/src/assert): Assert a string
- [CONF_FAIL()](https://github.com/mscalindt/shell-glossary/blob/main/src/conf_fail): Signify bad configuration file and exit
- [COPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/copt): Match `-`-delimited short/long option with a mandatory option argument
- [COPT_OPTIONAL()](https://github.com/mscalindt/shell-glossary/blob/main/src/copt_optional): Match `-`-delimited short/long option with an optional option argument
- [FPATH()](https://github.com/mscalindt/shell-glossary/blob/main/src/fpath): Convert a path to absolute
- [OPD_MAX()](https://github.com/mscalindt/shell-glossary/blob/main/src/opd_max): Signify operand count exceeds maximum allowed
- [OPD_MIN()](https://github.com/mscalindt/shell-glossary/blob/main/src/opd_min): Signify operand count below minimum required
- [OPT_COMPAT_FAIL()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_compat_fail): Signify option incompatibility with a previous option and exit
- [OPT_ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_err): Signify option failure
- [OPT_FAIL()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_fail): Signify option failure and exit
- [OPT_INVALID()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_invalid): Signify invalid short option
- [OPT_UNKNOWN()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_unknown): Signify unknown option
- [OPT_UNRECOGNIZED()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_unrecognized): Signify unrecognized long option
- [SCOPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/scopt): True if specific complex (argument) option
- [SCOPT_OPTIONAL()](https://github.com/mscalindt/shell-glossary/blob/main/src/scopt_optional): True if specific optional-complex (possible argument) option
- [SOPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/sopt): True if simple (no argument) option
- [SSOPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ssopt): True if specific simple (no argument) option
