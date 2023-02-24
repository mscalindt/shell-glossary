A collection of reusable pure POSIX `sh` functions with optional external
utility calls.

# Normal functions:

* [CHARS_EVEN()](https://github.com/mscalindt/shell-glossary#chars_even)
* [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary#confirm_cont)
* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [ERRF()](https://github.com/mscalindt/shell-glossary#errF)
* [ESC_STR()](https://github.com/mscalindt/shell-glossary#esc_str)
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount)
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath)
* [INFO()](https://github.com/mscalindt/shell-glossary#info)
* [LSTRIP()](https://github.com/mscalindt/shell-glossary#lstrip)
* [LTL_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltl_substr0)
* [LTL_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltl_substr1)
* [LTR_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltr_substr0)
* [LTR_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltr_substr1)
* [NUM_TO_CHAR()](https://github.com/mscalindt/shell-glossary#num_to_char)
* [PARSE()](https://github.com/mscalindt/shell-glossary#parse)
* [PLINE()](https://github.com/mscalindt/shell-glossary#pline)
* [REMCHARS()](https://github.com/mscalindt/shell-glossary#remchars)
* [REMSTR()](https://github.com/mscalindt/shell-glossary#remstr)
* [REPLCHARS()](https://github.com/mscalindt/shell-glossary#replchars)
* [REPLSTR()](https://github.com/mscalindt/shell-glossary#replstr)
* [RSTRIP()](https://github.com/mscalindt/shell-glossary#rstrip)
* [SQ_ARG()](https://github.com/mscalindt/shell-glossary#sq_arg)
* [STR()](https://github.com/mscalindt/shell-glossary#str)
* [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary#str_to_chars)

# Stdin functions:

* [PARSE_FD1()](https://github.com/mscalindt/shell-glossary#parse_fd1)
* [PLINE_FD1()](https://github.com/mscalindt/shell-glossary#pline_fd1)
* [STR_FD1()](https://github.com/mscalindt/shell-glossary#str_fd1)

## chars_even

```sh
#! .desc:
# Test if the character(s) count (combined), in a given string, is even
#! .params:
# <"$1"> - character(s)
# <"$2"> - string
#! .gives:
# (0|1) <$_count> - the count
#! .rc:
# (0) even
# (1) uneven
# (2) ! $1
#.
chars_even() {
    char_even() {
        _old_ifs="$IFS"; IFS="$1"

        set -f; case "$2" in
            *"$IFS") set -- $2; _count=$# ;;
            *) set -- $2; _count=$(($# - 1)) ;;
        esac; set +f

        IFS="$_old_ifs"

        [ "$((_count % 2))" -eq 0 ]
    }

    case ${#1} in
        1)
            char_even "$1" "$2"
        ;;
        *)
            char_even "$1" "$2"

            case "$1" in
                *"${2#"${2%?}"}"*)
                    case "$2" in
                        *"$1") : ;;
                        *) _count=$((_count + 1)) ;;
                    esac
                ;;
            esac
        ;;
    esac

    case $_count in 0) return 2 ;; esac

    [ "$((_count % 2))" -eq 0 ]
}
```

## confirm_cont

```sh
#! .desc:
# Ask for confirmation to continue
#! .params:
# <$1> - action(
#     '-n' - default action: N
#     '-y' - default action: Y
#     .
# )
#! .uses:
# [NO_COLOR] $ - this environment variable disables colored output
#! .gives:
# <"$_action"> - the raw input
#! .rc:
# (0) permitted
# (1) forbidden
#.
confirm_cont() {
    case "$1" in
        '-n')
            if [ "$NO_COLOR" ]; then
                printf 'Continue? [Y/n] '
            else
                printf "%bContinue? [Y/n]%b " '\033[1;37m' '\033[0m'
            fi
            read -r _action

            case "$_action" in
                Y*|y*) return 0 ;;
            esac

            return 1
        ;;
        '-y')
            if [ "$NO_COLOR" ]; then
                printf 'Continue? [Y/n] '
            else
                printf "%bContinue? [Y/n]%b " '\033[1;37m' '\033[0m'
            fi
            read -r _action

            case "$_action" in
                N*|n*) return 1 ;;
            esac

            return 0
        ;;
    esac
}
```

## err

```sh
#! .desc:
# Print formatted text to stderr
#! .params:
# <$1> - color(
#     '-'  - none
#     '-bk' - black
#     '-r' - red
#     '-g' - green
#     '-y' - yellow
#     '-be' - blue
#     '-m' - magenta
#     '-c' - cyan
#     '-w' - white
#     .
# )
# <$2> - type(
#     '-' - raw
#     '--' - raw, no newline
#     '-1' - %H%M%S date fmt
#     '-2' - %H%M%S date fmt, no newline
#     .
# )
# <"$3"+> - text
#! .uses:
# [NO_COLOR] $ - this environment variable disables colored output
#! .rc:
# (0) success
# (255) bad input
#.
err() {
    [ "$#" -ge 3 ] || return 255

    case $1 in
        '-') _color= ;;
        '-bk') _color='\033[1;30m' ;;
        '-r') _color='\033[1;31m' ;;
        '-g') _color='\033[1;32m' ;;
        '-y') _color='\033[1;33m' ;;
        '-be') _color='\033[1;34m' ;;
        '-m') _color='\033[1;35m' ;;
        '-c') _color='\033[1;36m' ;;
        '-w') _color='\033[1;37m' ;;
        *) return 255 ;;
    esac

    if [ "$NO_COLOR" ]; then
        case $2 in
            '-')
                shift 2
                printf "%s\n" "$*" >&2
            ;;
            '--')
                shift 2
                printf "%s" "$*" >&2
            ;;
            '-1')
                shift 2
                printf "[%s] =>>: %s\n" "$(date "+%H:%M:%S")" "$*" >&2
            ;;
            '-2')
                shift 2
                printf "[%s] =>>: %s" "$(date "+%H:%M:%S")" "$*" >&2
            ;;
            *)
                return 255
            ;;
        esac
    else
        case $2 in
            '-')
                shift 2
                printf "%b%s%b\n" "$_color" "$*" '\033[0m' >&2
            ;;
            '--')
                shift 2
                printf "%b%s%b" "$_color" "$*" '\033[0m' >&2
            ;;
            '-1')
                shift 2
                printf "%b[%s] =>>: %b%s%b\n" "$_color" "$(date "+%H:%M:%S")" \
                                              '\033[1;37m' "$*" '\033[0m' >&2
            ;;
            '-2')
                shift 2
                printf "%b[%s] =>>: %b%s%b" "$_color" "$(date "+%H:%M:%S")" \
                                            '\033[1;37m' "$*" '\033[0m' >&2
            ;;
            *)
                return 255
            ;;
        esac
    fi
}
```

## errF

```sh
#! .desc:
# Print red-colored formatted text to stderr and exit
#! .params:
# <$1> - exit code
# <$2> - type(
#     '-' - raw
#     '--' - raw, no newline
#     '-1' - %H%M%S date fmt
#     '-2' - %H%M%S date fmt, no newline
#     .
# )
# <"$3"+> - text
#! .uses:
# [NO_COLOR] $ - this environment variable disables colored output
#! .rc:
# (*) success
# (255) bad input
#.
errF() {
    [ "$#" -ge 3 ] || return 255

    # Check if $1 is a whole number
    case :"$1${1#*[!0123456789]}" in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    [ "$1" -le 255 ] || return 255

    _exit_code="$1"

    if [ "$NO_COLOR" ]; then
        case $2 in
            '-')
                shift 2
                printf "%s\n" "$*" >&2
            ;;
            '--')
                shift 2
                printf "%s" "$*" >&2
            ;;
            '-1')
                shift 2
                printf "[%s] =>>: %s\n" "$(date "+%H:%M:%S")" "$*" >&2
            ;;
            '-2')
                shift 2
                printf "[%s] =>>: %s" "$(date "+%H:%M:%S")" "$*" >&2
            ;;
            *)
                return 255
            ;;
        esac
    else
        case $2 in
            '-')
                shift 2
                printf "%b%s%b\n" '\033[1;31m' "$*" '\033[0m' >&2
            ;;
            '--')
                shift 2
                printf "%b%s%b" '\033[1;31m' "$*" '\033[0m' >&2
            ;;
            '-1')
                shift 2
                printf "%b[%s] =>>: %b%s%b\n" '\033[1;31m' \
                                              "$(date "+%H:%M:%S")" \
                                              '\033[1;37m' "$*" '\033[0m' >&2
            ;;
            '-2')
                shift 2
                printf "%b[%s] =>>: %b%s%b" '\033[1;31m' \
                                            "$(date "+%H:%M:%S")" \
                                            '\033[1;37m' "$*" '\033[0m' >&2
            ;;
            *)
                return 255
            ;;
        esac
    fi

    exit "$_exit_code"
}
```

## esc_str

```sh
#! .desc:
# Escape all POSIX-defined shell meta characters in a string; characters are:
# \ | & ; < > ( ) $ ` " ' * ? [ ] # ~ = %
#! .params:
# <"$1"> - string
# ["$2"] - escape_chars(
#     "-chars X" - escape only "X" whitespace-separated character(s)
#     .
# )
# [$3] - options(
#     '-sqesc' - escape single quote character with itself
#     '-strip' - strip the characters
#     .
# )
# [$4] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the modified string
# <"$_chars"> - the characters;
#               whitespace delimited;
#               sorted by order parsed
#! .rc:
# (0) escaped/stripped $1
# (1) no meta characters in $1
#.
esc_str() {
    _str="$1"; _str_ref=

    case "$2" in
        '-chars'*) _chars="${2#'-chars '}" ;;
        *) _chars='\ | & ; < > ( ) $ ` " '\'' * ? [ ] # ~ = %' ;;
    esac

    case "$_chars" in
        \\*) : ;;
        *\\) _chars="\\ ${_chars%%\\*}${_chars#*\\}" ;;
        *\\*) _chars="\\ ${_chars%%\\*}${_chars#*\\ }" ;;
    esac

    set -f; for _char in $_chars; do
        case "$_str" in
            *"$_char"*) : ;;
            *) continue ;;
        esac

        case "$_char$2$3$4" in
            \'*'-sqesc'*)
                _str_ref="${_str%%\'*}'\\''"
            ;;
            *'-strip'*)
                _str_ref="${_str%%"$_char"*}"
            ;;
            *)
                _str_ref="${_str%%"$_char"*}\\$_char"
            ;;
        esac
        _str="$_str_ref${_str#*"$_char"}"

        case "$_char$2$3$4" in
            \'*'-sqesc'*)
                while :; do case "$_str" in
                    "$_str_ref"*\'*)
                        _str="${_str#*"$_str_ref"}"
                        _str_ref="$_str_ref${_str%%\'*}'\\''"
                        _str="$_str_ref${_str#*\'}"
                    ;;
                    *)
                        break
                    ;;
                esac done
            ;;
            *'-strip'*)
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
    done; set +f

    [ "$_str_ref" ] || return 1

    case "$2$3$4" in
        *'-nout'*) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## fcount

```sh
#! .desc:
# Count files and directories in a directory
#! .params:
# <"$1"> - directory
# ["$2"] - count_type(
#     "0X" - count files and directories ending with "X"
#     '1' - count only files
#     "1X" - count only files ending with "X"
#     '2' - count only directories
#     "2X" - count only directories ending with "X"
#     .
# )
# [$3] - options(
#     '3' - exclude hidden files/directories
#     .
# )
# [$4] - type(
#     '4' - no output
#     .
# )
#! .gives:
# (0) <$_count> - the count
#! .rc:
# (0) count
# (1) not a directory | directory does not exist
#! .caveats:
# > The number of files/directories the function (i.e. system) can process
#   varies. The files/directories are stored as arguments, which means that the
#   amount of things that can be processed depends on the system and version,
#   on the number of files and their respective argument size, and on the
#   number and size of environment variable names.
#   >> For more information, related limits shall be checked: ARG_MAX
#   >> Fix: none; not applicable
#.
fcount() {
    fcount_all() {
        _fcount=0

        if [ "$_sfix" ]; then
            set -- "$_dir"/*"$_sfix"
            [ -e "$1" ] && _fcount=$#

            set -- "$_dir"/.*"$_sfix"
            case "$_sfix" in
                '.')
                    [ "$#" -ge 2 ] && _fcount=$((_fcount + $# - 1))
                ;;
                *)
                    [ -e "$1" ] && _fcount=$((_fcount + $#))
                ;;
            esac
        else
            set -- "$_dir"/*
            [ -e "$1" ] && _fcount=$#

            set -- "$_dir"/.*
            [ "$#" -ge 3 ] && _fcount=$((_fcount + $# - 2))
        fi
    }
    fcount_dotdirs() {
        _fcount=0

        if [ "$_sfix" ]; then
            set -- "$_dir"/.*"$_sfix"/
            case "$_sfix" in
                '.')
                    [ "$#" -ge 2 ] && _fcount=$(($# - 1))
                ;;
                *)
                    [ -e "$1" ] && _fcount=$#
                ;;
            esac
        else
            set -- "$_dir"/.*/
            [ "$#" -ge 3 ] && _fcount=$(($# - 2))
        fi
    }
    fcount_dotfiles() {
        _fcount=0

        if [ "$_sfix" ]; then
            set -- "$_dir"/.*"$_sfix"
            case "$_sfix" in
                '.')
                    [ "$#" -ge 2 ] && _fcount=$((_fcount + $# - 1))
                ;;
                *)
                    [ -e "$1" ] && _fcount=$((_fcount + $#))
                ;;
            esac

            set -- "$_dir"/.*"$_sfix"/
            case "$_sfix" in
                '.')
                    [ "$#" -ge 2 ] && _fcount=$((_fcount - $# + 1))
                ;;
                *)
                    [ -e "$1" ] && _fcount=$((_fcount - $#))
                ;;
            esac
        else
            set -- "$_dir"/.*
            [ "$#" -ge 3 ] && _fcount=$(($# - 2))

            set -- "$_dir"/.*/
            [ "$#" -ge 3 ] && _fcount=$((_fcount - $# + 2))
        fi
    }
    fcount_dirs() {
        _fcount=0

        if [ "$_sfix" ]; then
            set -- "$_dir"/*"$_sfix"/
        else
            set -- "$_dir"/*/
        fi
        [ -e "$1" ] && _fcount=$#
    }
    fcount_files() {
        _fcount=0

        if [ "$_sfix" ]; then
            set -- "$_dir"/*"$_sfix"
            [ -e "$1" ] && _fcount=$#

            set -- "$_dir"/*"$_sfix"/
            [ -e "$1" ] && _fcount=$((_fcount - $#))
        else
            set -- "$_dir"/*
            [ -e "$1" ] && _fcount=$#

            set -- "$_dir"/*/
            [ -e "$1" ] && _fcount=$((_fcount - $#))
        fi
    }

    [ -d "$1" ] || return 1

    _count=0
    case "$1" in
        *'/') _dir="${1%?}" ;;
        *) _dir="$1" ;;
    esac
    case "$2" in
        0*|1*|2*) _sfix="${2#?}" ;;
        *) _sfix= ;;
    esac

    case $3:"$2" in
        3:2*)
            fcount_dirs; _count=$_fcount
        ;;
        3:1*)
            fcount_files; _count=$_fcount
        ;;
        3:0*|"$3":3)
            fcount_files; _count=$_fcount
            fcount_dirs; _count=$((_count + $_fcount))
        ;;
        "$3":2*)
            fcount_dirs; _count=$_fcount
            fcount_dotdirs; _count=$((_count + $_fcount))
        ;;
        "$3":1*)
            fcount_files; _count=$_fcount
            fcount_dotfiles; _count=$((_count + $_fcount))
        ;;
        *)
            fcount_all; _count=$_fcount
        ;;
    esac

    case $4:$3:"$2" in
        4*|"$4":4*|"$4":"$3":4) : ;;
        *) printf "%d" "$_count" ;;
    esac
}
```

## get_fpath

```sh
#! .desc:
# Convert relative path to absolute path
#! .params:
# <"$1"> - path
# [$2] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# <"$_path"> - the absolute path | path
#! .rc:
# (0) absolute $1 | $1
#.
get_fpath() {
    case "$1" in
        '/'*) _path="$1" ;;
        *) _path="$PWD/$1" ;;
    esac

    case "$2" in
        '-nout') : ;;
        *) printf "%s" "$_path" ;;
    esac
}
```

## info

```sh
#! .desc:
# Print formatted text to stdout
#! .params:
# <$1> - color(
#     '-'  - none
#     '-bk' - black
#     '-r' - red
#     '-g' - green
#     '-y' - yellow
#     '-be' - blue
#     '-m' - magenta
#     '-c' - cyan
#     '-w' - white
#     .
# )
# <$2> - type(
#     '-' - raw
#     '--' - raw, no newline
#     '-1' - %H%M%S date fmt
#     '-2' - %H%M%S date fmt, no newline
#     .
# )
# <"$3"+> - text
#! .uses:
# [NO_COLOR] $ - this environment variable disables colored output
#! .rc:
# (0) success
# (255) bad input
#.
info() {
    [ "$#" -ge 3 ] || return 255

    case $1 in
        '-') _color= ;;
        '-bk') _color='\033[1;30m' ;;
        '-r') _color='\033[1;31m' ;;
        '-g') _color='\033[1;32m' ;;
        '-y') _color='\033[1;33m' ;;
        '-be') _color='\033[1;34m' ;;
        '-m') _color='\033[1;35m' ;;
        '-c') _color='\033[1;36m' ;;
        '-w') _color='\033[1;37m' ;;
        *) return 255 ;;
    esac

    if [ "$NO_COLOR" ]; then
        case $2 in
            '-')
                shift 2
                printf "%s\n" "$*"
            ;;
            '--')
                shift 2
                printf "%s" "$*"
            ;;
            '-1')
                shift 2
                printf "[%s] =>>: %s\n" "$(date "+%H:%M:%S")" "$*"
            ;;
            '-2')
                shift 2
                printf "[%s] =>>: %s" "$(date "+%H:%M:%S")" "$*"
            ;;
            *)
                return 255
            ;;
        esac
    else
        case $2 in
            '-')
                shift 2
                printf "%b%s%b\n" "$_color" "$*" '\033[0m'
            ;;
            '--')
                shift 2
                printf "%b%s%b" "$_color" "$*" '\033[0m'
            ;;
            '-1')
                shift 2
                printf "%b[%s] =>>: %b%s%b\n" "$_color" "$(date "+%H:%M:%S")" \
                                              '\033[1;37m' "$*" '\033[0m'
            ;;
            '-2')
                shift 2
                printf "%b[%s] =>>: %b%s%b" "$_color" "$(date "+%H:%M:%S")" \
                                            '\033[1;37m' "$*" '\033[0m'
            ;;
            *)
                return 255
            ;;
        esac
    fi
}
```

## lstrip

```sh
#! .desc:
# Strip character(s) from left of string
#! .params:
# <"$1"> - character(s)
# <"$2"> - string
# [$3] - type(
#     '0' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the stripped string
#! .rc:
# (0) stripped $2
# (1) ! $1
# (2) $1 = $2
#.
lstrip() {
    case "$2" in
        "$1") return 2 ;;
        "$1"*) _str="${2#"$1"}"; [ ! "$3" ] && printf "%s" "$_str"; return 0 ;;
    esac

    return 1
}
```

## ltl_substr0

```sh
#! .desc:
# Get positional substring, (left of) (N)character(s), in a string
#! .params:
# <$1> - "N"_LTR(
#     '0' - max
#     .
# )
# <"$2"> - character(s)
# <"$3"> - string
# [$4] - options(
#     '0' - strip all leading whitespace characters
#     '1' - strip all trailing whitespace characters
#     '2' - strip all leading/trailing whitespace characters
#     .
# )
# [$5] - string_options(
#     '3' - keep $2
#     .
# )
# [$6] - accuracy(
#     '4' - verify the expansion
#     .
# )
# [$7] - type(
#     '5' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the substring
# [$_i] - the iterations completed;
#         whole number
#! .rc:
# (0) substring | incorrect substring ($1 > $2)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#! .rc (accuracy '4'):
# (0) substring
# (1) empty expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (255) bad input
#.
ltl_substr0() {
    case :$1${1#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    _str="$3"

    case $6$5$4 in
        *4*)
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
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case $5$4 in
        *3*) _str="$_str$2" ;;
    esac

    case $7$6$5$4 in
        *5*) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## ltl_substr1

```sh
#! .desc:
# Get positional substring, (left of, up to) (N)character(s), in a string
#! .params:
# <$1> - "N"_LTR(
#     '0' - max
#     .
# )
# <"$2"> - left of "X" character(s)
# <$3> - "N"_$2-TL(
#     '0' - max
#     .
# )
# <"$4"> - up to "X" character(s)
# <"$5"> - string
# [$6] - options(
#     '0' - strip all leading whitespace characters
#     '1' - strip all trailing whitespace characters
#     '2' - strip all leading/trailing whitespace characters
#     .
# )
# [$7] - string_options(
#     '3' - keep $2
#     '4' - keep $4
#     '5' - keep $2/$4
#     .
# )
# [$8] - accuracy(
#     '6' - verify the expansion
#     .
# )
# [$9] - type(
#     '7' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the substring
# [$_i] - the iterations completed;
#         whole number
#! .rc:
# (0) substring | incorrect substring ($1/$3 > $2/$4)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#! .rc (accuracy '6'):
# (0) substring
# (1) unspecified expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (4) empty expansion ($2..$4)
# (5) unspecified expansion (! $4)
# (6) incorrect expansion ($3 > $4)
# (255) bad input
#.
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

    case $8$7$6 in
        *6*)
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
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case $7$6 in
        *3*) _str="$_str$2" ;;
        *4*) _str="$4$_str" ;;
        *5*) _str="$4$_str$2" ;;
    esac

    case $9$8$7$6 in
        *7*) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## ltr_substr0

```sh
#! .desc:
# Get positional substring, (right of) (N)character(s), in a string
#! .params:
# <$1> - "N"_LTR(
#     '0' - max
#     .
# )
# <"$2"> - character(s)
# <"$3"> - string
# [$4] - options(
#     '0' - strip all leading whitespace characters
#     '1' - strip all trailing whitespace characters
#     '2' - strip all leading/trailing whitespace characters
#     .
# )
# [$5] - string_options(
#     '3' - keep $2
#     .
# )
# [$6] - accuracy(
#     '4' - verify the expansion
#     .
# )
# [$7] - type(
#     '5' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the substring
# [$_i] - the iterations completed;
#         whole number
#! .rc:
# (0) substring | incorrect substring ($1 > $2)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#! .rc (accuracy '4'):
# (0) substring
# (1) empty expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (255) bad input
#.
ltr_substr0() {
    case :$1${1#*[!0123456789]} in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    _str="$3"

    case $6$5$4 in
        *4*)
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
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case $5$4 in
        *3*) _str="$2$_str" ;;
    esac

    case $7$6$5$4 in
        *5*) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## ltr_substr1

```sh
#! .desc:
# Get positional substring, (right of, up to) (N)character(s), in a string
#! .params:
# <$1> - "N"_LTR(
#     '0' - max
#     .
# )
# <"$2"> - right of "X" character(s)
# <$3> - "N"_$2-TR(
#     '0' - max
#     .
# )
# <"$4"> - up to "X" character(s)
# <"$5"> - string
# [$6] - options(
#     '0' - strip all leading whitespace characters
#     '1' - strip all trailing whitespace characters
#     '2' - strip all leading/trailing whitespace characters
#     .
# )
# [$7] - string_options(
#     '3' - keep $2
#     '4' - keep $4
#     '5' - keep $2/$4
#     .
# )
# [$8] - accuracy(
#     '6' - verify the expansion
#     .
# )
# [$9] - type(
#     '7' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the substring
# [$_i] - the iterations completed;
#         whole number
#! .rc:
# (0) substring | incorrect substring ($1/$3 > $2/$4)
# (1) empty <unspecified/incorrect> expansion
# (2) unspecified <empty/incorrect> expansion
# (255) bad input
#! .rc (accuracy '6'):
# (0) substring
# (1) unspecified expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion (! $2)
# (3) incorrect expansion ($1 > $2)
# (4) empty expansion ($2..$4)
# (5) unspecified expansion (! $4)
# (6) incorrect expansion ($3 > $4)
# (255) bad input
#.
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

    case $8$7$6 in
        *6*)
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
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        1)
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        2)
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case $7$6 in
        *3*) _str="$2$_str" ;;
        *4*) _str="$_str$4" ;;
        *5*) _str="$2$_str$4" ;;
    esac

    case $9$8$7$6 in
        *7*) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## num_to_char

