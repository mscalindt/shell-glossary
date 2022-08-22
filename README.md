A collection of reusable pure POSIX `sh` functions with no external binary calls (on somewhat modern shells where `read`, `printf`, and `echo` are builtins).

# Normal functions:

* [CHARS_EVEN()](https://github.com/mscalindt/shell-glossary#chars_even)
* [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary#confirm_cont)
* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [ERR_NE()](https://github.com/mscalindt/shell-glossary#err_ne)
* [ERR_FMT()](https://github.com/mscalindt/shell-glossary#err_fmt)
* [ERR_NE_FMT()](https://github.com/mscalindt/shell-glossary#err_ne_fmt)
* [ESC_STR()](https://github.com/mscalindt/shell-glossary#esc_str) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/11
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/12
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/6
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
* [SQ_ARG()](https://github.com/mscalindt/shell-glossary#sq_arg)
* [STR()](https://github.com/mscalindt/shell-glossary#str) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/7.1
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

* [PARSE_FD1()](https://github.com/mscalindt/shell-glossary#parse_fd1)
* [PLINE_FD1()](https://github.com/mscalindt/shell-glossary#pline_fd1)
* [STR_FD1()](https://github.com/mscalindt/shell-glossary#str_fd1)

## chars_even

```
# Description:
# Test if the character(s) count (combined), in a given string, is even
#
# Parameters:
# <"$1"> - character(s)
# <"$2"> - string
#
# Provides:
# <"$_count"> - the count
#
# Returns:
# (0) even
# (1) uneven
#
chars_even() {
    set -f; _old_ifs="$IFS"

    IFS="$1"

    _chars="$1"; while :; do case ${#_chars} in
        1)
            _char="$_chars"

            case "$2" in
                *"$_char") set -- $2; _count=$#; break ;;
                *) set -- $2; _count=$(($# - 1)); break ;;
            esac
        ;;
        *)
            while :; do
                _char="${_chars%"${_chars#?}"}"
                _chars="${_chars#?}"

                case "$2" in
                    *"$_char") set -- $2; _count=$#; break 2 ;;
                esac

                case ${#_chars} in 1) break ;; esac
            done
        ;;
    esac done

    IFS="$_old_ifs"; set +f

    [ "$((_count % 2))" -eq 0 ] || return 1
}
```

## confirm_cont

```sh
# Description:
# Ask for confirmation to continue
#
# Parameters:
# <$1> - mode1-2('1' - default action: Y,
#                '2' - default action: N)
#
# Provides:
# <"$_action"> - the raw input
#
# Returns:
# (0) permitted
# (1) forbidden
#
confirm_cont() {
    case $1 in
        1)
            printf 'Continue? [Y/n] '
            read -r _action

            case "$_action" in
                N*|n*) return 1 ;;
            esac

            return 0
        ;;
        2)
            printf 'Continue? [y/N] '
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
# <$1> - mode1-2('1' - default action: Y,
#                '2' - default action: N)
#
# Provides:
# <"$_action"> - the raw input
#
# Returns:
# (0) permitted
# (1) forbidden
#
confirm_cont_clr() {
    case $1 in
        1)
            printf "%bContinue? [Y/n]%b " "\033[1;37m" "\033[0m"
            read -r _action

            case "$_action" in
                N*|n*) return 1 ;;
            esac

            return 0
        ;;
        2)
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
    _rc="$1"; shift
    printf "ERROR: %s\n" "$*" 1>&2
    exit "$_rc"
}
```

## err_clr

```sh
# Description:
# Colorfully print error and exit
#
# Parameters:
# <$1> - exit code
# <"$2"+> - text
#
err_clr() {
    _rc="$1"; shift
    printf "%bERROR:%b %s\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit "$_rc"
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
# (0) error-formatted $1
#
err_ne() {
    printf "ERROR: %s\n" "$*" 1>&2
}
```

## err_ne_clr

```sh
# Description:
# Colorfully print error, no exit
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) error-formatted $1
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
    _rc="$1"; _printf_fmt="$2"; shift 2
    printf "ERROR: ${_printf_fmt}%s\n" "$*" 1>&2
    exit "$_rc"
}
```

## err_fmt_clr

```sh
# Description:
# Colorfully print error with printf format before text and exit
#
# Parameters:
# <$1> - exit code
# <"$2"> - printf format
# <"$3"+> - text
#
err_fmt_clr() {
    _rc="$1"; _printf_fmt="$2"; shift 2
    printf "%bERROR:%b ${_printf_fmt}%s\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit "$_rc"
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
# (0) error-formatted $2
#
err_ne_fmt() {
    _printf_fmt="$1"; shift
    printf "ERROR: ${_printf_fmt}%s\n" "$*" 1>&2
}
```

## err_ne_fmt_clr

```sh
# Description:
# Colorfully print error with printf format before text, no exit
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) error-formatted $2
#
err_ne_fmt_clr() {
    _printf_fmt="$1"; shift
    printf "%bERROR:%b ${_printf_fmt}%s\n" "\033[1;31m" "\033[0m" "$*" 1>&2
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
# ["$2"] - mode0("0 X" - escape only "X" whitespace-separated character(s))
# [$3] - mode1-2('1' - strip the characters,
#                '2' - escape single quote character with itself)
#
# Provides:
# (0) <"$_str"> - the modified string
# <"$_chars"> - the characters;
#               whitespace delimited;
#               sorted by order parsed
#
# Returns:
# (0) escaped/stripped $1
# (1) no meta characters in $1
#
esc_str() {
    _str="$1"
    unset _str_ref

    set -f

    case "$2" in
        0*) _chars="${2#??}" ;;
        *) _chars='\ | & ; < > ( ) $ ` " '\'' * ? [ ] # ~ = %' ;;
    esac

    case "$_chars" in
        '\'*) : ;;
        *'\') _chars='\'" ${_chars%%\\*}${_chars#*\\}" ;;
        *'\'*) _chars='\'" ${_chars%%\\*}${_chars#*\\ }" ;;
    esac

    for _char in $_chars; do
        case "$_str" in
            *"$_char"*) : ;;
            *) continue ;;
        esac

        case $_char:$#:$3:"$2" in
            "'":3:2*|"'":2::2)
                _str_ref="${_str%%\'*}'\\''"
            ;;
            "$_char":3:1*|"$_char":2::1)
                _str_ref="${_str%%"$_char"*}"
            ;;
            *)
                _str_ref="${_str%%"$_char"*}\\$_char"
            ;;
        esac
        _str="$_str_ref${_str#*"$_char"}"

        case $_char:$#:$3:"$2" in
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
            "$_char":3:1*|"$_char":2::1)
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

## fcount

```sh
# Description:
# Count files and directories in a directory
#
# Parameters:
# <"$1"> - directory
# ["$2"] - mode0-2("0X" - count files and directories ending with "X",
#                  '1' - count only files,
#                  "1X" - count only files ending with "X",
#                  '2' - count only directories,
#                  "2X" - count only directories ending with "X")
# [$3] - mode3('3' - exclude hidden files/directories)
#
# Provides:
# (0) <$_count> - the count
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

    case "$1" in
        *'/') _dir="${1%?}" ;;
        *) _dir="$1" ;;
    esac
    _count=0

    case "$2" in
        0*|1*|2*) _sfix="${2#?}" ;;
    esac

    case $#:"$2" in
        1:)
            set -- "$_dir"/*
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/.*
            [ "$#" -ge 3 ] && _count=$((_count + $# - 2))
        ;;
        2:0*)
            set -- "$_dir"/*"$_sfix"
            [ -e "$1" ] && _count=$#

            set -- "$_dir"/.*"$_sfix"
            case "$_sfix" in
                '.')
                    [ -e "$2" ] && _count=$((_count + $# - 1))
                ;;
                '..')
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
                '.') [ -e "$2" ] && _count=$((_count + $# - 1)) ;;
                *) [ -e "$1" ] && _count=$((_count + $#)) ;;
            esac

            set -- "$_dir"/.*"$_sfix"/
            case "$_sfix" in
                '.') [ -e "$2" ] && _count=$((_count - $# + 1)) ;;
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
                '.')
                    [ -e "$2" ] && _count=$((_count + $# - 1))
                ;;
                '..')
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
# Provides:
# <"$_str"> - the absolute path | path
#
# Returns:
# (0) absolute $1 | $1
#
get_fpath() {
    case "$1" in
        '/'*) _str="$1"; printf "%s" "$1" ;;
        *) _str="$PWD/$1"; printf "%s" "$PWD/$1" ;;
    esac
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
# (0) info-formatted $1
#
info() {
    printf "INFO: %s\n" "$*"
}
```

## info_clr

```sh
# Description:
# Colorfully print info
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) info-formatted $1
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
# (0) info-formatted $2
#
info_fmt() {
    _printf_fmt="$1"; shift
    printf "INFO: ${_printf_fmt}%s\n" "$*"
}
```

## info_fmt_clr

```sh
# Description:
# Colorfully print info with printf format before text
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) info-formatted $2
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
# [$4] - mode0-2('0' - strip all leading whitespace characters,
#                '1' - strip all trailing whitespace characters,
#                '2' - strip all leading/trailing whitespace characters)
# [$5] - mode3('3' - keep $2)
# [$6] - mode4('4' - verify the expansion)
#
# Provides:
# (0) <"$_str"> - the modified string
# [$_i] - the iterations completed;
#         whole number
#
# Returns:
# (0) substring | incorrect substring ($1 > $2)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#
# Returns (mode '4'):
# (0) substring
# (1) empty expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (255) bad input
#
ltl_substr0() {
    case :$1${1#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    _str="$3"

    case $#:$5:$4 in
        6*|5:4*|4::4)
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
                    case $1:"$_str" in
                        1:"$2"*) return 1 ;;
                        "$1":*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _sfix="$_str"; _i=0; until [ "$_i" -eq "$1" ]; do
                        _sfix="${_sfix#*"$2"}"

                        case "$_sfix" in
                            *"$2"*) : ;;
                            *) [ "$((_i + 1))" -eq "$1" ] || return 3 ;;
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
                    _sfix="$_str"; _i=0; until [ "$_i" -eq "$1" ]; do
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
            _str="${_str#"${_str%%[![:space:]]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[![:space:]]*}"}"
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
    esac

    case $5:$4 in
        3*|*3) _str="$_str$2" ;;
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
# [$6] - mode0-2('0' - strip all leading whitespace characters,
#                '1' - strip all trailing whitespace characters,
#                '2' - strip all leading/trailing whitespace characters)
# [$7] - mode3-5('3' - keep $2,
#                '4' - keep $4,
#                '5' - keep $2/$4)
# [$8] - mode6('6' - verify the expansion)
#
# Provides:
# (0) <"$_str"> - the modified string
# [$_i] - the iterations completed;
#         whole number
#
# Returns:
# (0) substring | incorrect substring ($1/$3 > $2/$4)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#
# Returns (mode '6'):
# (0) substring
# (1) unspecified expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (4) empty expansion ($2..$4)
# (5) unspecified expansion (! $4)
# (6) incorrect expansion ($3 > $4)
# (255) bad input
#
ltl_substr1() {
    case :$1${1#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    case :$3${3#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$3$3") : ;;
        *) return 255 ;;
    esac

    _str="$5"

    case $#:$7:$6 in
        8*|7:6*|6::6)
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
                    case $1:"$_str" in
                        1:"$2"*) return 1 ;;
                        "$1":*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _sfix="$_str"; _i=0; until [ "$_i" -eq "$1" ]; do
                        _sfix="${_sfix#*"$2"}"

                        case "$_sfix" in
                            *"$2"*) : ;;
                            *) [ "$((_i + 1))" -eq "$1" ] || return 3 ;;
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
                    case $3:"$_str" in
                        1:*"$4") return 4 ;;
                        "$3":*"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    _pfix="$_str"; _i=0; until [ "$_i" -eq "$3" ]; do
                        _pfix="${_pfix%"$4"*}"

                        case "$_pfix" in
                            *"$4"*) : ;;
                            *) [ "$((_i + 1))" -eq "$3" ] || return 6 ;;
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
                    _sfix="$_str"; _i=0; until [ "$_i" -eq "$1" ]; do
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
                    _pfix="$_str"; _i=0; until [ "$_i" -eq "$3" ]; do
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
            _str="${_str#"${_str%%[![:space:]]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[![:space:]]*}"}"
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
    esac

    case $7:$6 in
        3*|*3) _str="$_str$2" ;;
        4*|*4) _str="$4$_str" ;;
        5*|*5) _str="$4$_str$2" ;;
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
# [$4] - mode0-2('0' - strip all leading whitespace characters,
#                '1' - strip all trailing whitespace characters,
#                '2' - strip all leading/trailing whitespace characters)
# [$5] - mode3('3' - keep $2)
# [$6] - mode4('4' - verify the expansion)
#
# Provides:
# (0) <"$_str"> - the modified string
# [$_i] - the iterations completed;
#         whole number
#
# Returns:
# (0) substring | incorrect substring ($1 > $2)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#
# Returns (mode '4'):
# (0) substring
# (1) empty expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (255) bad input
#
ltr_substr0() {
    case :$1${1#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    _str="$3"

    case $#:$5:$4 in
        6*|5:4*|4::4)
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
                    case $1:"$_str" in
                        "$1":*"$2"*"$2"*) : ;;
                        1:*"$2") return 1 ;;
                        "$1":*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _i=0; until [ "$_i" -eq "$1" ]; do
                        _str="${_str#*"$2"}"

                        case "$_str" in
                            *"$2"*"$2"*) : ;;
                            *"$2") [ "$((_i + 2))" -eq "$1" ] && return 1 ;;
                            *"$2"*) : ;;
                            *) [ "$((_i + 1))" -eq "$1" ] || return 3 ;;
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
                    _i=0; until [ "$_i" -eq "$1" ]; do
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
            _str="${_str#"${_str%%[![:space:]]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[![:space:]]*}"}"
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
    esac

    case $5:$4 in
        3*|*3) _str="$2$_str" ;;
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
# [$6] - mode0-2('0' - strip all leading whitespace characters,
#                '1' - strip all trailing whitespace characters,
#                '2' - strip all leading/trailing whitespace characters)
# [$7] - mode3-5('3' - keep $2,
#                '4' - keep $4,
#                '5' - keep $2/$4)
# [$8] - mode6('6' - verify the expansion)
#
# Provides:
# (0) <"$_str"> - the modified string
# [$_i] - the iterations completed;
#         whole number
#
# Returns:
# (0) substring | incorrect substring ($1/$3 > $2/$4)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#
# Returns (mode '6'):
# (0) substring
# (1) unspecified expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (4) empty expansion ($2..$4)
# (5) unspecified expansion (! $4)
# (6) incorrect expansion ($3 > $4)
# (255) bad input
#
ltr_substr1() {
    case :$1${1#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    case :$3${3#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$3$3") : ;;
        *) return 255 ;;
    esac

    _str="$5"

    case $#:$7:$6 in
        8*|7:6*|6::6)
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
                    case $1:"$_str" in
                        "$1":*"$2"*"$2"*) : ;;
                        1:*"$2") return 1 ;;
                        "$1":*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    _i=0; until [ "$_i" -eq "$1" ]; do
                        _str="${_str#*"$2"}"

                        case "$_str" in
                            *"$2"*"$2"*) : ;;
                            *"$2") [ "$((_i + 2))" -eq "$1" ] && return 1 ;;
                            *"$2"*) : ;;
                            *) [ "$((_i + 1))" -eq "$1" ] || return 3 ;;
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
                    case $3:"$_str" in
                        1:"$4"*) return 4 ;;
                        "$3":*"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    _sfix="$_str"; _i=0; until [ "$_i" -eq "$3" ]; do
                        _sfix="${_sfix#*"$4"}"

                        case "$_sfix" in
                            *"$4"*) : ;;
                            *) [ "$((_i + 1))" -eq "$3" ] || return 6 ;;
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
                    _i=0; until [ "$_i" -eq "$1" ]; do
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
                    _sfix="$_str"; _i=0; until [ "$_i" -eq "$3" ]; do
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
            _str="${_str#"${_str%%[![:space:]]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[![:space:]]*}"}"
            _str="${_str%"${_str##*[![:space:]]}"}"
        ;;
    esac

    case $7:$6 in
        3*|*3) _str="$2$_str" ;;
        4*|*4) _str="$_str$4" ;;
        5*|*5) _str="$2$_str$4" ;;
    esac

    printf "%s" "$_str"
}
```

## num_to_char

```sh
# Description:
# Convert N to that amount of a character
#
# Parameters:
# <$1> - N
# <"$2"> - character
# [$3] - mode0('0' - whitespace-separated characters)
#
# Returns:
# (0) character(s)
# (255) bad input
#
num_to_char() {
    case $1:${1#*[!0123456789]} in
        : | 0*) return 255 ;;
        "$1:$1") : ;;
        *) return 255 ;;
    esac

    case $# in
        3)
            printf "%s" "$2"

            _i=1; until [ "$_i" -eq "$1" ]; do
                printf " %s" "$2"
                _i=$((_i + 1))
            done
        ;;
        *)
            _i=0; until [ "$_i" -eq "$1" ]; do
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
# Parse the content of a file
#
# Parameters:
# <"$1"> - file
# [$2] - mode1-4('1' - add one leading/trailing whitespace character,
#                '2' - add two leading/trailing whitespace characters,
#                '3' - skip empty lines,
#                '4' - strip trailing/leading whitespace characters)
# ["$3"] - mode5("5 N" - stop parsing further than "N" line)
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

    case $#:$3:$2 in
        1*)
            while IFS= read -r _line; do
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        2::1)
            while IFS= read -r _line; do
                printf " %s \n" "$_line"
            done < "$1"
        ;;
        2::2)
            while IFS= read -r _line; do
                printf "  %s  \n" "$_line"
            done < "$1"
        ;;
        2::3)
            while IFS= read -r _line; do
                case ":$_line" in :) : ;; *) printf "%s\n" "$_line" ;; esac
            done < "$1"
        ;;
        2::4)
            while read -r _line; do
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        2::5*)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        3:5*:1)
            _maxN="${3#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf " %s \n" "$_line"
            done < "$1"
        ;;
        3:5*:2)
            _maxN="${3#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "  %s  \n" "$_line"
            done < "$1"
        ;;
        3:5*:3)
            _maxN="${3#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                case ":$_line" in :) : ;; *) printf "%s\n" "$_line" ;; esac
            done < "$1"
        ;;
        3:5*:4)
            _maxN="${3#??}"; _i=0; while read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done < "$1"
        ;;
    esac

    case $#:$3:$2 in
        1*|2::3|2::4|2::5*|3:5*:3|3:5*:4)
            printf "%s" "$_line"
        ;;
        2::1|3:5*:1)
            [ "$_line" ] && printf " %s " "$_line"
        ;;
        2::2|3:5*:2)
            [ "$_line" ] && printf "  %s  " "$_line"
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
# [$1] - mode1-4('1' - add one leading/trailing whitespace character,
#                '2' - add two leading/trailing whitespace characters,
#                '3' - skip empty lines,
#                '4' - strip trailing/leading whitespace characters)
# ["$2"] - mode5("5 N" - stop parsing further than "N" line)
#
# Returns:
# (0) output | empty output (stdin) | empty output (by the given ruleset)
#
# Caveats:
# 1. NULL character.
#
parse_fd1() {
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
            _maxN="${1#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done
        ;;
        2:5*1)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf " %s \n" "$_line"
            done
        ;;
        2:5*2)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "  %s  \n" "$_line"
            done
        ;;
        2:5*3)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                case ":$_line" in :) : ;; *) printf "%s\n" "$_line" ;; esac
            done
        ;;
        2:5*4)
            _maxN="${2#??}"; _i=0; while read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done
        ;;
    esac

    case $#:"$2$1" in
        0:|1:3|1:4|1:5*|2:5*3|2:5*4)
            printf "%s" "$_line"
        ;;
        1:1|2:5*1)
            [ "$_line" ] && printf " %s " "$_line"
        ;;
        1:2|2:5*2)
            [ "$_line" ] && printf "  %s  " "$_line"
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
# Provides:
# (0) <"$_line"> - the line
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
        case $_i in "$1") printf "%s" "$_line"; return 0 ;; esac
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
# Provides:
# (0) <"$_line"> - the line
#
# Returns:
# (0) line's content | line empty
# (1) line does not exist
#
# Caveats:
# 1. NULL character.
#
pline_fd1() {
    _i=0; while read -r _line || [ "$_line" ]; do
        _i=$((_i + 1))
        case $_i in "$1") printf "%s" "$_line"; return 0 ;; esac
    done

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
    set -f; _old_ifs="$IFS"

    IFS="$1"
    set -- $2
    printf "%s" "$@"

    IFS="$_old_ifs"; set +f
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
# <$3> - mode1-3('1' - remove the first occurrence,
#                '2' - remove the last occurrence,
#                '3' - remove all occurrences)
# [$4] - mode4('4' - whitespace is delimiter)
#
# Provides:
# (0) <"$_str"> - the modified string
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
        1|3)
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
                4)
                    case "$_str" in
                        "$1 "*) _sfix="${_sfix# }" ;;
                    esac
                ;;
            esac

            _str="$_pfix$_sfix"
        ;;
        2)
            _pfix="${_str%"$1"*}"
            case $# in
                4)
                    _pfix="${_pfix% }"
                ;;
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
        3)
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
                        4)
                            case "$_str" in
                                "$1 "*) _sfix="${_sfix# }" ;;
                            esac
                        ;;
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
# <$4> - mode1-3('1' - replace the first occurrence,
#                '2' - replace the last occurrence,
#                '3' - replace all occurrences)
#
# Provides:
# (0) <"$_str"> - the modified string
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
        "$3") printf "%s" "$_str"; return 0 ;;
    esac

    case $4 in
        1)
            _str="${_str%%"$1"*}$3${_str#*"$1"}"
        ;;
        2)
            _str="${_str%"$1"*}$3${_str##*"$1"}"
        ;;
        3)
            _str_ref="${_str%%"$1"*}$3"
            _str="$_str_ref${_str#*"$1"}"

            while :; do case "$_str" in
                "$_str_ref"*"$1"*)
                    _str="${_str#*"$_str_ref"}"
                    _str_ref="$_str_ref${_str%%"$1"*}$3"
                    _str="$_str_ref${_str#*"$1"}"
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

## sq_arg

```sh
# Description:
# Get N single-quoted argument in a shell-folded string
#
# Parameters:
# <$1> - N
# <"$2"> - string
#
# Provides:
# (0) <"$_arg"> - the argument
# (0) <"$_pfix"> - a pattern;
#                  left side of the argument
# (0) <"$_sfix"> - a pattern;
#                  right side of the argument
# [$_i] - the iterations completed;
#         whole number
#
# Returns:
# (0) argument
# (1) argument does not exist
# (2) invalid $2
# (255) bad input
#
sq_arg() {
    sq_even() {
        set -f; _old_ifs="$IFS"

        IFS=\'
        set -- $1

        IFS="$_old_ifs"; set +f

        [ "$(($# % 2))" -eq 0 ] && return 0 || return 1
    }

    case $1:${1#*[!0123456789]} in
        : | 0*) return 255 ;;
        "$1:$1") : ;;
        *) return 255 ;;
    esac

    sq_even "$2" || return 2

    _sfix="$2"; _i=0; while :; do
        _i=$((_i + 1))

        _sfix="${_sfix#*\'*\'}"

        while :; do case ":$_sfix" in
            :'\'*) _sfix="${_sfix#???*\'}" ;;
            :' '*) break ;;
            :) [ "$1" -eq "$_i" ] && break || return 1 ;;
            *) return 2 ;;
        esac done

        if [ "$1" -eq "$_i" ]; then
            _pfix="$2"
            _pfix="${_pfix%"$_sfix"}"
            _pfix="${_pfix%\'*\'*}"

            while :; do case ":${_pfix#"${_pfix%??}"}" in
                :"\\'") _pfix="${_pfix%\'*???}" ;;
                :*' ' | :) break ;;
                *) return 2 ;;
            esac done

            _arg="$2"
            _arg="${_arg#"$_pfix"}"
            _arg="${_arg%"$_sfix"}"

            printf "%s" "$_arg"
            return 0
        fi
    done
}
```

## str

```sh
# Description:
# Check the existence/position of a substring in string
#
# Parameters:
# <"$1"> - substring
# <"$2"> - string
# [$3] - mode1-3('1' - $1 is first character(s) of $2,
#                '2' - $1 is last character(s) of $2,
#                '3' - $1 is, on its own, $2)
#
# Returns:
# (0) match
# (1) no match
#
str() {
    case $#:$3 in
        2:) case "$2" in *"$1"*) return 0 ;; esac ;;
        3:1) case "$2" in "$1"*) return 0 ;; esac ;;
        3:2) case "$2" in *"$1") return 0 ;; esac ;;
        3:3) case "$2" in "$1") return 0 ;; esac ;;
    esac

    return 1
}
```

## str_fd1

```sh
# Description:
# Check the existence/position of a substring in stdin
#
# Parameters:
# <"$1"> - substring
# [$2] - mode1-3('1' - $1 is first character(s) of stdin,
#                '2' - $1 is last character(s) of stdin,
#                '3' - $1 is, on its own, stdin)
#
# Returns:
# (0) match
# (1) no match
#
# Caveats:
# 1. NULL character.
#
str_fd1() {
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

## str_to_chars

```sh
# Description:
# Convert a string to whitespace-separated characters
#
# Parameters:
# <"$1"> - string
# [$2] - mode0('0' - remove duplicate characters)
#
# Returns:
# (0) character(s)
#
# Caveats:
# 1. Only characters in the printable set of C ASCII characters ('[\x20-\x7E]')
#    will be printed.
#
str_to_chars() {
    _str="$1"

    _old_lc="$LC_ALL"; export LC_ALL=C

    while :; do
        case ":$_str" in :) break ;; esac

        _char="${_str%"${_str#?}"}"
        _str="${_str#?}"

        case $_char in
            ' ')
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

                case $_char in
                    ' ')
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

                case $_char in
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

    export LC_ALL="$_old_lc"
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
# (0) warning-formatted $1
#
warn() {
    printf "WARNING: %s\n" "$*"
}
```

## warn_clr

```sh
# Description:
# Colorfully print a warning
#
# Parameters:
# <"$1"+> - text
#
# Returns:
# (0) warning-formatted $1
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
# (0) warning-formatted $2
#
warn_fmt() {
    _printf_fmt="$1"; shift
    printf "WARNING: ${_printf_fmt}%s\n" "$*"
}
```

### warn_fmt_clr

```sh
# Description:
# Colorfully print a warning with printf format before text
#
# Parameters:
# <"$1"> - printf format
# <"$2"+> - text
#
# Returns:
# (0) warning-formatted $2
#
warn_fmt_clr() {
    _printf_fmt="$1"; shift
    printf "%bWARNING:%b ${_printf_fmt}%s\n" "\033[1;33m" "\033[0m" "$*"
}
```
