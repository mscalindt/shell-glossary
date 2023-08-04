A collection of reusable pure POSIX `sh` functions with optional external
utility calls.

# Normal functions:

* [CCOUNT()](https://github.com/mscalindt/shell-glossary#ccount)
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
* [PATH_SPEC()](https://github.com/mscalindt/shell-glossary#path_spec)
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

## ccount

```sh
#! .desc:
# Count the times a character appears in a string
#! .params:
# <"$1"> - character
# <"$2"> - string
# [$3] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# <$_count> - integer
#! .rc:
# (0) count
# (1) ! $1
#.
ccount() {
    _ccount() {
        # Save IFS
        _old_IFS="$IFS" 2> /dev/null
        ${IFS+':'} unset _old_IFS 2> /dev/null

        IFS="$1"

        set -f; case "$2" in
            *"$IFS") set -- $2; _count=$# ;;
            *) set -- $2; _count=$(($# - 1)) ;;
        esac; set +f

        # Restore IFS
        IFS="$_old_IFS" 2> /dev/null
        ${_old_IFS+':'} unset IFS 2> /dev/null
    }

    _count=0

    case "$2" in
        *"$1"*) : ;;
        *) return 1 ;;
    esac

    _ccount "$1" "$2"

    case "$3" in
        '-nout') : ;;
        *) printf "%s" "$_count" ;;
    esac
}
```

## chars_even

```sh
#! .desc:
# Test if the character(s) count (combined), in a given string, is even
#! .params:
# <"$1"> - character(s)
# <"$2"> - string
#! .gives:
# (0|1) <$_count> - integer
#! .rc:
# (0) even
# (1) uneven
# (2) ! $1
#.
chars_even() {
    char_even() {
        # Save IFS
        _old_IFS="$IFS" 2> /dev/null
        ${IFS+':'} unset _old_IFS 2> /dev/null

        IFS="$1"

        set -f; case "$2" in
            *"$IFS") set -- $2; _count=$# ;;
            *) set -- $2; _count=$(($# - 1)) ;;
        esac; set +f

        # Restore IFS
        IFS="$_old_IFS" 2> /dev/null
        ${_old_IFS+':'} unset IFS 2> /dev/null

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
# [NO_COLOR] $ - environment variable;
#                disables colored output
#! .gives:
# <"$_action"> - string;
#                raw input
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
#     '-date' - %H%M%S date fmt
#     '-date-nolf' - %H%M%S date fmt, no newline
#     .
# )
# <"$3"+> - text
#! .uses:
# [NO_COLOR] $ - environment variable;
#                disables colored output
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
            '-date')
                shift 2
                printf "[%s] =>>: %s\n" "$(date "+%H:%M:%S")" "$*" >&2
            ;;
            '-date-nolf')
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
            '-date')
                shift 2
                printf "%b[%s] =>>: %b%s%b\n" "$_color" "$(date "+%H:%M:%S")" \
                    '\033[1;37m' "$*" '\033[0m' >&2
            ;;
            '-date-nolf')
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
#     '-date' - %H%M%S date fmt
#     '-date-nolf' - %H%M%S date fmt, no newline
#     .
# )
# <"$3"+> - text
#! .uses:
# [NO_COLOR] $ - environment variable;
#                disables colored output
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
            '-date')
                shift 2
                printf "[%s] =>>: %s\n" "$(date "+%H:%M:%S")" "$*" >&2
            ;;
            '-date-nolf')
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
            '-date')
                shift 2
                printf "%b[%s] =>>: %b%s%b\n" '\033[1;31m' \
                    "$(date "+%H:%M:%S")" '\033[1;37m' "$*" '\033[0m' >&2
            ;;
            '-date-nolf')
                shift 2
                printf "%b[%s] =>>: %b%s%b" '\033[1;31m' \
                    "$(date "+%H:%M:%S")" '\033[1;37m' "$*" '\033[0m' >&2
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
# Escape shell-defined meta characters in a string
#! .params:
# <"$1"> - string
# <"$2"> - type(
#     "-chars X" - escape only "X" whitespace-delimited character(s)
#     '-dq' - escape all special characters within double quotes; they are:
#             $ ` " \ [:space:]
#     '-sq' - escape all special characters within single quotes; they are:
#             '
#     '-ws' - escape all whitespace characters
#     .
# )
# <$3> - backend(
#     '-sed' - use the host version of `sed`
#     '-shell' - use built-in shell word splitting
#     .
# )
# [$4] - options(
#     '-strip' - strip the characters instead
#     .
# )
# [$5] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 modified $1
# <$_meta_flag> - integer (bool integer);
#                 '1' if meta characters
# <"$_chars"> - string;
#               character(s) used;
#               whitespace-delimited and sorted by order parsed
#! .rc:
# (0) modified $1
# (1) no meta characters in $1
# (255) bad input/usage
#.
esc_str() {
    dup_chars() {
        _str="$1"
        _index=

        while [ "$_str" ]; do
            _char="${_str%"${_str#?}"}"

            case "$_index" in
                *"$_char"*) return 0 ;;
                *) [ "$_char" = ' ' ] || _index="$_index$_char" ;;
            esac

            _str="${_str#?}"
        done

        return 1
    }

    replchar() {
        case "$3" in
            *"$1"*) : ;;
            *) _str="$3"; return 0 ;;
        esac

        # Save IFS
        _old_IFS="$IFS" 2> /dev/null
        ${IFS+':'} unset _old_IFS 2> /dev/null

        IFS="$1"; _chars="$2"

        set -f; set -- $3 "$3"; set +f
        _str=; while [ "$#" -ge 3 ]; do
            _str="$_str$1$_chars"; shift
        done
        case "$IFS" in
            *"${2#"${2%?}"}"*) _str="$_str$1$_chars" ;;
            *) _str="$_str$1" ;;
        esac

        # Restore IFS
        IFS="$_old_IFS" 2> /dev/null
        ${_old_IFS+':'} unset IFS 2> /dev/null
    }

    # Define sensible constraints for usage.
    [ "${#2}" -le 64 ] || return 255

    case "$2" in
        '-chars '*)
            _chars="${2#'-chars '}"

            if dup_chars "$_chars"; then
                return 255
            fi

            # It is required that `\` is escaped first as to not escape our own
            # escapes since this character is used to escape the other
            # characters. To make sure `\` will always be escaped first,
            # reorder it to always come first if present.
            case "$_chars" in
                \\*) : ;;
                *\\) _chars="\\ ${_chars%%\\*}${_chars#*\\}" ;;
                *\\*) _chars="\\ ${_chars%%\\*}${_chars#*\\ }" ;;
            esac
        ;;
        '-dq')
            _chars='\ $ " `  '
        ;;
        '-sq')
            _chars=\'
        ;;
        '-ws')
            _chars=' '
        ;;
    esac

    _str="$1"
    _meta_flag=0

    _chars_ref="$_chars"; while [ "$_chars_ref" ]; do
        case "$_chars_ref" in
            '   '*)
                _char=' '
                _chars_ref="${_chars_ref#???}"
            ;;
            '  ' | ' ')
                _char=' '
                _chars_ref=
            ;;
            ' '*)
                _chars_ref="${_chars_ref#?}"

                continue
            ;;
            *)
                if [ "${#_chars_ref}" -eq 1 ]; then
                    _char="$_chars_ref"
                    _chars_ref=
                else
                    _char="${_chars_ref%"${_chars_ref#?}"}"
                    _chars_ref="${_chars_ref#?}"
                fi
            ;;
        esac

        case "$_str" in
            *"$_char"*) _meta_flag=1 ;;
            *) continue ;;
        esac

        if [ "$3" = '-sed' ]; then
            if [ "$2" = '-sq' ]; then
                if [ "$4" = '-strip' ]; then
                    _str=$(
                        printf "%s" "$_str" | sed "s/'//g" && \
                        printf x
                    )
                else
                    _str=$(
                        printf "%s" "$_str" | sed "s/'/'\\\\''/g" && \
                        printf x
                    )
                fi
                _str="${_str%?}"
            else
                _str_ref="$_str"

                replchar '\' '\\' "$_char"
                replchar '.' '\.' "$_str"
                replchar '*' '\*' "$_str"
                replchar '[' '\[' "$_str"
                replchar ']' '\]' "$_str"
                replchar '^' '\^' "$_str"
                replchar '$' '\$' "$_str"
                replchar '/' '\/' "$_str"
                _mp="$_str"

                replchar '\' '\\' "$_char"
                replchar '&' '\&' "$_str"
                replchar '/' '\/' "$_str"
                _rp="$_str"

                _str="$_str_ref"

                if [ "$4" = '-strip' ]; then
                    _str=$(
                        printf "%s" "$_str" | sed "s/$_mp//g" && \
                        printf x
                    )
                else
                    _str=$(
                        printf "%s" "$_str" | sed "s/$_mp/\\\\$_rp/g" && \
                        printf x
                    )
                fi
                _str="${_str%?}"
            fi
        elif [ "$3" = '-shell' ]; then
            if [ "$2" = '-sq' ]; then
                if [ "$4" = '-strip' ]; then
                    replchar \' '' "$_str"
                else
                    replchar \' "'\\''" "$_str"
                fi
            else
                if [ "$4" = '-strip' ]; then
                    replchar "$_char" '' "$_str"
                else
                    replchar "$_char" "\\$_char" "$_str"
                fi
            fi
        fi
    done

    [ "$_meta_flag" -eq 1 ] || return 1

    case "$4$5" in
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
# <"$1"> - absolute path of a directory
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
# (0) <$_count> - integer
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
# <"$_path"> - string;
#              [absolute <$1>]
#! .rc:
# (0) [absolute <$1>]
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
#     '-date' - %H%M%S date fmt
#     '-date-nolf' - %H%M%S date fmt, no newline
#     .
# )
# <"$3"+> - text
#! .uses:
# [NO_COLOR] $ - environment variable;
#                disables colored output
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
            '-date')
                shift 2
                printf "[%s] =>>: %s\n" "$(date "+%H:%M:%S")" "$*"
            ;;
            '-date-nolf')
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
            '-date')
                shift 2
                printf "%b[%s] =>>: %b%s%b\n" "$_color" "$(date "+%H:%M:%S")" \
                    '\033[1;37m' "$*" '\033[0m'
            ;;
            '-date-nolf')
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
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 stripped $2
#! .rc:
# (0) stripped $2
# (1) ! $1
# (2) $1 = $2
#.
lstrip() {
    case "$2" in
        "$1")
            return 2
        ;;
        "$1"*)
            _str="${2#"$1"}"

            case "$3" in
                '-nout') : ;;
                *) printf "%s" "$_str" ;;
            esac

            return 0
        ;;
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
#     '-stripl' - strip all leading whitespace characters
#     '-stript' - strip all trailing whitespace characters
#     '-ltstrip' - strip all leading/trailing whitespace characters
#     .
# )
# [$5] - string_options(
#     '-keepl' - keep $2
#     .
# )
# [$6] - accuracy(
#     '-verifyexp' - verify the expansion
#     .
# )
# [$7] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 substring
# [$_i] - integer;
#         iterations completed
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

    case "$4$5$6" in
        *'-verifyexp'*)
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

    case "$4" in
        '-stripl')
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        '-stript')
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        '-ltstrip')
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case "$4$5" in
        *'-keepl'*) _str="$_str$2" ;;
    esac

    case "$4$5$6$7" in
        *'-nout'*) : ;;
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
#     '-stripl' - strip all leading whitespace characters
#     '-stript' - strip all trailing whitespace characters
#     '-ltstrip' - strip all leading/trailing whitespace characters
#     .
# )
# [$7] - string_options(
#     '-keepl' - keep $2
#     '-keepu' - keep $4
#     '-lukeep' - keep $2/$4
#     .
# )
# [$8] - accuracy(
#     '-verifyexp' - verify the expansion
#     .
# )
# [$9] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 substring
# [$_i] - integer;
#         iterations completed
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

    case "$6$7$8" in
        *'-verifyexp'*)
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

    case "$6" in
        '-stripl')
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        '-stript')
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        '-ltstrip')
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case "$6$7" in
        *'-keepl'*) _str="$_str$2" ;;
        *'-keepu'*) _str="$4$_str" ;;
        *'-lukeep'*) _str="$4$_str$2" ;;
    esac

    case "$6$7$8$9" in
        *'-nout'*) : ;;
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
#     '-stripl' - strip all leading whitespace characters
#     '-stript' - strip all trailing whitespace characters
#     '-ltstrip' - strip all leading/trailing whitespace characters
#     .
# )
# [$5] - string_options(
#     '-keepr' - keep $2
#     .
# )
# [$6] - accuracy(
#     '-verifyexp' - verify the expansion
#     .
# )
# [$7] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 substring
# [$_i] - integer;
#         iterations completed
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

    case "$4$5$6" in
        *'-verifyexp'*)
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

    case "$4" in
        '-stripl')
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        '-stript')
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        '-ltstrip')
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case "$4$5" in
        *'-keepr'*) _str="$2$_str" ;;
    esac

    case "$4$5$6$7" in
        *'-nout'*) : ;;
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
#     '-stripl' - strip all leading whitespace characters
#     '-stript' - strip all trailing whitespace characters
#     '-ltstrip' - strip all leading/trailing whitespace characters
#     .
# )
# [$7] - string_options(
#     '-keepr' - keep $2
#     '-keepu' - keep $4
#     '-rukeep' - keep $2/$4
#     .
# )
# [$8] - accuracy(
#     '-verifyexp' - verify the expansion
#     .
# )
# [$9] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 substring
# [$_i] - integer;
#         iterations completed
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

    case "$6$7$8" in
        *'-verifyexp'*)
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

    case "$6" in
        '-stripl')
            _str="${_str#"${_str%%[! ]*}"}"
        ;;
        '-stript')
            _str="${_str%"${_str##*[! ]}"}"
        ;;
        '-ltstrip')
            _str="${_str#"${_str%%[! ]*}"}"
            _str="${_str%"${_str##*[! ]}"}"
        ;;
    esac

    case "$6$7" in
        *'-keepr'*) _str="$2$_str" ;;
        *'-keepu'*) _str="$_str$4" ;;
        *'-rukeep'*) _str="$2$_str$4" ;;
    esac

    case "$6$7$8$9" in
        *'-nout'*) : ;;
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
#     '-wschars' - whitespace-separated characters
#     .
# )
# [$4] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_chars"> - string
#! .rc:
# (0) character(s)
# (255) bad input
#.
num_to_char() {
    # Check if $1 is a whole number
    case :"$1${1#*[!0123456789]}" in
        :) return 255 ;;
        :00) : ;;
        :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    case $1 in
        0) _chars=; return 0 ;;
    esac

    _chars="$2"

    case "$3" in
        '-wschars')
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

    case "$3$4" in
        *'-nout'*) : ;;
        *) printf "%s" "$_chars" ;;
    esac
}
```

## parse

```sh
#! .desc:
# Parse the content of a file
#! .params:
# <"$1"> - absolute path of a file
# [$2] - options(
#     '1' - add one leading/trailing whitespace character
#     '2' - add two leading/trailing whitespace characters
#     '3' - skip empty lines
#     '4' - strip all leading/trailing whitespace characters
#     .
# )
# ["$3"] - parse_options(
#     "-stopat N" - stop parsing further than "N" line
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

    case "$3:$2" in
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
        :'-stopat'*)
            _maxN="${2#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done < "$1"
        ;;
        '-stopat'*:1)
            _maxN="${3#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf " %s \n" "$_line"
            done < "$1"
        ;;
        '-stopat'*:2)
            _maxN="${3#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "  %s  \n" "$_line"
            done < "$1"
        ;;
        '-stopat'*:3)
            _maxN="${3#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                [ "$_line" ] && printf "%s\n" "$_line"
            done < "$1"
        ;;
        '-stopat'*:4)
            _maxN="${3#'-stopat '}"; _i=0; while read -r _line; do
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

    case "$3:$2" in
        :1|'-stopat'*:1)
            [ "$_line" ] && printf " %s " "$_line"
        ;;
        :2|'-stopat'*:2)
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
#     "-stopat N" - stop parsing further than "N" line
#     .
# )
#! .rc:
# (0) output | empty output (stdin) | empty output (by the given ruleset)
#! .caveats:
# > NULL character.
#   >> Fix: none
#.
parse_fd1() {
    case "$2:$1" in
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
        :'-stopat'*)
            _maxN="${1#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "%s\n" "$_line"
            done
        ;;
        '-stopat'*:1)
            _maxN="${2#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf " %s \n" "$_line"
            done
        ;;
        '-stopat'*:2)
            _maxN="${2#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                printf "  %s  \n" "$_line"
            done
        ;;
        '-stopat'*:3)
            _maxN="${2#'-stopat '}"; _i=0; while IFS= read -r _line; do
                _i=$((_i + 1))
                case $_i in "$_maxN") break ;; esac
                [ "$_line" ] && printf "%s\n" "$_line"
            done
        ;;
        '-stopat'*:4)
            _maxN="${2#'-stopat '}"; _i=0; while read -r _line; do
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

    case "$2:$1" in
        :1|'-stopat'*:1)
            [ "$_line" ] && printf " %s " "$_line"
        ;;
        :2|'-stopat'*:2)
            [ "$_line" ] && printf "  %s  " "$_line"
        ;;
        *)
            printf "%s" "$_line"
        ;;
    esac
}
```

## path_spec

```sh
#! .desc:
# Mangle and assert a path in a specified way
#! .params:
# <$1> - options(
#     '-assert' - assert dir/file
#     '-join' - join the two paths
#     '-join-assert' - join the two paths and assert dir/file
#     .
# )
# <"$2"> - absolute path
# ["$3"] - join path
# [$4] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_path"> - string;
#                  [modified <$2>]
#! .rc:
# (0) [modified <$2>]
# (255) bad input
#.
path_spec() {
    case "$2" in
        '/'*) : ;;
        *) return 255 ;;
    esac

    case "$1" in
        '-join' | '-join-assert')
            { [ "$3" ] && [ "$3" != '-nout' ]; } || return 255
        ;;
    esac

    while :; do case "$2" in
        *'///'*)
            return 255
        ;;
        *'//'*)
            set -- "$1" "${2%'//'*}/${2#*'//'}" "$3" "$4"
        ;;
        *)
            break
        ;;
    esac done

    case "$1" in
        '-join' | '-join-assert')
            while :; do case "$3" in
                *'///'*)
                    return 255
                ;;
                *'//'*)
                    set -- "$1" "$2" "${3%'//'*}/${3#*'//'}" "$4"
                ;;
                *)
                    break
                ;;
            esac done

            case "$2" in
                *'/') set -- "$1" "${2%?}" "$3" "$4" ;;
            esac

            case "$3" in
                '/'*) set -- "$1" "$2" "${3#?}" "$4" ;;
            esac
        ;;
    esac

    case "$1" in
        '-assert')
            case "$2" in
                *'/') set -- "$1" "${2%?}" "$3" "$4" ;;
            esac

            _path="$2"
        ;;
        '-join')
            _path="$2/$3"
        ;;
        '-join-assert')
            case "$3" in
                *'/') set -- "$1" "$2" "${3%?}" "$4" ;;
            esac

            _path="$2/$3"
        ;;
    esac

    case "$1" in
        '-assert' | '-join-assert')
            if [ -d "$_path" ]; then
                _path="$_path/"
            fi
        ;;
    esac

    case "$4$3" in
        '-nout'*) : ;;
        *) printf "%s" "$_path" ;;
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
# [$3] - options(
#     '-ltstrip' - strip all leading/trailing whitespace characters
#     .
# )
# [$4] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_line"> - string
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

    case "$3" in
        '-ltstrip')
            _i=0; while read -r _line || [ "$_line" ]; do
                _i=$((_i + 1))
                case $_i in
                    "$1")
                        case "$3$4" in
                            *'-nout'*) : ;;
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
                        case "$3$4" in
                            *'-nout'*) : ;;
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
# [$2] - options(
#     '-ltstrip' - strip all leading/trailing whitespace characters
#     .
# )
# [$3] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_line"> - string
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

    case "$2" in
        '-ltstrip')
            _i=0; while read -r _line || [ "$_line" ]; do
                _i=$((_i + 1))
                case $_i in
                    "$1")
                        case "$2$3" in
                            *'-nout'*) : ;;
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
                        case "$2$3" in
                            *'-nout'*) : ;;
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
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 modified $2
#! .rc:
# (0) modified $2
# (1) ! $1
#.
remchars() {
    remchar() {
        # Save IFS
        _old_IFS="$IFS" 2> /dev/null
        ${IFS+':'} unset _old_IFS 2> /dev/null

        IFS="$1"

        set -f; set -- $2; set +f
        _str=; while [ "$#" -ge 1 ]; do
            _str="$_str$1"; shift
        done

        # Restore IFS
        IFS="$_old_IFS" 2> /dev/null
        ${_old_IFS+':'} unset IFS 2> /dev/null
    }

    remchar "$1" "$2"

    case "$_str" in
        "$2") return 1 ;;
    esac

    case "$3" in
        '-nout') : ;;
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
#     '-remfst' - remove the first occurrence
#     '-remlst' - remove the last occurrence
#     '-remall' - remove all occurrences
#     .
# )
# [$4] - options(
#     '-delimws' - whitespace is delimiter
#     .
# )
# [$5] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 modified $2
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

    case "$3" in
        '-remfst' | '-remall')
            _pfix="${2%%"$1"*}"
            _sfix="${2#*"$1"}"
        ;;
        '-remlst')
            _pfix="${2%"$1"*}"
            _sfix="${2##*"$1"}"
        ;;
    esac; case "$4" in
        '-delimws')
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

    case "$3" in
        '-remall')
            while :; do case "$_str" in
                *"$1"*)
                    _pfix="${_str%%"$1"*}"
                    _sfix="${_str#*"$1"}"

                    case "$4" in
                        '-delimws')
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

    case "$4$5" in
        *'-nout'*) : ;;
        *) printf "%s" "$_str" ;;
    esac
}
```

## replchars

```sh
#! .desc:
# Replace specific character(s) with character(s) in string
#! .params:
# <$1> - backend(
#     '-sed' - use the host version of `sed`
#     '-shell' - use built-in shell word splitting
#     .
# )
# <"$2"> - specific character(s)
# <"$3"> - character(s)
# <"$4"> - string
# [$5] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 modified $4
#! .rc:
# (0) modified $4
# (1) ! $2
# (255) bad input/usage
#.
replchars() {
    replchar() {
        case "$3" in
            *"$1"*) : ;;
            *) _str="$3"; return 0 ;;
        esac

        # Save IFS
        _old_IFS="$IFS" 2> /dev/null
        ${IFS+':'} unset _old_IFS 2> /dev/null

        IFS="$1"; _chars="$2"

        set -f; set -- $3 "$3"; set +f
        _str=; while [ "$#" -ge 3 ]; do
            _str="$_str$1$_chars"; shift
        done
        case "$IFS" in
            *"${2#"${2%?}"}"*) _str="$_str$1$_chars" ;;
            *) _str="$_str$1" ;;
        esac

        # Restore IFS
        IFS="$_old_IFS" 2> /dev/null
        ${_old_IFS+':'} unset IFS 2> /dev/null
    }

    # Define sensible constraints for usage.
    [ "${#2}" -le 32 ] || return 255
    [ "${#3}" -le 6 ] || return 255

    if [ "$1" = '-sed' ]; then
        replchar '\' '\\' "$2"
        replchar '.' '\.' "$_str"
        replchar '*' '\*' "$_str"
        replchar '[' '\[' "$_str"
        replchar ']' '\]' "$_str"
        replchar '^' '\^' "$_str"
        replchar '$' '\$' "$_str"
        replchar '/' '\/' "$_str"
        _mp="$_str"

        replchar '\' '\\' "$3"
        replchar '&' '\&' "$_str"
        replchar '/' '\/' "$_str"
        _rp="$_str"

        _str=$(printf "%s" "$4" | sed "s/$_mp/$_rp/g" && printf x)
        _str="${_str%?}"
    elif [ "$1" = '-shell' ]; then
        replchar "$2" "$3" "$4"
    fi

    case "$_str" in
        "$4") return 1 ;;
    esac

    case "$5" in
        '-nout') : ;;
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
#     '-replfst' - replace the first occurrence
#     '-repllst' - replace the last occurrence
#     '-replall' - replace all occurrences
#     .
# )
# [$5] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 modified $2
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

    case "$4" in
        '-replfst')
            _str="${2%%"$1"*}$3${2#*"$1"}"
        ;;
        '-repllst')
            _str="${2%"$1"*}$3${2##*"$1"}"
        ;;
        '-replall')
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

    case "$5" in
        '-nout') : ;;
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
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_str"> - string;
#                 stripped $2
#! .rc:
# (0) stripped $2
# (1) ! $1
# (2) $1 = $2
#.
rstrip() {
    case "$2" in
        "$1")
            return 2
        ;;
        *"$1")
            _str="${2%"$1"}"

            case "$3" in
                '-nout') : ;;
                *) printf "%s" "$_str" ;;
            esac

            return 0
        ;;
    esac

    return 1
}
```

## sq_arg

```sh
#! .desc:
# Get a single-quoted argument in a single-quote-escaped string
#! .params:
# <$1> - arg(
#     '-first' - get the first argument
#     '-last' - get the last argument
#     "-N" - get "N" argument
#     .
# )
# <"$2"> - string
# [$3] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_arg"> - string;
#                 argument
# (0) <"$_pfix"> - string;
#                  left side single-quote-escaped pattern of argument
# (0) <"$_sfix"> - string;
#                  right side single-quote-escaped pattern of argument
# (0) <$_i> - integer;
#             iterations completed
#! .rc:
# (0) argument
# (1) argument does not exist
# (2) invalid $2
# (255) bad input
#.
sq_arg() {
    char_even() {
        # Save IFS
        _old_IFS="$IFS" 2> /dev/null
        ${IFS+':'} unset _old_IFS 2> /dev/null

        IFS="$1"

        set -f; case "$2" in
            *"$IFS") set -- $2; _count=$# ;;
            *) set -- $2; _count=$(($# - 1)) ;;
        esac; set +f

        # Restore IFS
        IFS="$_old_IFS" 2> /dev/null
        ${_old_IFS+':'} unset IFS 2> /dev/null

        [ "$((_count % 2))" -eq 0 ]
    }

    char_even \' "$2" || return 2

    case "$2" in
        \'*\') : ;;
        *) return 2 ;;
    esac

    _i=0

    case "$1" in
        '-first')
            case "$2" in
                *" '"*)
                    _sfix="'${2#*\' \'}"; _pfix=

                    _arg="$2"
                    _arg="${_arg%%\' \'*}"
                    _arg="${_arg#?}"
                ;;
                *)
                    _sfix=; _pfix=

                    _arg="$2"
                    _arg="${_arg#?}"
                    _arg="${_arg%?}"
                ;;
            esac

            case "$3" in
                '-nout') : ;;
                *) printf "%s" "$_arg" ;;
            esac
            return 0
        ;;
        '-last')
            case "$2" in
                *" '"*)
                    _pfix="${2%\' \'*}'"; _sfix=

                    _arg="$2"
                    _arg="${_arg##*\' \'}"
                    _arg="${_arg%?}"
                ;;
                *)
                    _pfix=; _sfix=

                    _arg="$2"
                    _arg="${_arg#?}"
                    _arg="${_arg%?}"
                ;;
            esac

            case "$3" in
                '-nout') : ;;
                *) printf "%s" "$_arg" ;;
            esac
            return 0
        ;;
        *)
            if [ "$3" ]; then
                set -- "${1#?}" "$2" "$3"
            else
                set -- "${1#?}" "$2"
            fi
        ;;
    esac

    case :$1${1#*[!0123456789]} in
        : | :0*) return 255 ;;
        :"$1$1") : ;;
        *) return 255 ;;
    esac

    _sfix="$2"; while :; do
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

            if [ "$_pfix" ]; then
                _pfix="${_pfix%?}"
            fi
            if [ "$_sfix" ]; then
                _sfix="${_sfix#?}"
            fi
            _arg="${_arg#?}"
            _arg="${_arg%?}"

            case "$3" in
                '-nout') : ;;
                *) printf "%s" "$_arg" ;;
            esac
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
#     '-posfst' - $1 is first character(s) of $2
#     '-poslst' - $1 is last character(s) of $2
#     '-same' - $1 is, on its own, $2
#     .
# )
#! .rc:
# (0) match
# (1) no match
#.
str() {
    case "$2":"$3" in
        "$1"*:'-posfst') return 0 ;;
        *"$1":'-poslst') return 0 ;;
        "$1":'-same') return 0 ;;
        *"$1"*:) return 0 ;;
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
#     '-posfst' - $1 is first character(s) of stdin
#     '-poslst' - $1 is last character(s) of stdin
#     '-same' - $1 is, on its own, stdin
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
    case "$2" in
        '-posfst')
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in "$1"*) return 0 ;; esac
            done
        ;;
        '-poslst')
            while IFS= read -r _line || [ "$_line" ]; do
                case "$_line" in *"$1") return 0 ;; esac
            done
        ;;
        '-same')
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
#     '-remdup' - remove duplicate characters
#     .
# )
# [$3] - type(
#     '-nout' - no output
#     .
# )
#! .gives:
# (0) <"$_chars"> - string
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

    export LC_ALL=C; case "$2" in
        '-remdup')
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

    case "$2$3" in
        *'-nout'*) : ;;
        *) printf "%s" "$_chars" ;;
    esac
}
```