```sh
#! .desc:
# Convert N to that amount of a character
#! .params:
# <$1> - N
# <"$2"> - character
# [$3] - options(
#     '0' - whitespace-separated characters
#     .
# )
# [$4] - type(
#     '1' - no output
#     .
# )
#! .gives:
# (0) <"$_chars"> - the character(s)
#! .rc:
# (0) character(s)
# (255) bad input
#.
num_to_char() {
    case $1:${1#*[!0123456789]} in
        : | 0*) return 255 ;;
        "$1:$1") : ;;
        *) return 255 ;;
    esac

    _chars="$2"

    case $3 in
        0)
            _i=1; until [ "$_i" -eq "$1" ]; do
                _chars="$_chars $2"
                _i=$((_i + 1))
            done
        ;;
        *)
            _i=1; until [ "$_i" -eq "$1" ]; do
                _chars="$_chars$2"
                _i=$((_i + 1))
            done
        ;;
    esac

    case $4$3 in
        *1*) : ;;
        *) printf "%s" "$_chars" ;;
    esac
}
```

## parse

```sh
#! .desc:
# Parse the content of a file
#! .params:
# <"$1"> - file
# [$2] - options(
#     '1' - add one leading/trailing whitespace character
#     '2' - add two leading/trailing whitespace characters
#     '3' - skip empty lines
#     '4' - strip all leading/trailing whitespace characters
#     .
# )
# ["$3"] - parse_options(
#     "5 N" - stop parsing further than "N" line
#     .
# )
#! .rc:
# (0) output | empty output (file) | empty output (by the given ruleset)
# (1) not a file | file does not exist
# (2) file access error
#! .caveats:
# > NULL character.
#   >> Fix: none
# > Reading /proc is unreliable.
#   >> Fix: `cat`
#.
parse() {
    [ -f "$1" ] || return 1
    [ -r "$1" ] || return 2

    case $3:$2 in
        :1)
            while IFS= read -r _line; do
                printf " %s \n" "$_line"
            done < "$1"
        ;;
        :2)
            while IFS= read -r _line; do
                printf "  %s  \n" "$_line"
            done < "$1"
        ;;
        :3)
            while IFS= read -r _line; do
                [ "$_line" ] && printf "%s\n" "$_line"
            done < "$1"
        ;;
        :4)
            while read -r _line; do
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        :5*)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        5*:1)
            _maxN="${3#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf " %s \n" "$_line"
            done < "$1"
        ;;
        5*:2)
            _maxN="${3#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "  %s  \n" "$_line"
            done < "$1"
        ;;
        5*:3)
            _maxN="${3#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                [ "$_line" ] && printf "%s\n" "$_line"
            done < "$1"
        ;;
        5*:4)
            _maxN="${3#??}"; _i=0; while read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        *)
            while IFS= read -r _line; do
                printf "%s\n" "$_line"
            done < "$1"
        ;;
    esac

    case $3:$2 in
        :1|5*:1)
            [ "$_line" ] && printf " %s " "$_line"
        ;;
        :2|5*:2)
            [ "$_line" ] && printf "  %s  " "$_line"
        ;;
        *)
            printf "%s" "$_line"
        ;;
    esac
}
```

