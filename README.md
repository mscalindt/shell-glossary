## Table of Contents

1. [Introduction](#introduction)
2. [Roadmap](#roadmap)
3. [Contributors](#contributors)
4. [License](#license)
5. [Notice](#notice)
6. [Collection 1: General POSIX Shell Functions](#collection-1-general-posix-shell-functions)
7. [Collection 2: POSIX Shell Functions for Shell Option Parsing](#collection-2-posix-shell-functions-for-shell-option-parsing)

## Introduction

[`shell-glossary`](https://github.com/mscalindt/shell-glossary) is a glossary
of reusable POSIX shell functions. It features collections of optimized
functions for general use.

Currently, the state of the functions is considered unstable until certain
criteria in [Roadmap](#roadmap) are met.

## Roadmap

* Portable C89 in-house program to follow/resolve symbolic links.
* Unit tests (`wunit`).
  * Transition to versioned release tags.
  * Makefile to run the unit tests.
* Benchmarks in `bash`, `dash`, `ksh`.
* Shell code style guide.
  * Assert code for style.
* Assertable function documentation header syntax.
  * Assertable function documentation header.
* Manpage of `shell-glossary`.
  * Provide manpage equivalents of the function documentation headers.
* Makefile to build src/ into a `shell-glossary` file.

For updates on recent changes, see the [NEWS](NEWS) file.

## Contributors

Contribution guidelines will be published in the future. At this time,
external contributions are not accepted.

For a list of people who have contributed to `shell-glossary`,
see the [CREDITS](CREDITS) file.

## License

[BSD 2-Clause "Simplified" License](LICENSE)

For a list of external dependencies and their licenses,
refer to the [DEPENDENCIES](DEPENDENCIES) file.

## Notice

This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

For detailed information on external dependencies,
see the [DEPENDENCIES](DEPENDENCIES) file.

---

## Collection 1: General POSIX Shell Functions

A collection of reusable pure and very portable shell functions
with optional external utility calls.

- [ARG_SAVE()](src/arg_save):
Save with single-quote escape an argument into a variable
- [ARG_SET()](src/arg_set):
Set with single-quote escape an argument into a variable
- [CCOUNT()](src/ccount):
Count the times character(s) appear in a string using IFS
- [CCOUNT_POSIX()](src/ccount_posix):
Count the times a character appears in a string using PE
- [CONFIRM_CONT()](src/confirm_cont):
Assert confirmation to continue
- [COUNT_DIR()](src/count_dir):
Count visible directories in a directory
- [COUNT_DIR_DOT()](src/count_dir_dot):
Count `.*` (dot/hidden) directories in a directory
- [COUNT_FILE()](src/count_file):
Count visible file objects in a directory
- [COUNT_FILE_DOT()](src/count_file_dot):
Count `.*` (dot/hidden) file objects in a directory
- [COUNT_OBJ()](src/count_obj):
Count all visible objects in a directory
- [COUNT_OBJ_DOT()](src/count_obj_dot):
Count all `.*` (dot/hidden) objects in a directory
- [ERR()](src/err):
Print formatted text to stderr
- [ESC_SED()](src/esc_sed):
Escape a string to be used literally as a POSIX `sed` string
- [FED()](src/fed):
Create a deterministic file data edit format
- [FILE_PRELOAD()](src/file_preload):
Read a file raw
- [FLINE()](src/fline):
Get a specific line in a file
- [FLINE_MAP()](src/fline_map):
Process each populated file line with a function
- [FTYPE()](src/ftype):
Identify the type of an object on the filesystem
- [IFS_MAP()](src/ifs_map):
Process each populated IFS field split with a function
- [INFO()](src/info):
Print formatted text
- [LIBFILE()](src/libfile):
Modify regular file data in-memory using a deterministic format
- [LIBFILE_N()](src/libfile_n):
Deterministically modify regular file data in-memory
- [LIBFILE_N_ADD()](src/libfile_n_add):
For natural N, add line content
- [LIBFILE_N_REM()](src/libfile_n_rem):
For natural N, remove line string
- [MAP()](src/map):
Process each argument with a function
- [MAP_FIND()](src/map_find):
Find success among arguments using a function
- [MAP_FIND_REV()](src/map_find_rev):
Find success among arguments in reverse using a function
- [MAP_PCHUNK()](src/map_pchunk):
Process each cumulative path segment with a function
- [MAP_PCHUNK_FIND()](src/map_pchunk_find):
Find success among cumulative path segments using a function
- [MAP_PCHUNK_FIND_REV()](src/map_pchunk_find_rev):
Find success among cumulative path segments in reverse using a function
- [MAP_PCHUNK_REV()](src/map_pchunk_rev):
Process each cumulative path segment in reverse with a function
- [MAP_REV()](src/map_rev):
Process each argument in reverse with a function
- [MAP_TRY()](src/map_try):
Process each argument with a function irrespective of rc
- [MAP_TRY_REV()](src/map_try_rev):
Process each argument in reverse with a function irrespective of rc
- [MASK_CHECK()](src/mask_check):
Assert a bit, the power of 2, in a pseudo-bitmask shell variable
- [MASK_SET()](src/mask_set):
Set a bit, the power of 2, in a pseudo-bitmask shell variable
- [MASK_UNSET()](src/mask_unset):
Unset a bit, the power of 2, in a pseudo-bitmask shell variable
- [NUM_TO_CHAR()](src/num_to_char):
Create N amount of character(s) using PE
- [OFFSET()](src/offset):
Offset arguments by N for a function call
- [PATH_SPEC()](src/path_spec):
Lexically normalize a path into an assertable canonical path
- [PATH_SPEC_FS()](src/path_spec_fs):
Semantically normalize a path into an assertable canonical path
- [PATH_SPEC_STATIC()](src/path_spec_static):
Modify a path string
- [PATH_STRIP()](src/path_strip):
Strip a path N segments RTL
- [REPLCHAR()](src/replchar):
Replace specific character(s) with character(s) in a string using IFS
- [REPLCHAR_POSIX()](src/replchar_posix):
Replace a specific character with character(s) in a string using PE
- [REPLSTR_SED_POSIX()](src/replstr_sed_posix):
Replace a substring with character(s) in a string using `sed`
- [SLINE()](src/sline):
Get a specific line in a string
- [SLINE_MAP()](src/sline_map):
Process each populated string line with a function
- [SQ_ARG()](src/sq_arg):
Get N evaluated argument in a single-quotes array of arguments
- [SQ_ARG_EXTRACT()](src/sq_arg_extract):
Extract an evaluated argument in a single-quotes array of arguments

## Collection 2: POSIX Shell Functions for Shell Option Parsing

A collection of reusable pure and very portable shell functions
to make shell option parsing a breeze.

- [ABSPATH()](src/abspath):
Convert a relative pathname into an absolute pathname
- [ASSERT()](src/assert):
Assert a string
- [CFG_FAIL()](src/cfg_fail):
Signify bad configuration file and exit
- [COPT()](src/copt):
Match `-`-delimited short/long option with a mandatory option argument
- [COPT_OPTIONAL()](src/copt_optional):
Match `-`-delimited short/long option with an optional option argument
- [OPD_MAX()](src/opd_max):
Signify operand count exceeds maximum allowed
- [OPD_MIN()](src/opd_min):
Signify operand count below minimum required
- [OPT_COMPAT_FAIL()](src/opt_compat_fail):
Signify option incompatibility with a previous option and exit
- [OPT_ERR()](src/opt_err):
Signify option failure
- [OPT_FAIL()](src/opt_fail):
Signify option failure and exit
- [OPT_INVALID()](src/opt_invalid):
Signify invalid short option
- [OPT_UNKNOWN()](src/opt_unknown):
Signify unknown option
- [OPT_UNRECOGNIZED()](src/opt_unrecognized):
Signify unrecognized long option
- [SCOPT()](src/scopt):
Match specific option(s) with a mandatory option argument
- [SCOPT_OPTIONAL()](src/scopt_optional):
Match specific option(s) with an optional option argument
- [SOPT()](src/sopt):
Match `-`-delimited short/long option without an option argument
- [SSOPT()](src/ssopt):
Match specific option(s) without an option argument
