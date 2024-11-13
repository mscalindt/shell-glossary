## Collection 1: General POSIX `sh` Functions

A collection of reusable pure and very portable `sh` functions with optional
external utility calls.

### Functions List:

- [ARG_SAVE()](https://github.com/mscalindt/shell-glossary/blob/main/src/arg_save): Save with single-quote escape an argument into a variable
- [CCOUNT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ccount): Count the times a character appears in a string
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
- [FTYPE()](https://github.com/mscalindt/shell-glossary/blob/main/src/ftype): Identify the type of an object on the filesystem
- [INFO()](https://github.com/mscalindt/shell-glossary/blob/main/src/info): Print formatted text
- [MAP_FLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_fline): Process each populated file line with a function
- [MAP_PCHUNK()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pchunk): Process each cumulative path segment with a function
- [MAP_PCHUNK_FIND()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pchunk_find): Find success among cumulative path segments using a function
- [MAP_PCHUNK_FIND_REV()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pchunk_find_rev): Find success among cumulative path segments in reverse using a function
- [MAP_PCHUNK_REV()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pchunk_rev): Process each cumulative path segment in reverse with a function
- [MAP_PSEG()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pseg): Process each path segment with a function
- [MAP_PSEG_FIND()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pseg_find): Find success among path segments using a function
- [MAP_PSEG_REV()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_pseg_rev): Process each path segment in reverse with a function
- [MAP_VLINE()](https://github.com/mscalindt/shell-glossary/blob/main/src/map_vline): Process each populated string line with a function
- [MASK_CHECK()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_check): Check a bit, the power of 2, in a pseudo-bitmask shell variable
- [MASK_SET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_set): Set a bit, the power of 2, in a pseudo-bitmask shell variable
- [MASK_UNSET()](https://github.com/mscalindt/shell-glossary/blob/main/src/mask_unset): Unset a bit, the power of 2, in a pseudo-bitmask shell variable
- [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/num_to_char): Convert N to that amount of a character
- [PATH_SPEC()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec): Lexically normalize a path into an assertable canonical path
- [PATH_SPEC_FS()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec_fs): Semantically normalize a path into an assertable canonical path
- [PATH_SPEC_STATIC()](https://github.com/mscalindt/shell-glossary/blob/main/src/path_spec_static): Modify a path string
- [REPLCHAR()](https://github.com/mscalindt/shell-glossary/blob/main/src/replchar): Replace a specific character with character(s) in a string
- [SQ_ARG()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg): Get N evaluated argument in a single-quotes array of arguments
- [SQ_ARG_EXTRACT()](https://github.com/mscalindt/shell-glossary/blob/main/src/sq_arg_extract): Extract an evaluated argument in a single-quotes array of arguments

## Collection 2: POSIX `sh` Functions for Shell Option Parsing

A collection of reusable pure and very portable `sh` functions to make shell
option parsing a breeze.

### Functions List:

- [ABSPATH()](https://github.com/mscalindt/shell-glossary/blob/main/src/abspath): Convert a relative pathname into an absolute pathname
- [ASSERT()](https://github.com/mscalindt/shell-glossary/blob/main/src/assert): Assert a string
- [CFG_FAIL()](https://github.com/mscalindt/shell-glossary/blob/main/src/cfg_fail): Signify bad configuration file and exit
- [COPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/copt): Match `-`-delimited short/long option with a mandatory option argument
- [COPT_OPTIONAL()](https://github.com/mscalindt/shell-glossary/blob/main/src/copt_optional): Match `-`-delimited short/long option with an optional option argument
- [OPD_MAX()](https://github.com/mscalindt/shell-glossary/blob/main/src/opd_max): Signify operand count exceeds maximum allowed
- [OPD_MIN()](https://github.com/mscalindt/shell-glossary/blob/main/src/opd_min): Signify operand count below minimum required
- [OPT_COMPAT_FAIL()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_compat_fail): Signify option incompatibility with a previous option and exit
- [OPT_ERR()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_err): Signify option failure
- [OPT_FAIL()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_fail): Signify option failure and exit
- [OPT_INVALID()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_invalid): Signify invalid short option
- [OPT_UNKNOWN()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_unknown): Signify unknown option
- [OPT_UNRECOGNIZED()](https://github.com/mscalindt/shell-glossary/blob/main/src/opt_unrecognized): Signify unrecognized long option
- [SCOPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/scopt): Match specific option(s) with a mandatory option argument
- [SCOPT_OPTIONAL()](https://github.com/mscalindt/shell-glossary/blob/main/src/scopt_optional): Match specific option(s) with an optional option argument
- [SOPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/sopt): Match `-`-delimited short/long option without an option argument
- [SSOPT()](https://github.com/mscalindt/shell-glossary/blob/main/src/ssopt): Match specific option(s) without an option argument