## parse_fd1

```sh
#! .desc:
# Parse the content of stdin
#! .params:
# [$1] - options(
#     '1' - add one leading/trailing whitespace character
#     '2' - add two leading/trailing whitespace characters
#     '3' - skip empty lines
#     '4' - strip all leading/trailing whitespace characters
#     .
# )
# ["$2"] - parse_options(
#     "5 N" - stop parsing further than "N" line
#     .
# )
#! .rc:
# (0) output | empty output (stdin) | empty output (by the given ruleset)
#! .caveats:
# > NULL character.
#   >> Fix: none
#.
parse_fd1() {
    case $2:$1 in
        :1)
            while IFS= read -r _line; do
                printf " %s \n" "$_line"
            done
        ;;
        :2)
            while IFS= read -r _line; do
                printf "  %s  \n" "$_line"
            done
        ;;
        :3)
            while IFS= read -r _line; do
                [ "$_line" ] && printf "%s\n" "$_line"
            done
        ;;
        :4)
            while read -r _line; do
                printf "%s\n" "$_line"
            done
        ;;
        :5*)
            _maxN="${1#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done
        ;;
        5*:1)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf " %s \n" "$_line"
            done
        ;;
        5*:2)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "  %s  \n" "$_line"
            done
        ;;
        5*:3)
            _maxN="${2#??}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                [ "$_line" ] && printf "%s\n" "$_line"
            done
        ;;
        5*:4)
            _maxN="${2#??}"; _i=0; while read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done
        ;;
        *)
            while IFS= read -r _line; do
                printf "%s\n" "$_line"
            done
        ;;
    esac

    case $2:$1 in
        :1|5*:1)
            [ "$_line" ] && printf " %s " "$_line"
        ;;
        :2|5*:2)
            [ "$_line" ] && printf "  %s  " "$_line"
        ;;
        *)
            printf "%s" "$_line"
        ;;
    esac
}
```

