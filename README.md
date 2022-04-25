A collection of reusable pure POSIX `sh` functions with no external binary calls (on somewhat modern shells where `read`, `printf`, and `echo` are builtins).

# Normal functions:

* [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary#confirm_cont)
* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [ERR_NE()](https://github.com/mscalindt/shell-glossary#err_ne)
* [ERR_FMT()](https://github.com/mscalindt/shell-glossary#err_fmt)
* [ERR_NE_FMT()](https://github.com/mscalindt/shell-glossary#err_ne_fmt)
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/12
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/6
* [GREP_STR()](https://github.com/mscalindt/shell-glossary#grep_str) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/7.1
* [INFO()](https://github.com/mscalindt/shell-glossary#info)
* [INFO_FMT()](https://github.com/mscalindt/shell-glossary#info_fmt)
* [LSTRIP()](https://github.com/mscalindt/shell-glossary#lstrip) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/8
* [LTL_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltl_substr0) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/15
* [LTL_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltl_substr1) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/16
* [LTR_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltr_substr0) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/17
* [LTR_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltr_substr1) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/18
* [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary#num_to_char)
* [PARSE()](https://github.com/mscalindt/shell-glossary#parse) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/13
* [PLINE()](https://github.com/mscalindt/shell-glossary#pline) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/14
* [REMCHARS()](https://github.com/mscalindt/shell-glossary#remchars) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/5.1
* [REMSTR()](https://github.com/mscalindt/shell-glossary#remstr) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/3.4
* [REPLSTR()](https://github.com/mscalindt/shell-glossary#replstr) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/4.2
* [RSTRIP()](https://github.com/mscalindt/shell-glossary#rstrip) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/9
* [ESC_STR()](https://github.com/mscalindt/shell-glossary#esc_str) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/11
* [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary#str_to_chars) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/10
* [WARN()](https://github.com/mscalindt/shell-glossary#warn)
* [WARN_FMT()](https://github.com/mscalindt/shell-glossary#warn_fmt)

# Normal functions (color):

* [CONFIRM_CONT_CLR()](https://github.com/mscalindt/shell-glossary#confirm_cont_clr)
* [ERR_CLR()](https://github.com/mscalindt/shell-glossary#err_clr)
* [ERR_NE_CLR()](https://github.com/mscalindt/shell-glossary#err_ne_clr)
* [ERR_FMT_CLR()](https://github.com/mscalindt/shell-glossary#err_fmt_clr)
* [ERR_NE_FMT_CLR()](https://github.com/mscalindt/shell-glossary#err_ne_fmt_clr)
* [INFO_CLR()](https://github.com/mscalindt/shell-glossary#info_clr)
* [INFO_FMT_CLR()](https://github.com/mscalindt/shell-glossary#info_fmt_clr)
* [WARN_CLR()](https://github.com/mscalindt/shell-glossary#warn_clr)
* [WARN_FMT_CLR()](https://github.com/mscalindt/shell-glossary#warn_fmt_clr)

# Stdin functions:

* [GREP_STR_FD1()](https://github.com/mscalindt/shell-glossary#grep_str_fd1)
* [PARSE_FD1()](https://github.com/mscalindt/shell-glossary#parse_fd1)
* [PLINE_FD1()](https://github.com/mscalindt/shell-glossary#pline_fd1)

## confirm_cont

```sh
# Description:
# Ask for confirmation to continue
#
# Parameters:
# <$1> - mode('0' - default action: Y,
#             '1' - default action: N)
#
# Returns:
# (0) permitted
# (1) forbidden
#
confirm_cont() {
    case $1 in
    0)
        printf "Continue? [Y/n] "
        read -r _action

        case "$_action" in
            N*|n*) return 1 ;;
        esac

        return 0
    ;;
    1)
        printf "Continue? [y/N] "
        read -r _action

        case "$_action" in
            Y*|y*) return 0 ;;
        esac

        return 1
    ;;
    esac
}
```

## confirm_cont_clr

```sh
# Description:
# Colorfully ask for confirmation to continue
#
# Parameters:
# <$1> - mode('0' - default action: Y,
#             '1' - default action: N)
#
# Returns:
# (0) permitted
# (1) forbidden
#
confirm_cont_clr() {
    case $1 in
    0)
        printf "%bContinue? [Y/n]%b " "\033[1;37m" "\033[0m"
        read -r _action

        case "$_action" in
            N*|n*) return 1 ;;
        esac

        return 0
    ;;
    1)
        printf "%bContinue? [y/N]%b " "\033[1;37m" "\033[0m"
        read -r _action

        case "$_action" in
            Y*|y*) return 0 ;;
        esac

        return 1
    ;;
    esac
}
```

## err

```sh
# Description:
# Print error and exit
#
# Parameters:
# <$1> - exit code
# <"$2"+> - text
#
err() {
    _rc=$1; shift
    printf "\nERROR: %s\n\n" "$*" 1>&2
    exit $_rc
}
```

## err_clr

```sh
# Description:
# Print colorful error and exit
#
# Parameters:
# <$1> - exit code
# <"$2"+> - text
#
err_clr() {
    _rc=$1; shift
    printf "\n%bERROR:%b %s\n\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit $_rc
}
```

## err_ne

```sh
# Description:
# Print error, no exit
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) text
#
err_ne() {
    printf "ERROR: %s\n" "$*" 1>&2
}
```

## err_ne_clr

```sh
# Description:
# Print colorful error, no exit
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) text
#
err_ne_clr() {
    printf "%bERROR:%b %s\n" "\033[1;31m" "\033[0m" "$*" 1>&2
}
```

## err_fmt

```sh
# Description:
# Print error with printf format before text and exit
#
# Parameters:
# <$1> - exit code
# <"$2"> - printf format
# <"$3"+> - text
#
err_fmt() {
    _rc=$1; _printf_fmt="$2"; shift 2
    printf "\nERROR: ${_printf_fmt}%s\n\n" "$*" 1>&2
    exit $_rc
}
```

## err_fmt_clr

```sh
# Description:
# Print colorful error with printf format before text and exit
#
# Parameters:
# <$1> - exit code
# <"$2"> - printf format
# <"$3"+> - text
#
err_fmt_clr() {
    _rc=$1; _printf_fmt="$2"; shift 2
    printf "\n%bERROR:%b ${_printf_fmt}%s\n\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit $_rc
}
```

## err_ne_fmt

```sh
# Description:
# Print error with printf format before text, no exit
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) text
#
err_ne_fmt() {
    _printf_fmt="$1"; shift
    printf "ERROR: ${_printf_fmt}%s\n" "$*" 1>&2
}
```

## err_ne_fmt_clr

```sh
# Description:
# Print colorful error with printf format before text, no exit
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) text
#
err_ne_fmt_clr() {
    _printf_fmt="$1"; shift
    printf "%bERROR:%b ${_printf_fmt}%s\n" "\033[1;31m" "\033[0m" "$*" 1>&2
}
```

## fcount

```sh
# Description:
# Count files and directories in a directory
#
# Parameters:
# <"$1"> - directory
# ["$2"] - mode("0X" - count files and directories ending with "X",
#               '1' - count only files,
#               "1X" - count only files ending with "X",
#               '2' - count only directories,
#               "2X" - count only directories ending with "X")
# [$3] - mode('3' - exclude hidden files/directories)
#
# Returns:
# (0) count
# (1) not a directory | directory does not exist
#
# Caveats:
# 1. The number of files/directories the function (i.e. system) can process
#    varies. The files/directories are stored as arguments, which means that the
#    amount of things that can be processed depends on the system and version,
#    on the number of files and their respective argument size, and on the
#    number and size of environment variable names. For more information,
#    related limits shall be checked: ARG_MAX.
#    Fix: none; not applicable.
#
fcount() {
    [ -d "$1" ] || return 1

    _dir="$1"
    _count=0

    case "$2" in
        0*|1*|2*) _sfix="${2#?}" ;;
    esac

    case $#:"$2" in
        1:)
            set -- "$_dir"/*
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/.*
            [ $# -ge 3 ] && _count=$((_count + $# - 2))
        ;;
        2:0*)
            set -- "$_dir"/*"$_sfix"
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/.*"$_sfix"
            case "$_sfix" in
                ".")
                    [ -e "$2" ] && _count=$((_count + $# - 1))
                ;;
                "..")
                    [ -e "$1" ] && _count=$((_count + $#))
                ;;
                *)
                    [ -e "$_dir/$_sfix" ] && _count=$((_count + 1))
                    [ -e "$1" ] && _count=$((_count + $#))
                ;;
            esac
        ;;
        2:1)
            set -- "$_dir"/*
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/*/
            [ -e "$1" ] && _count=$((_count - $#))

            set -- "$_dir"/.*
            [ -e "$3" ] && _count=$((_count + $# - 2))

            set -- "$_dir"/.*/
            [ -e "$3" ] && _count=$((_count - $# + 2))
        ;;
        2:1*)
            [ -f "$_dir/$_sfix" ] && _count=1

            set -- "$_dir"/*"$_sfix"
            [ -e "$1" ] && _count=$((_count + $#))

            set -- "$_dir"/.*"$_sfix"
            case "$_sfix" in
                ".") [ -e "$2" ] && _count=$((_count + $# - 1)) ;;
                *) [ -e "$1" ] && _count=$((_count + $#)) ;;
            esac

            set -- "$_dir"/.*"$_sfix"/
            case "$_sfix" in
                ".") [ -e "$2" ] && _count=$((_count - $# + 1)) ;;
                *) [ -e "$1" ] && _count=$((_count - $#)) ;;
            esac

            set -- "$_dir"/*"$_sfix"/
            [ -e "$1" ] && _count=$((_count - $#))
        ;;
        2:2)
            set -- "$_dir"/*/
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/.*/
            [ -e "$3" ] && _count=$((_count + $# - 2))
        ;;
        2:2*)
            set -- "$_dir"/*"$_sfix"/
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/.*"$_sfix"/
            case "$_sfix" in
                ".")
                    [ -e "$2" ] && _count=$((_count + $# - 1))
                ;;
                "..")
                    [ -e "$1" ] && _count=$((_count + $#))
                ;;
                *)
                    [ -d "$_dir/$_sfix" ] && _count=$((_count + 1))
                    [ -e "$1" ] && _count=$((_count + $#))
                ;;
            esac
        ;;
        2:3)
            set -- "$_dir"/*
            [ -e "$1" ] && _count=$#
        ;;
        3:0*)
            set -- "$_dir"/*"$_sfix"
            [ -e "$1" ] && _count=$#
        ;;
        3:1)
            set -- "$_dir"/*
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/*/
            [ -e "$1" ] && _count=$((_count - $#))
        ;;
        3:1*)
            set -- "$_dir"/*"$_sfix"
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/*"$_sfix"/
            [ -e "$1" ] && _count=$((_count - $#))
        ;;
        3:2)
            set -- "$_dir"/*/
            [ -e "$1" ] && _count=$#
        ;;
        3:2*)
            set -- "$_dir"/*"$_sfix"/
            [ -e "$1" ] && _count=$#
        ;;
    esac

    printf "%d" "$_count"
}
```

## get_fpath

```sh
# Description:
# Convert relative path to absolute path
#
# Parameters:
# <"$1"> - path
#
# Returns:
# (0) absolute $1 | $1
#
get_fpath() {
    case "$1" in
        /*) printf "%s" "$1" ;;
        *) printf "%s/%s" "$PWD" "$1" ;;
    esac
}
```

## grep_str

```sh
# Description:
# Check the existence/position of a substring in string
#
# Parameters:
# <"$1"> - substring
# <"$2"> - string
# [$3] - mode('1' - $1 is first character(s) of $2,
#             '2' - $1 is last character(s) of $2,
#             '3' - $1 is, on its own, $2)
#
# Returns:
# (0) match
# (1) no match
#
grep_str() {
    case $#:$3 in
        2:) case "$2" in *"$1"*) return 0 ;; esac ;;
        3:1) case "$2" in "$1"*) return 0 ;; esac ;;
        3:2) case "$2" in *"$1") return 0 ;; esac ;;
        3:3) case "$2" in "$1") return 0 ;; esac ;;
    esac

    return 1
}
```

## grep_str_fd1

```sh
# Description:
# Check the existence/position of a substring in stdin
#
# Parameters:
# <"$1"> - substring
# [$2] - mode('1' - $1 is first character(s) of stdin,
#             '2' - $1 is last character(s) of stdin,
#             '3' - $1 is, on its own, stdin)
#
# Returns:
# (0) match
# (1) no match
#
# Caveats:
# 1. NULL character.
#
grep_str_fd1() {
    case $#:$2 in
        1:)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in *"$1"*) return 0 ;; esac
            done
        ;;
        2:1)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in "$1"*) return 0 ;; esac
            done
        ;;
        2:2)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in *"$1") return 0 ;; esac
            done
        ;;
        2:3)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in "$1") return 0 ;; esac
            done
        ;;
    esac

    return 1
}
```

## info

```sh
# Description:
# Print info
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) text
#
info() {
    printf "INFO: %s\n" "$*"
}
```

## info_clr

```sh
# Description:
# Print colorful info
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) text
#
info_clr() {
    printf "%bINFO:%b %s\n" "\033[1;37m" "\033[0m" "$*"
}
```

## info_fmt

```sh
# Description:
# Print info with printf format before text
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) text
#
info_fmt() {
    _printf_fmt="$1"; shift
    printf "INFO: ${_printf_fmt}%s\n" "$*"
}
```

## info_fmt_clr

```sh
# Description:
# Print colorful info with printf format before text
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) text
#
info_fmt_clr() {
    _printf_fmt="$1"; shift
    printf "%bINFO:%b ${_printf_fmt}%s\n" "\033[1;37m" "\033[0m" "$*"
}
```

## lstrip

```sh
# Description:
# Strip character(s) from left of string
#
# Parameters:
# <"$1"> - character(s)
# <"$2"> - string
#
# Returns:
# (0) stripped $2
# (1) ! $1
# (2) $1 = $2
#
lstrip() {
    case "$2" in
        "$1") return 2 ;;
        "$1"*) printf "%s" "${2#"$1"}"; return 0 ;;
    esac

    return 1
}
```

## ltl_substr0

```sh
# Description:
# Get positional substring, (from LTL) (N)character(s), in a string
#
# Parameters:
# <$1> - from "N" LTR character('0' - max)
# <"$2"> - from "X" character(s)
# <"$3"> - string
# [$4] - mode('0' - strip all leading whitespace characters,
#             '1' - strip all trailing whitespace characters,
#             '2' - strip all leading/trailing whitespace characters)
# [$5] - mode('3' - keep $2)
# [$6] - mode('4' - verify the expansion)
#
# Returns:
# (0) substring | incorrect substring ($1 > $2)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
#
# Returns (mode '4'):
# (0) substring
# (1) empty expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
#
ltl_substr0() {
    _str="$3"

    case $#:$5$4 in
        6:*|5:4*|4:4)
            case $1 in
                0)
                    case "$_str" in
                        *"$2"*"$2"*) : ;;
                        "$2"*) return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _str="${_str%"$2"*}"
                ;;
                *)
                    case $1"$_str" in
                        1"$2"*) return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _sfix="$_str"; _i=0; until [ $_i -eq $1 ]; do
                        _sfix="${_sfix#*"$2"}"

                        case "$_sfix" in
                            *"$2"*) : ;;
                            *) [ $((_i + 1)) -eq $1 ] || return 3 ;;
                        esac

                        _i=$((_i + 1))
                    done

                    _str="${_str%"$2$_sfix"}"
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    _str="${_str%"$2"*}"
                ;;
                *)
                    _sfix="$_str"; _i=0; until [ $_i -eq $1 ]; do
                        _sfix="${_sfix#*"$2"}"
                        _i=$((_i + 1))
                    done

                    _str="${_str%"$2$_sfix"}"
                ;;
            esac

            [ "$_str" = "$3" ] && return 2
        ;;
    esac

    [ "$_str" ] || return 1

    case $4 in
        0)
            _str="${_str#${_str%%[![:space:]]*}}"
        ;;
        1)
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
        2)
            _str="${_str#${_str%%[![:space:]]*}}"
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
    esac

    case $5$4 in
        *3*) _str="$_str$2" ;;
    esac

    printf "%s" "$_str"
}
```

## ltl_substr1

```sh
# Description:
# Get positional substring, (from&to LTL) (N)character(s), in a string
#
# Parameters:
# <$1> - from "N" LTR character('0' - max)
# <"$2"> - from "X" character(s)
# <$3> - to "N" $2-TL character('0' - max)
# <"$4"> - to "X" character(s)
# <"$5"> - string
# [$6] - mode('0' - strip all leading whitespace characters,
#             '1' - strip all trailing whitespace characters,
#             '2' - strip all leading/trailing whitespace characters)
# [$7] - mode('3' - keep $2,
#             '4' - keep $4,
#             '5' - keep $2/$4)
# [$8] - mode('6' - verify the expansion)
#
# Returns:
# (0) substring | incorrect substring ($1/$3 > $2/$4)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
#
# Returns (mode '6'):
# (0) substring
# (1) unspecified expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (4) empty expansion ($2..$4)
# (5) unspecified expansion (! $4)
# (6) incorrect expansion ($3 > $4)
#
ltl_substr1() {
    _str="$5"

    case $#:$7$6 in
        8:*|7:6*|6:6)
            case $1 in
                0)
                    case "$_str" in
                        *"$2"*"$2"*) : ;;
                        "$2"*) return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _str="${_str%"$2"*}"
                ;;
                *)
                    case $1"$_str" in
                        1"$2"*) return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _sfix="$_str"; _i=0; until [ $_i -eq $1 ]; do
                        _sfix="${_sfix#*"$2"}"

                        case "$_sfix" in
                            *"$2"*) : ;;
                            *) [ $((_i + 1)) -eq $1 ] || return 3 ;;
                        esac

                        _i=$((_i + 1))
                    done

                    _str="${_str%"$2$_sfix"}"
                ;;
            esac

            case $3 in
                0)
                    case "$_str" in
                        *"$4"*"$4"*) : ;;
                        *"$4") return 4 ;;
                        *"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    _str="${_str#*"$4"}"
                ;;
                *)
                    case $3"$_str" in
                        1*"$4") return 4 ;;
                        $3*"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    _pfix="$_str"; _i=0; until [ $_i -eq $3 ]; do
                        _pfix="${_pfix%"$4"*}"

                        case "$_pfix" in
                            *"$4"*) : ;;
                            *) [ $((_i + 1)) -eq $3 ] || return 6 ;;
                        esac

                        _i=$((_i + 1))
                    done

                    _str="${_str#"$_pfix$4"}"
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    _str="${_str%"$2"*}"
                ;;
                *)
                    _sfix="$_str"; _i=0; until [ $_i -eq $1 ]; do
                        _sfix="${_sfix#*"$2"}"
                        _i=$((_i + 1))
                    done

                    _str="${_str%"$2$_sfix"}"
                ;;
            esac

            [ "$_str" = "$5" ] && return 2
            _old_str="$_str"

            case $3 in
                0)
                    _str="${_str#*"$4"}"
                ;;
                *)
                    _pfix="$_str"; _i=0; until [ $_i -eq $3 ]; do
                        _pfix="${_pfix%"$4"*}"
                        _i=$((_i + 1))
                    done

                    _str="${_str#"$_pfix$4"}"
                ;;
            esac

            [ "$_str" = "$_old_str" ] && return 2
        ;;
    esac

    [ "$_str" ] || return 1

    case $6 in
        0)
            _str="${_str#${_str%%[![:space:]]*}}"
        ;;
        1)
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
        2)
            _str="${_str#${_str%%[![:space:]]*}}"
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
    esac

    case $7$6 in
        *3*) _str="$_str$2" ;;
        *4*) _str="$4$_str" ;;
        *5*) _str="$4$_str$2" ;;
    esac

    printf "%s" "$_str"
}
```

## ltr_substr0

```sh
# Description:
# Get positional substring, (from LTR) (N)character(s), in a string
#
# Parameters:
# <$1> - from "N" LTR character('0' - max)
# <"$2"> - from "X" character(s)
# <"$3"> - string
# [$4] - mode('0' - strip all leading whitespace characters,
#             '1' - strip all trailing whitespace characters,
#             '2' - strip all leading/trailing whitespace characters)
# [$5] - mode('3' - keep $2)
# [$6] - mode('4' - verify the expansion)
#
# Returns:
# (0) substring | incorrect substring ($1 > $2)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
#
# Returns (mode '4'):
# (0) substring
# (1) empty expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
#
ltr_substr0() {
    _str="$3"

    case $#:$5$4 in
        6:*|5:4*|4:4)
            case $1 in
                0)
                    case "$_str" in
                        *"$2") return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _str="${_str##*"$2"}"
                ;;
                *)
                    case $1"$_str" in
                        $1*"$2"*"$2"*) : ;;
                        1*"$2") return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _i=0; until [ $_i -eq $1 ]; do
                        _str="${_str#*"$2"}"

                        case "$_str" in
                            *"$2"*"$2"*) : ;;
                            *"$2") [ $((_i + 2)) -eq $1 ] && return 1 ;;
                            *"$2"*) : ;;
                            *) [ $((_i + 1)) -eq $1 ] || return 3 ;;
                        esac

                        _i=$((_i + 1))
                    done
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    _str="${_str##*"$2"}"
                ;;
                *)
                    _i=0; until [ $_i -eq $1 ]; do
                        _str="${_str#*"$2"}"
                        _i=$((_i + 1))
                    done
                ;;
            esac

            [ "$_str" = "$3" ] && return 2
        ;;
    esac

    [ "$_str" ] || return 1

    case $4 in
        0)
            _str="${_str#${_str%%[![:space:]]*}}"
        ;;
        1)
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
        2)
            _str="${_str#${_str%%[![:space:]]*}}"
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
    esac

    case $5$4 in
        *3*) _str="$2$_str" ;;
    esac

    printf "%s" "$_str"
}
```

## ltr_substr1

```sh
# Description:
# Get positional substring, (from&to LTR) (N)character(s), in a string
#
# Parameters:
# <$1> - from "N" LTR character('0' - max)
# <"$2"> - from "X" character(s)
# <$3> - to "N" $2-TR character('0' - max)
# <"$4"> - to "X" character(s)
# <"$5"> - string
# [$6] - mode('0' - strip all leading whitespace characters,
#             '1' - strip all trailing whitespace characters,
#             '2' - strip all leading/trailing whitespace characters)
# [$7] - mode('3' - keep $2,
#             '4' - keep $4,
#             '5' - keep $2/$4)
# [$8] - mode('6' - verify the expansion)
#
# Returns:
# (0) substring | incorrect substring ($1/$3 > $2/$4)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
#
# Returns (mode '6'):
# (0) substring
# (1) unspecified expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (4) empty expansion ($2..$4)
# (5) unspecified expansion (! $4)
# (6) incorrect expansion ($3 > $4)
#
ltr_substr1() {
    _str="$5"

    case $#:$7$6 in
        8:*|7:6*|6:6)
            case $1 in
                0)
                    case "$_str" in
                        *"$2") return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _str="${_str##*"$2"}"
                ;;
                *)
                    case $1"$_str" in
                        $1*"$2"*"$2"*) : ;;
                        1*"$2") return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _i=0; until [ $_i -eq $1 ]; do
                        _str="${_str#*"$2"}"

                        case "$_str" in
                            *"$2"*"$2"*) : ;;
                            *"$2") [ $((_i + 2)) -eq $1 ] && return 1 ;;
                            *"$2"*) : ;;
                            *) [ $((_i + 1)) -eq $1 ] || return 3 ;;
                        esac

                        _i=$((_i + 1))
                    done
                ;;
            esac

            case $3 in
                0)
                    case "$_str" in
                        *"$4"*"$4"*) : ;;
                        "$4"*) return 4 ;;
                        *"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    _str="${_str%"$4"*}"
                ;;
                *)
                    case $3"$_str" in
                        1"$4"*) return 4 ;;
                        $3*"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    _sfix="$_str"; _i=0; until [ $_i -eq $3 ]; do
                        _sfix="${_sfix#*"$4"}"

                        case "$_sfix" in
                            *"$4"*) : ;;
                            *) [ $((_i + 1)) -eq $3 ] || return 6 ;;
                        esac

                        _i=$((_i + 1))
                    done

                    _str="${_str%"$4$_sfix"}"
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    _str="${_str##*"$2"}"
                ;;
                *)
                    _i=0; until [ $_i -eq $1 ]; do
                        _str="${_str#*"$2"}"
                        _i=$((_i + 1))
                    done
                ;;
            esac

            [ "$_str" = "$5" ] && return 2
            _old_str="$_str"

            case $3 in
                0)
                    _str="${_str%"$4"*}"
                ;;
                *)
                    _sfix="$_str"; _i=0; until [ $_i -eq $3 ]; do
                        _sfix="${_sfix#*"$4"}"
                        _i=$((_i + 1))
                    done

                    _str="${_str%"$4$_sfix"}"
                ;;
            esac

            [ "$_str" = "$_old_str" ] && return 2
        ;;
    esac

    [ "$_str" ] || return 1

    case $6 in
        0)
            _str="${_str#${_str%%[![:space:]]*}}"
        ;;
        1)
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
        2)
            _str="${_str#${_str%%[![:space:]]*}}"
            _str="${_str%${_str##*[![:space:]]}}"
        ;;
    esac

    case $7$6 in
        *3*) _str="$2$_str" ;;
        *4*) _str="$_str$4" ;;
        *5*) _str="$2$_str$4" ;;
    esac

    printf "%s" "$_str"
}
```

## num_to_char

```sh
# Description:
# Convert a natural number to that amount of a character.
#
# Parameters:
# <$1> - number
# <"$2"> - character
# [$3] - mode('0' - whitespace-separated characters)
#
# Returns:
# (0) character(s)
#
num_to_char() {
    case $# in
        3)
            printf "%s" "$2"

            _i=1; until [ $_i -eq $1 ]; do
                printf " %s" "$2"
                _i=$((_i + 1))
            done
        ;;
        *)
            _i=0; until [ $_i -eq $1 ]; do
                printf "%s" "$2"
                _i=$((_i + 1))
            done
        ;;
    esac
}
```

## parse

```sh
# Description:
# Parse the lines of a file
#
# Parameters:
# <"$1"> - file
# [$2] - mode('1' - add one leading/trailing whitespace character,
#             '2' - add two leading/trailing whitespace characters,
#             '3' - skip empty lines,
#             '4' - strip trailing/leading whitespace characters)
# ["$3"] - mode("5 N" - stop parsing further than "N" line)
#
# Returns:
# (0) output | empty output (file) | empty output (by the given ruleset)
# (1) not a file | file does not exist
# (2) file access error
#
# Caveats:
# 1. NULL character.
# 2. Reading /proc is unreliable.
#
parse() {
    [ -f "$1" ] || return 1
    [ -r "$1" ] || return 2

    case $#:"$3$2" in
        2:5*) _i=0; _maxN="${2#??}" ;;
        3:5*1|3:5*2|3:5*3|3:5*4) _i=0; _maxN="${3#??}" ;;
    esac

    case $#:"$3$2" in
        1:)
            while IFS= read -r _line; do
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        2:1)
            while IFS= read -r _line; do
                printf " %s \n" "$_line"
            done < "$1"
        ;;
        2:2)
            while IFS= read -r _line; do
                printf "  %s  \n" "$_line"
            done < "$1"
        ;;
        2:3)
            while IFS= read -r _line; do
                case ":$_line" in :) : ;; *) printf "%s\n" "$_line" ;; esac
            done < "$1"
        ;;
        2:4)
            while read -r _line; do
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        2:5*)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        3:5*1)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf " %s \n" "$_line"
            done < "$1"
        ;;
        3:5*2)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf "  %s  \n" "$_line"
            done < "$1"
        ;;
        3:5*3)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                case ":$_line" in :) : ;; *) printf "%s\n" "$_line" ;; esac
            done < "$1"
        ;;
        3:5*4)
            while read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf "%s\n" "$_line"
            done < "$1"
        ;;
    esac

    case $#:"$3$2" in
        1:|2:3|2:4|2:5*|3:5*3|3:5*4)
            printf "%s" "$_line"
        ;;
        2:1|3:5*1)
            case ":$_line" in :) : ;; *) printf " %s " "$_line" ;; esac
        ;;
        2:2|3:5*2)
            case ":$_line" in :) : ;; *) printf "  %s  " "$_line" ;; esac
        ;;
    esac
}
```

## parse_fd1

```sh
# Description:
# Parse the content of stdin
#
# Parameters:
# [$1] - mode('1' - add one leading/trailing whitespace character,
#             '2' - add two leading/trailing whitespace characters,
#             '3' - skip empty lines,
#             '4' - strip trailing/leading whitespace characters)
# ["$2"] - mode("5 N" - stop parsing further than "N" line)
#
# Returns:
# (0) output | empty output (stdin) | empty output (by the given ruleset)
#
# Caveats:
# 1. NULL character.
#
parse_fd1() {
    case $#:"$2$1" in
        1:5*) _i=0; _maxN="${1#??}" ;;
        2:5*1|2:5*2|2:5*3|2:5*4) _i=0; _maxN="${2#??}" ;;
    esac

    case $#:"$2$1" in
        0:)
            while IFS= read -r _line; do
                printf "%s\n" "$_line"
            done
        ;;
        1:1)
            while IFS= read -r _line; do
                printf " %s \n" "$_line"
            done
        ;;
        1:2)
            while IFS= read -r _line; do
                printf "  %s  \n" "$_line"
            done
        ;;
        1:3)
            while IFS= read -r _line; do
                case ":$_line" in :) : ;; *) printf "%s\n" "$_line" ;; esac
            done
        ;;
        1:4)
            while read -r _line; do
                printf "%s\n" "$_line"
            done
        ;;
        1:5*)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf "%s\n" "$_line"
            done
        ;;
        2:5*1)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf " %s \n" "$_line"
            done
        ;;
        2:5*2)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf "  %s  \n" "$_line"
            done
        ;;
        2:5*3)
            while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                case ":$_line" in :) : ;; *) printf "%s\n" "$_line" ;; esac
            done
        ;;
        2:5*4)
            while read -r _line; do
                _i=$((_i + 1))
                case $_i in $_maxN) break ;; esac
                printf "%s\n" "$_line"
            done
        ;;
    esac

    case $#:"$2$1" in
        0:|1:3|1:4|1:5*|2:5*3|2:5*4)
            printf "%s" "$_line"
        ;;
        1:1|2:5*1)
            case ":$_line" in :) : ;; *) printf " %s " "$_line" ;; esac
        ;;
        1:2|2:5*2)
            case ":$_line" in :) : ;; *) printf "  %s  " "$_line" ;; esac
        ;;
    esac
}
```

## pline

```sh
# Description:
# Print specific line in file
#
# Parameters:
# <$1> - line number
# <"$2"> - file
#
# Returns:
# (0) line's content | line empty
# (1) line does not exist
# (2) not a file | file does not exist
# (3) file access error
#
# Caveats:
# 1. NULL character.
# 2. Reading /proc is unreliable.
#
pline() {
    [ -f "$2" ] || return 2
    [ -r "$2" ] || return 3

    _i=0; while read -r _line || [ "$_line" ]; do
        _i=$((_i + 1))
        case $_i in $1) printf "%s" "$_line"; return 0 ;; esac
    done < "$2"

    return 1
}
```

## pline_fd1

```sh
# Description:
# Print specific line in stdin
#
# Parameters:
# <$1> - line number
#
# Returns:
# (0) line's content | line empty
# (1) line does not exist
#
# Caveats:
# 1. NULL character.
#
pline_fd1() {
    _i=0

    while read -r _line; do
        _i=$((_i + 1))
        case $_i in $1) printf "%s" "$_line"; return 0 ;; esac
    done

    case $((_i + 1)) in $1) printf "%s" "$_line"; return 0 ;; esac

    return 1
}
```

## remchars

```sh
# Description:
# Remove specific character(s) in string
#
# Parameters:
# <"$1"> - character(s)
# <"$2"> - string
#
# Returns:
# (0) stripped $2 | $2
#
remchars() {
    set -f; _old_ifs=$IFS

    IFS=$1
    set -- $2
    printf "%s" "$@"

    IFS=$_old_ifs; set +f
}
```

## remstr

```sh
# Description:
# Remove a positional substring of a string
#
# Parameters:
# <"$1"> - substring
# <"$2"> - string
# <$3> - mode('0' - remove the first occurrence,
#             '1' - remove the last occurrence,
#             '2' - remove all occurrences)
# [$4] - mode('3' - whitespace is delimiter)
#
# Returns:
# (0) stripped $2
# (1) $1 = $2
# (2) ! $1
#
remstr() {
    _str="$2"

    case "$_str" in
        "$1") return 1 ;;
        *"$1"*) : ;;
        *) return 2 ;;
    esac

    case $# in
        4) case "$_str" in " $1 ") return 1 ;; esac ;;
    esac

    case $3 in
        0|2)
            _pfix="${_str%%"$1"*}"

            case $# in
                4)
                    case "$_str" in
                        *" $1 "*) _pfix="${_pfix% }" ;;
                        *"$1"*" $1") : ;;
                        *" $1") _pfix="${_pfix% }" ;;
                    esac
                ;;
            esac

            _sfix="${_str#*"$1"}"

            case $# in
                4) case "$_str" in "$1 "*) _sfix="${_sfix# }" ;; esac ;;
            esac

            _str="$_pfix$_sfix"
        ;;
        1)
            _pfix="${_str%"$1"*}"

            case $# in
                4) _pfix="${_pfix% }" ;;
            esac

            _sfix="${_str##*"$1"}"

            case $# in
                4)
                    case "$_str" in
                        "$1 "*"$1"*) : ;;
                        "$1 "*) _sfix="${_sfix# }" ;;
                    esac
                ;;
            esac

            _str="$_pfix$_sfix"
        ;;
    esac

    case $3 in
        2)
            while :; do case "$_str" in
                "$1")
                    return 1
                ;;
                *"$1"*)
                    case $# in
                        4) case "$_str" in " $1 ") return 1 ;; esac ;;
                    esac

                    _pfix="${_str%%"$1"*}"

                    case $# in
                        4)
                            case "$_str" in
                                *" $1 "*) _pfix="${_pfix% }" ;;
                                *"$1"*" $1") : ;;
                                *" $1") _pfix="${_pfix% }" ;;
                            esac
                        ;;
                    esac

                    _sfix="${_str#*"$1"}"

                    case $# in
                        4) case "$_str" in "$1 "*) _sfix="${_sfix# }" ;; esac ;;
                    esac

                    _str="$_pfix$_sfix"
                ;;
                *)
                    break
                ;;
            esac done
        ;;
    esac

    [ "$_str" ] || return 1

    printf "%s" "$_str"
}
```

## replstr

```sh
# Description:
# Replace a substring of a string with character(s)
#
# Parameters:
# <"$1"> - substring
# <"$2"> - string
# <"$3"> - character(s)
# <$4> - mode('0' - replace the first occurrence,
#             '1' - replace the last occurrence,
#             '2' - replace all occurrences)
#
# Returns:
# (0) modified $2
# (1) $1 = $2
# (2) ! $1
#
replstr() {
    _str="$2"

    case "$_str" in
        "$1") return 1 ;;
        *"$1"*) : ;;
        *) return 2 ;;
    esac

    case "$1" in
        "$3") printf "%s" "$_str" && return 0 ;;
    esac

    case $4 in
        0)
            _str="${_str%%"$1"*}$3${_str#*"$1"}"
        ;;
        1)
            _str="${_str%"$1"*}$3${_str##*"$1"}"
        ;;
        2)
            _str_ref="${_str%%"$1"*}$3"
            _str="$_str_ref${_str#*"$1"}"

            while :; do case "$_str" in
                "$_str_ref"*"$1"*)
                    _pfix="${_str#*"$_str_ref"}" && _pfix="${_pfix%%"$1"*}"
                    _str="$_str_ref$_pfix$3${_str#*"$_str_ref$_pfix$1"}"
                    _str_ref="$_str_ref$_pfix$3"
                ;;
                *)
                    break
                ;;
            esac done
        ;;
    esac

    printf "%s" "$_str"
}
```

## rstrip

```sh
# Description:
# Strip character(s) from right of string
#
# Parameters:
# <"$1"> - character(s)
# <"$2"> - string
#
# Returns:
# (0) stripped $2
# (1) ! $1
# (2) $1 = $2
#
rstrip() {
    case "$2" in
        "$1") return 2 ;;
        *"$1") printf "%s" "${2%"$1"}"; return 0 ;;
    esac

    return 1
}
```

## esc_str

```sh
# Description:
# Escape all POSIX-defined shell meta characters in a string; characters are:
# \ | & ; < > ( ) $ ` " ' * ? [ ] # ~ = %
#
# Parameters:
# <"$1"> - string
# ["$2"] - mode("0 X" - escape only "X" whitespace-separated character(s))
# [$3] - mode('1' - strip the characters,
#             '2' - escape single quote character with itself)
#
# Returns:
# (0) escaped/stripped $1
# (1) no meta characters in $1
#
esc_str() {
    _str="$1"
    unset _str_ref

    set -f

    case $2 in
        0*) _chars="${2#??}" ;;
        *) _chars='\ | & ; < > ( ) $ ` " '\'' * ? [ ] # ~ = %' ;;
    esac

    case "$_chars" in
        '\'*) : ;;
        *'\'*) _chars="\\ ${_chars%%\\*}${_chars#*\\ }" ;;
    esac

    for _char in $_chars; do
        case "$_str" in
            *"$_char"*) : ;;
            *) continue ;;
        esac

        case $_char:$#:$3:$2 in
            "'":3:2*|"'":2::2)
                _str_ref="${_str%%\'*}'\\''"
            ;;
            $_char:3:1*|$_char:2::1)
                _str_ref="${_str%%"$_char"*}"
            ;;
            *)
                _str_ref="${_str%%"$_char"*}\\$_char"
            ;;
        esac
        _str="$_str_ref${_str#*"$_char"}"

        case $_char:$#:$3:$2 in
            "'":3:2*|"'":2::2)
                while :; do case "$_str" in
                    "$_str_ref"*"'"*)
                        _str="${_str#*"$_str_ref"}"
                        _str_ref="$_str_ref${_str%%\'*}'\\''"
                        _str="$_str_ref${_str#*\'}"
                    ;;
                    *)
                        break
                    ;;
                esac done
            ;;
            $_char:3:1*|$_char:2::1)
                while :; do case "$_str" in
                    *"$_char"*) _str="${_str%%"$_char"*}${_str#*"$_char"}" ;;
                    *) break ;;
                esac done
            ;;
            *)
                while :; do case "$_str" in
                    "$_str_ref"*"$_char"*)
                        _str="${_str#*"$_str_ref"}"
                        _str_ref="$_str_ref${_str%%"$_char"*}\\$_char"
                        _str="$_str_ref${_str#*"$_char"}"
                    ;;
                    *)
                        break
                    ;;
                esac done
            ;;
        esac
    done

    set +f

    [ "$_str_ref" ] || return 1

    printf "%s" "$_str"
}
```

## str_to_chars

```sh
# Description:
# Convert a string to whitespace-separated characters
#
# Parameters:
# <"$1"> - string
# [$2] - mode('0' - remove duplicate characters)
#
# Returns:
# (0) characters | ! $1
#
# Caveats:
# 1. Only characters in the printable set of C ASCII characters ('[\x20-\x7E]')
#    will be printed.
#
str_to_chars() {
    _str="$1"

    _old_lc_ctype=$LC_CTYPE
    LC_CTYPE=C

    while :; do
        case ":$_str" in :) break ;; esac

        _char="${_str%"${_str#?}"}"
        _str="${_str#?}"

        case "$_char" in
            " ")
                :
            ;;
            [[:print:]])
                case $# in
                    2) _chars_set="$_char" ;;
                esac

                printf "%s" "$_char"; break
            ;;
        esac
    done

    case $# in
        2)
            while :; do
                case ":$_str" in :) break ;; esac

                _char="${_str%"${_str#?}"}"
                _str="${_str#?}"

                case "$_chars_set" in
                    *"$_char"*) continue ;;
                esac

                case "$_char" in
                    " ")
                        :
                    ;;
                    [[:print:]])
                        _chars_set="$_chars_set$_char"

                        printf " %s" "$_char"
                    ;;
                esac
            done
        ;;
        *)
            while :; do
                case ":$_str" in :) break ;; esac

                _char="${_str%"${_str#?}"}"
                _str="${_str#?}"

                case "$_char" in
                    " ")
                        :
                    ;;
                    [[:print:]])
                        printf " %s" "$_char"
                    ;;
                esac
            done
        ;;
    esac

    LC_CTYPE=$_old_lc_ctype
}
```

## warn

```sh
# Description:
# Print a warning
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) text
#
warn() {
    printf "WARNING: %s\n" "$*"
}
```

## warn_clr

```sh
# Description:
# Print a colorful warning
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) text
#
warn_clr() {
    printf "%bWARNING:%b %s\n" "\033[1;33m" "\033[0m" "$*"
}
```

### warn_fmt

```sh
# Description:
# Print a warning with printf format before text
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) text
#
warn_fmt() {
    _printf_fmt="$1"; shift
    printf "WARNING: ${_printf_fmt}%s\n" "$*"
}
```

### warn_fmt_clr

```sh
# Description:
# Print a colorful warning with printf format before text
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) text
#
warn_fmt_clr() {
    _printf_fmt="$1"; shift
    printf "%bWARNING:%b ${_printf_fmt}%s\n" "\033[1;33m" "\033[0m" "$*"
}
```
