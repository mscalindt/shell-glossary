A collection of reusable pure POSIX `sh` functions to make shell option parsing
a breeze.

## Functions List:

- [ASSERT()](https://github.com/mscalindt/main-functions/blob/main/src/assert): Assert a string
- [CONF_FAIL()](https://github.com/mscalindt/main-functions/blob/main/src/conf_fail): Signify bad configuration file and exit
- [COPT()](https://github.com/mscalindt/main-functions/blob/main/src/copt): True if complex (argument) option
- [COPT_OPTIONAL()](https://github.com/mscalindt/main-functions/blob/main/src/copt_optional): True if optional-complex (possible argument) option
- [FPATH()](https://github.com/mscalindt/main-functions/blob/main/src/fpath): Get the absolute path into $_fpath
- [OPD_MAX()](https://github.com/mscalindt/main-functions/blob/main/src/opd_max): Signify invalid operand count (exceeds maximum allowed); ($1 > $2)
- [OPD_MIN()](https://github.com/mscalindt/main-functions/blob/main/src/opd_min): Signify insufficient operand count (below minimum required); ($1 < $2)
- [OPT_COMPAT_FAIL()](https://github.com/mscalindt/main-functions/blob/main/src/opt_compat_fail): Signify option incompatibility with a previous option and exit
- [OPT_ERR()](https://github.com/mscalindt/main-functions/blob/main/src/opt_err): Signify failed option
- [OPT_FAIL()](https://github.com/mscalindt/main-functions/blob/main/src/opt_fail): Signify failed option and exit
- [OPT_INVALID()](https://github.com/mscalindt/main-functions/blob/main/src/opt_invalid): Signify invalid (`-`...) option
- [OPT_UNKNOWN()](https://github.com/mscalindt/main-functions/blob/main/src/opt_unknown): Signify unknown (*) option
- [OPT_UNRECOGNIZED()](https://github.com/mscalindt/main-functions/blob/main/src/opt_unrecognized): Signify unrecognized (`--`...) option
- [SCOPT()](https://github.com/mscalindt/main-functions/blob/main/src/scopt): True if specific complex (argument) option
- [SCOPT_OPTIONAL()](https://github.com/mscalindt/main-functions/blob/main/src/scopt_optional): True if specific optional-complex (possible argument) option
- [SOPT()](https://github.com/mscalindt/main-functions/blob/main/src/sopt): True if simple (no argument) option
- [SSOPT()](https://github.com/mscalindt/main-functions/blob/main/src/ssopt): True if specific simple (no argument) option