## pline

```sh
#! .desc:
# Print specific line in file
#! .params:
# <$1> - line number
# <"$2"> - file
# [$3] - type(
#     '0' - no output
#     .
# )
# [$4] - options(
#     '1' - strip all leading/trailing whitespace characters
#     .
# )
#! .gives:
# (0) <"$_line"> - the line
#! .rc:
# (0) line's content | line empty
# (1) line does not exist
# (2) not a file | file does not exist
# (3) file access error
# (255) bad input
#! .caveats:
# > NULL character.
#   >> Fix: none
# > Reading /proc is unreliable.
#   >> Fix: `cat`
#.
pline() {
    case $1:${1#*[!0123456789]} in
        : | 0*) return 255 ;;
        "$1:$1") : ;;
        *) return 255 ;;
    esac

    [ -f "$2" ] || return 2
    [ -r "$2" ] || return 3

    case $4$3 in
        *1*)
            _i=0; while read -r _line || [ "$_line" ]; do
                _i=$((_i + 1))
                case $_i in
                    "$1")
                        case $3 in
                            0) : ;;
                            *) printf "%s" "$_line" ;;
                        esac

                        return 0
                    ;;
                esac
            done < "$2"
        ;;
        *)
            _i=0; while IFS= read -r _line || [ "$_line" ]; do
                _i=$((_i + 1))
                case $_i in
                    "$1")
                        case $3 in
                            0) : ;;
                            *) printf "%s" "$_line" ;;
                        esac

                        return 0
                    ;;
                esac
            done < "$2"
        ;;
    esac

    return 1
}
```

