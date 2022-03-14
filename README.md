A collection of reusable pure POSIX `sh` functions with no external binary calls (on somewhat modern shells where `read`, `printf`, and `echo` are builtins).

# Normal functions:

* [CONFIRM_CONT()](https://github.com/mscalindt/shell-glossary#confirm_cont)
* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [ERR_NE()](https://github.com/mscalindt/shell-glossary#err_ne)
* [ERR_PX()](https://github.com/mscalindt/shell-glossary#err_px)
* [ERR_NE_PX()](https://github.com/mscalindt/shell-glossary#err_ne_px)
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/12
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/6
* [GREP_STR()](https://github.com/mscalindt/shell-glossary#grep_str) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/7.1
* [INFO()](https://github.com/mscalindt/shell-glossary#info)
* [INFO_PX()](https://github.com/mscalindt/shell-glossary#info_px)
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
* [SAFE_STR()](https://github.com/mscalindt/shell-glossary#safe_str) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/11
* [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary#str_to_chars) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/10
* [WARN()](https://github.com/mscalindt/shell-glossary#warn)
* [WARN_PX()](https://github.com/mscalindt/shell-glossary#warn_px)

# Normal functions (color):

* [CONFIRM_CONT_CLR()](https://github.com/mscalindt/shell-glossary#confirm_cont_clr)
* [ERR_CLR()](https://github.com/mscalindt/shell-glossary#err_clr)
* [ERR_NE_CLR()](https://github.com/mscalindt/shell-glossary#err_ne_clr)
* [ERR_PX_CLR()](https://github.com/mscalindt/shell-glossary#err_px_clr)
* [ERR_NE_PX_CLR()](https://github.com/mscalindt/shell-glossary#err_ne_px_clr)
* [INFO_CLR()](https://github.com/mscalindt/shell-glossary#info_clr)
* [INFO_PX_CLR()](https://github.com/mscalindt/shell-glossary#info_px_clr)
* [WARN_CLR()](https://github.com/mscalindt/shell-glossary#warn_clr)
* [WARN_PX_CLR()](https://github.com/mscalindt/shell-glossary#warn_px_clr)

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
        read -r i

        case "$i" in
            N*|n*) return 1 ;;
        esac

        return 0
    ;;
    1)
        printf "Continue? [y/N] "
        read -r i

        case "$i" in
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
        read -r i

        case "$i" in
            N*|n*) return 1 ;;
        esac

        return 0
    ;;
    1)
        printf "%bContinue? [y/N]%b " "\033[1;37m" "\033[0m"
        read -r i

        case "$i" in
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
    i=$1 && shift
    printf "\nERROR: %s\n\n" "$*" 1>&2
    exit $i
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
    i=$1 && shift
    printf "\n%bERROR:%b %s\n\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit $i
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

## err_px

```sh
# Description:
# Print error with printf prefix before text and exit
#
# Parameters:
# <$1> - exit code
# <"$2"> - printf prefix
# <"$3"+> - text
#
err_px() {
    i=$1 && ii="$2"; shift 2
    printf "\nERROR: $ii%s\n\n" "$*" 1>&2
    exit $i
}
```

## err_px_clr

```sh
# Description:
# Print colorful error with printf prefix before text and exit
#
# Parameters:
# <$1> - exit code
# <"$2"> - printf prefix
# <"$3"+> - text
#
err_px_clr() {
    i=$1 && ii="$2"; shift 2
    printf "\n%bERROR:%b $ii%s\n\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit $i
}
```

## err_ne_px

```sh
# Description:
# Print error with printf prefix before text, no exit
#
# Parameters:
# <"$1"> - printf prefix
# <"$2"+> - text
#
# Returns:
# (0) text
#
err_ne_px() {
    i="$1" && shift
    printf "ERROR: $i%s\n" "$*" 1>&2
}
```

## err_ne_px_clr

```sh
# Description:
# Print colorful error with printf prefix before text, no exit
#
# Parameters:
# <"$1"> - printf prefix
# <"$2"+> - text
#
# Returns:
# (0) text
#
err_ne_px_clr() {
    i="$1" && shift
    printf "%bERROR:%b $i%s\n" "\033[1;31m" "\033[0m" "$*" 1>&2
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

    i="$1"
    iii=0

    case "$2" in
        0*|1*|2*) ii="${2#?}" ;;
    esac

    case $#:"$2" in
        1:)
            set -- "$i"/*
            [ -e "$1" ] && iii=$#

            set -- "$i"/.*
            [ $# -ge 3 ] && iii=$((iii + $# - 2))
        ;;
        2:0*)
            set -- "$i"/*"$ii"
            [ -e "$1" ] && iii=$#

            set -- "$i"/.*"$ii"
            case "$ii" in
                ".")
                    [ -e "$2" ] && iii=$((iii + $# - 1))
                ;;
                "..")
                    [ -e "$1" ] && iii=$((iii + $#))
                ;;
                *)
                    [ -e "$i/$ii" ] && iii=$((iii + 1))
                    [ -e "$1" ] && iii=$((iii + $#))
                ;;
            esac
        ;;
        2:1)
            set -- "$i"/*
            [ -e "$1" ] && iii=$#

            set -- "$i"/*/
            [ -e "$1" ] && iii=$((iii - $#))

            set -- "$i"/.*
            [ -e "$3" ] && iii=$((iii + $# - 2))

            set -- "$i"/.*/
            [ -e "$3" ] && iii=$((iii - $# + 2))
        ;;
        2:1*)
            [ -f "$i/$ii" ] && iii=1

            set -- "$i"/*"$ii"
            [ -e "$1" ] && iii=$((iii + $#))

            set -- "$i"/.*"$ii"
            case "$ii" in
                ".") [ -e "$2" ] && iii=$((iii + $# - 1)) ;;
                *) [ -e "$1" ] && iii=$((iii + $#)) ;;
            esac

            set -- "$i"/.*"$ii"/
            case "$ii" in
                ".") [ -e "$2" ] && iii=$((iii - $# + 1)) ;;
                *) [ -e "$1" ] && iii=$((iii - $#)) ;;
            esac

            set -- "$i"/*"$ii"/
            [ -e "$1" ] && iii=$((iii - $#))
        ;;
        2:2)
            set -- "$i"/*/
            [ -e "$1" ] && iii=$#

            set -- "$i"/.*/
            [ -e "$3" ] && iii=$((iii + $# - 2))
        ;;
        2:2*)
            set -- "$i"/*"$ii"/
            [ -e "$1" ] && iii=$#

            set -- "$i"/.*"$ii"/
            case "$ii" in
                ".")
                    [ -e "$2" ] && iii=$((iii + $# - 1))
                ;;
                "..")
                    [ -e "$1" ] && iii=$((iii + $#))
                ;;
                *)
                    [ -d "$i/$ii" ] && iii=$((iii + 1))
                    [ -e "$1" ] && iii=$((iii + $#))
                ;;
            esac
        ;;
        2:3)
            set -- "$i"/*
            [ -e "$1" ] && iii=$#
        ;;
        3:0*)
            set -- "$i"/*"$ii"
            [ -e "$1" ] && iii=$#
        ;;
        3:1)
            set -- "$i"/*
            [ -e "$1" ] && iii=$#

            set -- "$i"/*/
            [ -e "$1" ] && iii=$((iii - $#))
        ;;
        3:1*)
            set -- "$i"/*"$ii"
            [ -e "$1" ] && iii=$#

            set -- "$i"/*"$ii"/
            [ -e "$1" ] && iii=$((iii - $#))
        ;;
        3:2)
            set -- "$i"/*/
            [ -e "$1" ] && iii=$#
        ;;
        3:2*)
            set -- "$i"/*"$ii"/
            [ -e "$1" ] && iii=$#
        ;;
    esac

    printf "%d" "$iii"
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
            while IFS= read -r LINE || [ "$LINE" ]; do
                case "$LINE" in *"$1"*) return 0 ;; esac
            done
        ;;
        2:1)
            while IFS= read -r LINE || [ "$LINE" ]; do
                case "$LINE" in "$1"*) return 0 ;; esac
            done
        ;;
        2:2)
            while IFS= read -r LINE || [ "$LINE" ]; do
                case "$LINE" in *"$1") return 0 ;; esac
            done
        ;;
        2:3)
            while IFS= read -r LINE || [ "$LINE" ]; do
                case "$LINE" in "$1") return 0 ;; esac
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

## info_px

```sh
# Description:
# Print info with printf prefix before text
#
# Parameters:
# <"$1"> - printf prefix
# <"$2"+> - text
#
# Returns:
# (0) text
#
info_px() {
    i="$1" && shift
    printf "INFO: $i%s\n" "$*"
}
```

## info_px_clr

```sh
# Description:
# Print colorful info with printf prefix before text
#
# Parameters:
# <"$1"> - printf prefix
# <"$2"+> - text
#
# Returns:
# (0) text
#
info_px_clr() {
    i="$1" && shift
    printf "%bINFO:%b $i%s\n" "\033[1;37m" "\033[0m" "$*"
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
#
lstrip() {
    case "$2" in
        "$1"*) printf "%s" "${2#"$1"}" && return 0 ;;
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
    case $#:$5$4 in
        6:*|5:4*|4:4)
            case $1 in
                0)
                    case "$3" in
                        *"$2"*"$2"*) : ;;
                        "$2"*) return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    i="${3%"$2"*}"
                ;;
                *)
                    case $1"$3" in
                        1"$2"*) return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    x=0 && i="$3"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"

                        case "$i" in
                            *"$2"*) : ;;
                            *) [ $((x + 1)) -eq $1 ] || return 3 ;;
                        esac

                        x=$((x + 1))
                    done

                    case ":$i" in
                        :) i="${3%"$2"}" ;;
                        *) i="${3%"$2""$i"}" ;;
                    esac
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    i="${3%"$2"*}"
                ;;
                *)
                    x=0 && i="$3"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"
                        x=$((x + 1))
                    done
                    i="${3%"$2""$i"}"
                ;;
            esac

            [ "$i" = "$3" ] && return 2
        ;;
    esac

    [ "$i" ] || return 1

    case $4 in
        0)
            i="${i#${i%%[![:space:]]*}}"
        ;;
        1)
            i="${i%${i##*[![:space:]]}}"
        ;;
        2)
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        ;;
    esac

    case $5$4 in
        *3*) i="$i$2" ;;
    esac

    printf "%s" "$i"
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
    case $#:$7$6 in
        8:*|7:6*|6:6)
            case $1 in
                0)
                    case "$5" in
                        *"$2"*"$2"*) : ;;
                        "$2"*) return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    i="${5%"$2"*}"
                ;;
                *)
                    case $1"$5" in
                        1"$2"*) return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    x=0 && i="$5"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"

                        case "$i" in
                            *"$2"*) : ;;
                            *) [ $((x + 1)) -eq $1 ] || return 3 ;;
                        esac

                        x=$((x + 1))
                    done

                    case ":$i" in
                        :) i="${5%"$2"}" ;;
                        *) i="${5%"$2""$i"}" ;;
                    esac
                ;;
            esac

            case $3 in
                0)
                    case "$i" in
                        *"$4"*"$4"*) : ;;
                        *"$4") return 4 ;;
                        *"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    i="${i#*"$4"}"
                ;;
                *)
                    case $3"$i" in
                        1*"$4") return 4 ;;
                        $3*"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    x=0 && ii="$i"
                    until [ $x -eq $3 ]; do
                        i="${i%"$4"*}"

                        case "$i" in
                            *"$4"*) : ;;
                            *) [ $((x + 1)) -eq $3 ] || return 6 ;;
                        esac

                        x=$((x + 1))
                    done

                    case ":$i" in
                        :) i="${ii#"$4"}" ;;
                        *) i="${ii#"$i""$4"}" ;;
                    esac
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    i="${5%"$2"*}"
                ;;
                *)
                    x=0 && i="$5"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"
                        x=$((x + 1))
                    done
                    i="${5%"$2""$i"}"
                ;;
            esac

            [ "$i" = "$5" ] && return 2
            iii="$i"

            case $3 in
                0)
                    i="${i#*"$4"}"
                ;;
                *)
                    x=0 && ii="$i"
                    until [ $x -eq $3 ]; do
                        i="${i%"$4"*}"
                        x=$((x + 1))
                    done
                    i="${ii#"$i""$4"}"
                ;;
            esac

            [ "$i" = "$iii" ] && return 2
        ;;
    esac

    [ "$i" ] || return 1

    case $6 in
        0)
            i="${i#${i%%[![:space:]]*}}"
        ;;
        1)
            i="${i%${i##*[![:space:]]}}"
        ;;
        2)
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        ;;
    esac

    case $7$6 in
        *3*) i="$i$2" ;;
        *4*) i="$4$i" ;;
        *5*) i="$4$i$2" ;;
    esac

    printf "%s" "$i"
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
    case $#:$5$4 in
        6:*|5:4*|4:4)
            case $1 in
                0)
                    case "$3" in
                        *"$2") return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    i="${3##*"$2"}"
                ;;
                *)
                    case $1"$3" in
                        $1*"$2"*"$2"*) : ;;
                        1*"$2") return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    x=0 && i="$3"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"

                        case "$i" in
                            *"$2"*"$2"*) : ;;
                            *"$2") [ $((x + 2)) -eq $1 ] && return 1 ;;
                            *"$2"*) : ;;
                            *) [ $((x + 1)) -eq $1 ] || return 3 ;;
                        esac

                        x=$((x + 1))
                    done
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    i="${3##*"$2"}"
                ;;
                *)
                    x=0 && i="$3"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"
                        x=$((x + 1))
                    done
                ;;
            esac

            [ "$i" = "$3" ] && return 2
        ;;
    esac

    [ "$i" ] || return 1

    case $4 in
        0)
            i="${i#${i%%[![:space:]]*}}"
        ;;
        1)
            i="${i%${i##*[![:space:]]}}"
        ;;
        2)
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        ;;
    esac

    case $5$4 in
        *3*) i="$2$i" ;;
    esac

    printf "%s" "$i"
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
    case $#:$7$6 in
        8:*|7:6*|6:6)
            case $1 in
                0)
                    case "$5" in
                        *"$2") return 1 ;;
                        *"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    i="${5##*"$2"}"
                ;;
                *)
                    case $1"$5" in
                        $1*"$2"*"$2"*) : ;;
                        1*"$2") return 1 ;;
                        $1*"$2"*) : ;;
                        *) return 2 ;;
                    esac

                    x=0 && i="$5"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"

                        case "$i" in
                            *"$2"*"$2"*) : ;;
                            *"$2") [ $((x + 2)) -eq $1 ] && return 1 ;;
                            *"$2"*) : ;;
                            *) [ $((x + 1)) -eq $1 ] || return 3 ;;
                        esac

                        x=$((x + 1))
                    done
                ;;
            esac

            case $3 in
                0)
                    case "$i" in
                        *"$4"*"$4"*) : ;;
                        "$4"*) return 4 ;;
                        *"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    i="${i%"$4"*}"
                ;;
                *)
                    case $3"$i" in
                        1"$4"*) return 4 ;;
                        $3*"$4"*) : ;;
                        *) return 5 ;;
                    esac

                    x=0 && ii="$i"
                    until [ $x -eq $3 ]; do
                        i="${i#*"$4"}"

                        case "$i" in
                            *"$4"*) : ;;
                            *) [ $((x + 1)) -eq $3 ] || return 6 ;;
                        esac

                        x=$((x + 1))
                    done

                    case ":$i" in
                        :) i="${ii%"$4"}" ;;
                        *) i="${ii%"$4""$i"}" ;;
                    esac
                ;;
            esac
        ;;
        *)
            case $1 in
                0)
                    i="${5##*"$2"}"
                ;;
                *)
                    x=0 && i="$5"
                    until [ $x -eq $1 ]; do
                        i="${i#*"$2"}"
                        x=$((x + 1))
                    done
                ;;
            esac

            [ "$i" = "$5" ] && return 2
            iii="$i"

            case $3 in
                0)
                    i="${i%"$4"*}"
                ;;
                *)
                    x=0 && ii="$i"
                    until [ $x -eq $3 ]; do
                        i="${i#*"$4"}"
                        x=$((x + 1))
                    done
                    i="${ii%"$4""$i"}"
                ;;
            esac

            [ "$i" = "$iii" ] && return 2
        ;;
    esac

    [ "$i" ] || return 1

    case $6 in
        0)
            i="${i#${i%%[![:space:]]*}}"
        ;;
        1)
            i="${i%${i##*[![:space:]]}}"
        ;;
        2)
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        ;;
    esac

    case $7$6 in
        *3*) i="$2$i" ;;
        *4*) i="$i$4" ;;
        *5*) i="$2$i$4" ;;
    esac

    printf "%s" "$i"
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
            x=1; printf "%s" "$2"
            until [ $x -eq $1 ]; do
                x=$((x + 1))
                printf " %s" "$2"
            done
        ;;
        *)
            x=0
            until [ $x -eq $1 ]; do
                x=$((x + 1))
                printf "%s" "$2"
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
# [$3] - mode("5 N" - stop parsing further than specified "N" line)
#
# Returns:
# (0) output | empty output (file)
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

    case $#:"$2" in
        2:5*) x=0; i="${2#??}" ;;
        3:*1|3:*2|3:*3|3:*4) x=0; i="${3#??}" ;;
    esac

    case $#:"$3$2" in
        1:)
            while IFS= read -r LINE; do
                printf "%s\n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        2:1)
            while IFS= read -r LINE; do
                printf " %s \n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf " %s " "$LINE" ;;
            esac
        ;;
        2:2)
            while IFS= read -r LINE; do
                printf "  %s  \n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "  %s  " "$LINE" ;;
            esac
        ;;
        2:3)
            while IFS= read -r LINE; do
                [ -n "$LINE" ] && printf "%s\n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        2:4)
            while read -r LINE; do
                printf "%s\n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        2:5*)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf "%s\n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        3:*1)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf " %s \n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf " %s " "$LINE" ;;
            esac
        ;;
        3:*2)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf "  %s  \n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "  %s  " "$LINE" ;;
            esac
        ;;
        3:*3)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                [ -n "$LINE" ] && printf "%s\n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        3:*4)
            while read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf "%s\n" "$LINE"
            done < "$1"
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
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
# [$2] - mode("5 N" - stop parsing further than specified "N" line)
#
# Returns:
# (0) output | empty output (stdin)
#
# Caveats:
# 1. NULL character.
#
parse_fd1() {
    case $#:"$1" in
        1:5*) x=0; i="${1#??}" ;;
        2:*1|2:*2|2:*3|2:*4) x=0; i="${2#??}" ;;
    esac

    case $#:"$2$1" in
        0:)
            while IFS= read -r LINE; do
                printf "%s\n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        1:1)
            while IFS= read -r LINE; do
                printf " %s \n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf " %s " "$LINE" ;;
            esac
        ;;
        1:2)
            while IFS= read -r LINE; do
                printf "  %s  \n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "  %s  " "$LINE" ;;
            esac
        ;;
        1:3)
            while IFS= read -r LINE; do
                [ -n "$LINE" ] && printf "%s\n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        1:4)
            while read -r LINE; do
                printf "%s\n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        1:5*)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf "%s\n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        2:*1)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf " %s \n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf " %s " "$LINE" ;;
            esac
        ;;
        2:*2)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf "  %s  \n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "  %s  " "$LINE" ;;
            esac
        ;;
        2:*3)
            while IFS= read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                [ -n "$LINE" ] && printf "%s\n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
        ;;
        2:*4)
            while read -r LINE; do
                x=$((x + 1))
                [ $x -eq $i ] && break
                printf "%s\n" "$LINE"
            done
            case ":$LINE" in
                :) : ;;
                *) printf "%s" "$LINE" ;;
            esac
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
    x=0

    [ -f "$2" ] || return 2
    [ -r "$2" ] || return 3

    while read -r LINE; do
        x=$((x + 1))
        [ $x -eq $1 ] && printf "%s" "$LINE" && return 0
    done < "$2"
    [ $((x + 1)) -eq $1 ] && printf "%s" "$LINE" && return 0

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
    x=0

    while read -r LINE; do
        x=$((x + 1))
        [ $x -eq $1 ] && printf "%s" "$LINE" && return 0
    done
    [ $((x + 1)) -eq $1 ] && printf "%s" "$LINE" && return 0

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
    set -f

    i=$IFS

    IFS=$1
    set -- $2
    printf "%s" "$@"

    IFS=$i

    set +f
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
    case "$2" in
        "$1") return 1 ;;
        *"$1"*) : ;;
        *) return 2 ;;
    esac

    if [ $# -eq 4 ] && [ $4 -eq 3 ]; then
        case "$2" in
            " $1 ") return 1 ;;
        esac
    fi

    if [ $3 -eq 0 ] || [ $3 -eq 2 ]; then
        i="${2%%"$1"*}"
        if [ $# -eq 4 ] && [ $4 -eq 3 ]; then
            case "$2" in
                *" $1 "*) i="${i% }" ;;
                *"$1"*" $1") : ;;
                *" $1") i="${i% }" ;;
            esac
        fi

        ii="${2#*"$1"}"
        if [ $# -eq 4 ] && [ $4 -eq 3 ]; then
            case "$2" in "$1 "*) ii="${ii# }" ;; esac
        fi

        if [ -n "$i" ] && [ -n "$ii" ]; then
            i="$i$ii"
        elif [ -n "$ii" ]; then
            i="$ii"
        fi
    elif [ $3 -eq 1 ]; then
        i="${2%"$1"*}" && [ $# -eq 4 ] && [ $4 -eq 3 ] && i="${i% }"

        ii="${2##*"$1"}"
        if [ $# -eq 4 ] && [ $4 -eq 3 ]; then
            case "$2" in "$1 "*"$1"*) : ;; "$1 "*) ii="${ii# }" ;; esac
        fi

        if [ -n "$i" ] && [ -n "$ii" ]; then
            i="$i$ii"
        elif [ -n "$ii" ]; then
            i="$ii"
        fi
    fi

    if [ $3 -eq 2 ]; then
        while :; do case "$i" in
            "$1") return 1 ;;
            *"$1"*)
                if [ $# -eq 4 ] && [ $4 -eq 3 ]; then
                    case "$i" in
                        " $1 ") return 1 ;;
                    esac
                fi

                ii="${i%%"$1"*}"
                if [ $# -eq 4 ] && [ $4 -eq 3 ]; then
                    case "$i" in
                        *" $1 "*) ii="${ii% }" ;;
                        *"$1"*" $1") : ;;
                        *" $1") ii="${ii% }" ;;
                    esac
                fi

                iii="${i#*"$1"}"
                if [ $# -eq 4 ] && [ $4 -eq 3 ]; then
                    case "$i" in "$1 "*) iii="${iii# }" ;; esac
                fi

                if [ -n "$ii" ] && [ -n "$iii" ]; then
                    i="$ii$iii"
                elif [ -n "$ii" ]; then
                    i="$ii"
                elif [ -n "$iii" ]; then
                    i="$iii"
                fi
            ;;
            *) break ;;
        esac done
    fi

    [ "$i" ] || return 1

    printf "%s" "$i"
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
    case "$2" in
        "$1") return 1 ;;
        *"$1"*) : ;;
        *) return 2 ;;
    esac

    if [ "$1" = "$3" ]; then
        printf "%s" "$2" && return 0
    fi

    if [ $4 -eq 0 ]; then
        i="${2%%"$1"*}$3${2#*"$1"}"
    elif [ $4 -eq 1 ]; then
        i="${2%"$1"*}$3${2##*"$1"}"
    fi

    if [ $4 -eq 2 ]; then
        i="$2"
        ii="${i%%"$1"*}$3"
        i="${ii}${i#*"$1"}"

        while :; do case "$i" in
            "$ii"*"$1"*)
                iii="${i#*"$ii"}" && iii="${iii%%"$1"*}"
                ii="${ii}${iii}"
                iii="${i#*"$ii"}"
                i="${ii}$3${iii#*"$1"}"
                ii="${ii}$3"
            ;;
            *) break ;;
        esac done
    fi

    printf "%s" "$i"
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
#
rstrip() {
    case "$2" in
        *"$1") printf "%s" "${2%"$1"}" && return 0 ;;
    esac

    return 1
}
```

## safe_str

```sh
# Description:
# Escape all POSIX-defined shell meta characters in a string; characters are:
# \ | & ; < > ( ) $ ` " ' * ? [ ] # ~ = %
#
# Parameters:
# <"$1"> - string
# ["$2"] - mode("0 X" - escape only specified whitespace-separated "X"
#                       character(s))
# [$3] - mode('1' - strip the characters,
#             '2' - escape single quote character with itself)
#
# Returns:
# (0) escaped/stripped $1
# (1) no meta characters in $1
#
safe_str() {
    i="$1"
    unset ii

    set -f

    case $2 in
        0*) iiiii="${2#??}" ;;
        *) iiiii='\ | & ; < > ( ) $ ` " '\'' * ? [ ] # ~ = %' ;;
    esac

    case "$iiiii" in
        '\'*) : ;;
        *'\'*) iiiii="\\ ${iiiii%%\\*}${iiiii#*\\ }" ;;
    esac

    for iiii in $iiiii; do
        case "$i" in
            *"$iiii"*) : ;;
            *) continue ;;
        esac

        case $iiii:$#:$3:$2 in
            "'":3:2*|"'":2::2)
                ii="${i%%\'*}'\\''"
            ;;
            $iiii:3:1*|$iiii:2::1)
                ii="${i%%"$iiii"*}"
            ;;
            *)
                ii="${i%%"$iiii"*}\\$iiii"
            ;;
        esac
        i="${ii}${i#*"$iiii"}"

        case $iiii:$#:$3:$2 in
            "'":3:2*|"'":2::2)
                while :; do case "$i" in
                    "$ii"*"'"*)
                        iii="${i#*"$ii"}" && iii="${iii%%\'*}"
                        i="${ii}${iii}'\\''${i#*"${ii}${iii}"\'}"
                        ii="${ii}${iii}'\\''"
                    ;;
                    *)
                        break
                    ;;
                esac done
            ;;
            $iiii:3:1*|$iiii:2::1)
                while :; do case "$i" in
                    *"$iiii"*) i="${i%%"$iiii"*}${i#*"$iiii"}" ;;
                    *) break ;;
                esac done
            ;;
            *)
                while :; do case "$i" in
                    "$ii"*"$iiii"*)
                        iii="${i#*"$ii"}" && iii="${iii%%"$iiii"*}"
                        i="${ii}${iii}\\$iiii${i#*"${ii}${iii}$iiii"}"
                        ii="${ii}${iii}\\$iiii"
                    ;;
                    *)
                        break
                    ;;
                esac done
            ;;
        esac
    done

    set +f

    [ "$ii" ] || return 1

    printf "%s" "$i"
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
# (0) characters
# (1) no characters in $1
#
# Caveats:
# 1. A character is considered to be any character (except whitespace) in the
#    printable set of ASCII characters ('[:print:]'). This means that characters
#    outside this set will not be printed.
#    Exceptions: input with byte length of 1.
#    Fix: unknown; unknown.
#
str_to_chars() {
    iiiiii=$LC_CTYPE
    LC_CTYPE=C
    iii=1$2
    set -f
    ii=$IFS
    IFS=" "
    set -- $1
    i=$(printf "%s" "$@")
    IFS=$ii
    set +f

    case :${#i} in
        :0) return 1 ;;
        :1) printf "%s" "$i" && return 0 ;;
    esac

    if [ $iii -eq 10 ]; then
        i=$(
            ii="${i#?}"
            iii="${i%"$ii"}"
            iiii="$iii"
            case "$iii" in
                [[:print:]]) printf "%s" "$iii" && iiiii=1 ;;
                *) iiiii=0 ;;
            esac
            i="$ii"

            while [ -n "$i" ]; do
                ii="${i#?}"
                iii="${i%"$ii"}"
                case "$iii" in
                    [[:print:]])
                        case "$iiii" in
                            *"$iii"*) : ;;
                            *)
                                if [ $iiiii -eq 1 ]; then
                                    printf " %s" "$iii"
                                else
                                    printf "%s" "$iii"
                                    iiiii=1
                                fi
                            ;;
                        esac
                    ;;
                    *) : ;;
                esac
                iiii="$iiii$iii"
                i="$ii"
            done
        )
    else
        i=$(
            ii="${i#?}"
            iii="${i%"$ii"}"
            case "$iii" in
                [[:print:]]) printf "%s" "$iii" && iiii=1 ;;
                *) iiii=0 ;;
            esac
            i="$ii"

            while [ -n "$i" ]; do
                ii="${i#?}"
                iii="${i%"$ii"}"
                case "$iii" in
                    [[:print:]])
                        if [ $iiii -eq 1 ]; then
                            printf " %s" "$iii"
                        else
                            printf "%s" "$iii"
                            iiii=1
                        fi
                    ;;
                    *) : ;;
                esac
                i="$ii"
            done
        )
    fi

    LC_CTYPE=$iiiiii

    [ "$i" ] || return 1

    printf "%s" "$i"
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

### warn_px

```sh
# Description:
# Print a warning with printf prefix before text
#
# Parameters:
# <"$1"> - printf prefix
# <"$2"+> - text
#
# Returns:
# (0) text
#
warn_px() {
    i="$1" && shift
    printf "WARNING: $i%s\n" "$*"
}
```

### warn_px_clr

```sh
# Description:
# Print a colorful warning with printf prefix before text
#
# Parameters:
# <"$1"> - printf prefix
# <"$2"+> - text
#
# Returns:
# (0) text
#
warn_px_clr() {
    i="$1" && shift
    printf "%bWARNING:%b $i%s\n" "\033[1;33m" "\033[0m" "$*"
}
```