## pline_fd1

```sh
#! .desc:
# Print specific line in stdin
#! .params:
# <$1> - line number
# [$2] - type(
#     '0' - no output
#     .
# )
# [$3] - options(
#     '1' - strip all leading/trailing whitespace characters
#     .
# )
#! .gives:
# (0) <"$_line"> - the line
#! .rc:
# (0) line's content | line empty
# (1) line does not exist
# (255) bad input
#! .caveats:
# > NULL character.
#   >> Fix: none
#.
pline_fd1() {
    case $1:${1#*[!0123456789]} in
        : | 0*) return 255 ;;
        "$1:$1") : ;;
        *) return 255 ;;
    esac

    case $3$2 in
        *1*)
            _i=0; while read -r _line || [ "$_line" ]; do
                _i=$((_i + 1))
                case $_i in
                    "$1")
                        case $2 in
                            0) : ;;
                            *) printf "%s" "$_line" ;;
                        esac

                        return 0
                    ;;
                esac
            done
        ;;
        *)
            _i=0; while IFS= read -r _line || [ "$_line" ]; do
                _i=$((_i + 1))
                case $_i in
                    "$1")
                        case $2 in
                            0) : ;;
                            *) printf "%s" "$_line" ;;
                        esac

                        return 0
                    ;;
                esac
            done
        ;;
    esac

    return 1
}
```

## remchars

```sh
#! .desc:
# Remove specific character(s) in string
#! .params:
# <"$1"> - character(s)
# <"$2"> - string
# [$3] - type(
#     '0' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the modified string
#! .rc:
# (0) modified $2
# (1) ! $1
#.
remchars() {
    remchar() {
        _old_ifs="$IFS"; IFS="$1"

        set -f; set -- $2; set +f
        _str=; while [ "$#" -ge 1 ]; do
            _str="$_str$1"; shift
        done

        IFS="$_old_ifs"
    }

    remchar "$1" "$2"

    case "$_str" in
        "$2") return 1 ;;
    esac

    case $3 in
        0) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## remstr

```sh
#! .desc:
# Remove a positional substring of a string
#! .params:
# <"$1"> - substring
# <"$2"> - string
# <$3> - removal_options(
#     '1' - remove the first occurrence
#     '2' - remove the last occurrence
#     '3' - remove all occurrences
#     .
# )
# [$4] - options(
#     '4' - whitespace is delimiter
#     .
# )
# [$5] - type(
#     '5' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the modified string
#! .rc:
# (0) modified $2
# (1) $1 = $2
# (2) ! $1
#.
remstr() {
    case "$2" in
        "$1") return 1 ;;
        *"$1"*) : ;;
        *) return 2 ;;
    esac

    case $3 in
        1|3)
            _pfix="${2%%"$1"*}"
            _sfix="${2#*"$1"}"
        ;;
        2)
            _pfix="${2%"$1"*}"
            _sfix="${2##*"$1"}"
        ;;
    esac; case $4 in
        4)
            case "$2" in
                *" $1 "* | *" $1"* | *"$1 "*)
                    _pfix="${_pfix%"${_pfix##*[! ]}"}"
                    _sfix="${_sfix#"${_sfix%%[! ]*}"}"

                    if [ "$_pfix" ] && [ "$_sfix" ]; then
                        _pfix="$_pfix "
                    fi
                ;;
            esac
        ;;
    esac; _str="$_pfix$_sfix"

    case $3 in
        3)
            while :; do case "$_str" in
                *"$1"*)
                    _pfix="${_str%%"$1"*}"
                    _sfix="${_str#*"$1"}"

                    case $4 in
                        4)
                            case "$_str" in
                                *" $1 "* | *" $1"* | *"$1 "*)
                                    _pfix="${_pfix%"${_pfix##*[! ]}"}"
                                    _sfix="${_sfix#"${_sfix%%[! ]*}"}"

                                    if [ "$_pfix" ] && [ "$_sfix" ]; then
                                        _pfix="$_pfix "
                                    fi
                                ;;
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

    case $5$4 in
        *5*) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## replchars

```sh
#! .desc:
# Replace specific character(s) with character(s) in string
#! .params:
# <"$1"> - specific character(s)
# <"$2"> - character(s)
# <"$3"> - string
# [$4] - type(
#     '0' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the modified string
#! .rc:
# (0) modified $3
# (1) ! $1
#.
replchars() {
    replchar() {
        _old_ifs="$IFS"; IFS="$1"; _chars="$2"

        set -f; set -- $3 "$3"; set +f
        _str=; while [ "$#" -ge 3 ]; do
            _str="$_str$1$_chars"; shift
        done
        case "$IFS" in
            *"${2#"${2%?}"}"*) _str="$_str$1$_chars" ;;
            *) _str="$_str$1" ;;
        esac

        IFS="$_old_ifs"
    }

    replchar "$1" "$2" "$3"

    case "$_str" in
        "$3") return 1 ;;
    esac

    case $4 in
        0) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## replstr

```sh
#! .desc:
# Replace a substring of a string with character(s)
#! .params:
# <"$1"> - substring
# <"$2"> - string
# <"$3"> - character(s)
# <$4> - replacement_options(
#     '1' - replace the first occurrence
#     '2' - replace the last occurrence
#     '3' - replace all occurrences
#     .
# )
# [$5] - type(
#     '4' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the modified string
#! .rc:
# (0) modified $2
# (1) $1 = $2
# (2) ! $1
#.
replstr() {
    case "$2" in
        "$1") return 1 ;;
        *"$1"*) : ;;
        *) return 2 ;;
    esac

    case $4 in
        1)
            _str="${2%%"$1"*}$3${2#*"$1"}"
        ;;
        2)
            _str="${2%"$1"*}$3${2##*"$1"}"
        ;;
        3)
            _str_ref="${2%%"$1"*}$3"
            _str="$_str_ref${2#*"$1"}"

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

    case $5 in
        4) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## rstrip

```sh
#! .desc:
# Strip character(s) from right of string
#! .params:
# <"$1"> - character(s)
# <"$2"> - string
# [$3] - type(
#     '0' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - the stripped string
#! .rc:
# (0) stripped $2
# (1) ! $1
# (2) $1 = $2
#.
rstrip() {
    case "$2" in
        "$1") return 2 ;;
        *"$1") _str="${2%"$1"}"; [ ! "$3" ] && printf "%s" "$_str"; return 0 ;;
    esac

    return 1
}
```

## sq_arg

```sh
#! .desc:
# Get N single-quoted argument in a shell-folded string
#! .params:
# <$1> - N
# <"$2"> - string
# [$3] - type(
#     '0' - no output
#     .
# )
#! .gives:
# (0) <"$_arg"> - the argument
# (0) <"$_pfix"> - a pattern;
#                  left side of the argument
# (0) <"$_sfix"> - a pattern;
#                  right side of the argument
# [$_i] - the iterations completed;
#         natural number
#! .rc:
# (0) argument
# (1) argument does not exist
# (2) invalid $2
# (255) bad input
#.
sq_arg() {
    char_even() {
        _old_ifs="$IFS"; IFS="$1"

        set -f; case "$2" in
            *"$IFS") set -- $2; _count=$# ;;
            *) set -- $2; _count=$(($# - 1)) ;;
        esac; set +f

        IFS="$_old_ifs"

        [ "$((_count % 2))" -eq 0 ]
    }

    case :$1${1#*[!0123456789]} in
        : | :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    char_even \' "$2" || return 2

    _sfix="$2"; _i=0; while :; do
        _i=$((_i + 1))

        _sfix="${_sfix#*\'*\'}"; while :; do case :"$_sfix" in
            :'\'*) _sfix="${_sfix#???*\'}" ;;
            :' '*) break ;;
            :) [ "$1" -eq "$_i" ] && break || return 1 ;;
            *) return 2 ;;
        esac done

        if [ "$1" -eq "$_i" ]; then
            _pfix="$2"
            _pfix="${_pfix%"$_sfix"}"
            _pfix="${_pfix%\'*\'*}"

            while :; do case :"${_pfix#"${_pfix%??}"}" in
                :"\\'") _pfix="${_pfix%\'*???}" ;;
                :*' ' | :) break ;;
                *) return 2 ;;
            esac done

            _arg="$2"
            _arg="${_arg#"$_pfix"}"
            _arg="${_arg%"$_sfix"}"

            [ ! "$3" ] && printf "%s" "$_arg"
            return 0
        fi
    done
}
```

## str

```sh
#! .desc:
# Check the existence/position of a substring in string
#! .params:
# <"$1"> - substring
# <"$2"> - string
# [$3] - position(
#     '1' - $1 is first character(s) of $2
#     '2' - $1 is last character(s) of $2
#     '3' - $1 is, on its own, $2
#     .
# )
#! .rc:
# (0) match
# (1) no match
#.
str() {
    case $3:"$2" in
        1:"$1"*) return 0 ;;
        2:*"$1") return 0 ;;
        3:"$1") return 0 ;;
        :*"$1"*) return 0 ;;
    esac

    return 1
}
```

## str_fd1

```sh
#! .desc:
# Check the existence/position of a substring in stdin
#! .params:
# <"$1"> - substring
# [$2] - position(
#     '1' - $1 is first character(s) of stdin
#     '2' - $1 is last character(s) of stdin
#     '3' - $1 is, on its own, stdin
#     .
# )
#! .rc:
# (0) match
# (1) no match
#! .caveats:
# > NULL character.
#   >> Fix: none
#.
str_fd1() {
    case $2 in
        1)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in "$1"*) return 0 ;; esac
            done
        ;;
        2)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in *"$1") return 0 ;; esac
            done
        ;;
        3)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in "$1") return 0 ;; esac
            done
        ;;
        *)
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in *"$1"*) return 0 ;; esac
            done
        ;;
    esac

    return 1
}
```

## str_to_chars

```sh
#! .desc:
# Convert a string to whitespace-separated characters
#! .params:
# <"$1"> - string
# [$2] - options(
#     '0' - remove duplicate characters
#     .
# )
# [$3] - type(
#     '1' - no output
#     .
# )
#! .gives:
# (0) <"$_chars"> - the character(s)
#! .rc:
# (0) character(s)
# (1) invalid $1
#! .caveats:
# > Only printable ASCII characters will be included and any characters outside
#   of this set will be excluded.
#   >> The printable set: [\x20-\x7E]; [[:print:]]; ASCII 0x20-0x7E
#   >> Fix: not applicable
#.
str_to_chars() {
    _old_lc="$LC_ALL"; _str="$1"; _chars=

    export LC_ALL=C; case $2 in
        0)
            _chars_set=; while [ "$_str" ]; do
                _char="${_str%"${_str#?}"}"
                _str="${_str#?}"

                case "$_chars_set" in *"$_char"*) continue ;; esac

                case $_char in
                    ' ')
                        :
                    ;;
                    [[:print:]])
                        _chars_set="$_chars_set$_char"
                        _chars="$_chars$_char "
                    ;;
                esac
            done
        ;;
        *)
            while [ "$_str" ]; do
                _char="${_str%"${_str#?}"}"
                _str="${_str#?}"

                case $_char in
                    ' ')
                        :
                    ;;
                    [[:print:]])
                        _chars="$_chars$_char "
                    ;;
                esac
            done
        ;;
    esac; _chars="${_chars%?}"; export LC_ALL="$_old_lc"

    [ "$_chars" ] || return 1

    case $3$2 in
        *1*) : ;;
        *) printf "%s" "$_chars" ;;
    esac
}
```
