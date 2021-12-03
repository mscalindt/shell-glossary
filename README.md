A collection of reusable pure POSIX `sh` functions with no external binary calls (on somewhat modern shells where `printf` and `read` are builtins). 

# Normal functions:

* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [ERR_PX()](https://github.com/mscalindt/shell-glossary#err_px)
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount)
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/6
* [GREP_STR()](https://github.com/mscalindt/shell-glossary#grep_str) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/7.1
* [INFO()](https://github.com/mscalindt/shell-glossary#info)
* [INFO_PX()](https://github.com/mscalindt/shell-glossary#info_px)
* [LSTRIP()](https://github.com/mscalindt/shell-glossary#lstrip) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/8
* [LTL_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltl_substr0) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/1.3
* [LTL_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltl_substr1) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/1.3
* [LTR_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltr_substr0) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/1.3
* [LTR_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltr_substr1) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/1.3
* [PARSE()](https://github.com/mscalindt/shell-glossary#parse)
* [PLINE()](https://github.com/mscalindt/shell-glossary#pline)
* [REMCHARS()](https://github.com/mscalindt/shell-glossary#remchars) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/5.1
* [REMSTR()](https://github.com/mscalindt/shell-glossary#remstr) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/3.3
* [REPLSTR()](https://github.com/mscalindt/shell-glossary#replstr) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/4.1
* [RSTRIP()](https://github.com/mscalindt/shell-glossary#rstrip) | Unit tests: https://raw.githubusercontent.com/mscalindt/top-secret/root/2/9
* [SAFE_STR()](https://github.com/mscalindt/shell-glossary#safe_str)
* [STR_TO_CHARS()](https://github.com/mscalindt/shell-glossary#str_to_chars)
* [WARN()](https://github.com/mscalindt/shell-glossary#warn)
* [WARN_PX()](https://github.com/mscalindt/shell-glossary#warn_px)

# Stdin functions:

* [GREP_STR_FD1()](https://github.com/mscalindt/shell-glossary#grep_str_fd1)
* [PARSE_FD1()](https://github.com/mscalindt/shell-glossary#parse_fd1)
* [PLINE_FD1()](https://github.com/mscalindt/shell-glossary#pline_fd1)

## err

```sh
# Description:
# Print error and exit
#
# Parameters:
# <$1> - exit code
# <"$2+"> - text
#
err() {
    i=$1 && shift
    printf "\n%bERROR:%b %s\n\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit $i
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
# <"$3+"> - text
#
err_px() {
    i=$1 && ii="$2"; shift 2
    printf "\n%bERROR:%b $ii%s\n\n" "\033[1;31m" "\033[0m" "$*" 1>&2
    exit $i
}
```

## fcount

```sh
# Description:
# Count files and directories in a directory
#
# Parameters:
# <"$1"> - directory
# ['$2'] - mode('0X' - count files and directories ending with "X",
#               '1' - count only files,
#               '1X' - count only files ending with "X",
#               '2' - count only directories,
#               '2X' - count only directories ending with "X")
#
# Returns:
# (0) count
# (1) directory is empty
# (2) not a directory | directory does not exist
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
    [ -d "$1" ] && i="$1" || return 2

    if [ $# -eq 2 ]; then
        case "$2" in
            0*)
                ii="${2#0}"
                set -- "$i"/*"$ii" && iii="$1" && ii=$#
            ;;
            1)
                set -- "$i"/* && iii="$1" && ii=$#
                set -- "$i"/*/
                [ -e "$1" ] && i=$# && ii=$((ii - i))
            ;;
            1*)
                ii="${2#1}"
                set -- "$i"/*"$ii" && iii="$1" && iiii=$#
                set -- "$i"/*"$ii"/
                [ -e "$1" ] && i=$# && ii=$((iiii - i))
            ;;
            2)
                set -- "$i"/*/ && iii="$1" && ii=$#
            ;;
            2*)
                ii="${2#2}"
                set -- "$i"/*"$ii"/ && iii="$1" && ii=$#
            ;;
        esac
    else
        set -- "$i"/* && iii="$1" && ii=$#
    fi

    [ -e "$iii" ] && printf "%d" "$ii" && return 0
    return 1
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
# (0) absolute path / path is already absolute
#
get_fpath() {
    case "$1" in
        "/"*) printf "%s" "$1" ;;
        *) printf "%s/%s" "$PWD" "$1" ;;
    esac

    return 0
}
```

## grep_str

```sh
# Description:
# Check the existence/position of a substring in string
#
# Parameters:
# <'$1'> - substring
# <"$2"> - string
# [$3] - mode('1' - $1 is first character(s) of $2,
#             '2' - $1 is last character(s) of $2,
#             '3' - $1 is, on its own, $2)
#
# Returns:
# (0) substring exists
# (1) no match
#
grep_str() {
    if [ $# -eq 3 ]; then
        if [ $3 -eq 1 ]; then
            case "$2" in "$1"*) grep_str=0 && return 0 ;; esac
        elif [ $3 -eq 2 ]; then
            case "$2" in *"$1") grep_str=0 && return 0 ;; esac
        elif [ $3 -eq 3 ]; then
            case "$2" in "$1") grep_str=0 && return 0 ;; esac
        fi
    else
        case "$2" in *"$1"*) grep_str=0 && return 0 ;; esac
    fi

    return 1
}
```

## grep_str_fd1

```sh
# Description:
# Check the existence/position of a substring in stdin
#
# Parameters:
# <'$1'> - substring
# [$2] - mode('1' - $1 is first character(s) of stdin,
#             '2' - $1 is last character(s) of stdin,
#             '3' - $1 is, on its own, stdin)
#
# Returns:
# (0) substring exists
# (1) no match
#
grep_str_fd1() {
    if [ $# -eq 2 ]; then
        if [ $2 -eq 1 ]; then
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                case "$LINE" in "$1"*) return 0 ;; esac
            done
        elif [ $2 -eq 2 ]; then
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                case "$LINE" in *"$1") return 0 ;; esac
            done
        elif [ $2 -eq 3 ]; then
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                case "$LINE" in "$1") return 0 ;; esac
            done
        fi
    else
        while IFS= read -r LINE || [ -n "$LINE" ]; do
            case "$LINE" in *"$1"*) return 0 ;; esac
        done
    fi

    return 1
}
```

## info

```sh
# Description:
# Print info
#
# Parameters:
# <"$1+"> - text
#
info() {
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
# <"$2+"> - text
#
info_px() {
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
# <'$1'> - character(s)
# <"$2"> - string
#
# Returns:
# (0) stripped string
# (1) no $1 at the start of $2
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
# <'$2'> - from "X" character(s)
# <"$3"> - string
# [$4] - mode('0' - strip all leading whitespace characters,
#             '1' - strip all trailing whitespace characters,
#             '2' - strip all leading/trailing whitespace characters)
# [$5] - mode('3' - keep $2)
# [$6] - mode('4' - verify the expansion)
#
# Returns:
# (0) substring
# (1) empty expansion (empty, or unspecified, or wrong -- shell specific)
# (2) unspecified expansion (unspecified, or empty, or wrong -- shell specific)
#
# Returns (mode '4'):
# (0) substring
# (1) empty expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion ($2 is not present at all)
# (3) wrong expansion ($1 is greater than the total $2 in the string)
#
# Caveats:
# 1. If mode 4 is NOT passed, you should NOT assume the expansion is
#    respectively correct, even if the return code is 0. This is because every
#    shell has its own unique string expander and unspecified results will vary
#    between them; for example, if $2 is not present anymore or at all, one
#    shell might return the same expansion, while another might return empty
#    expansion. Also, the string expander on its own does NOT guarantee correct
#    expansion at all.
#    Fix: Use mode '4'. When mode 4 is passed, the function will do various
#    checks on each expansion to accurately determine what the expansion will
#    result into; this guarantees respectively correct expansion on any shell.
#    Performance cost should be negligible on most shells.
#
ltl_substr0() {
    if [ $# -eq 6 ] || \
       [ $# -ge 5 ] && [ $5 -eq 4 ] || \
       [ $# -ge 4 ] && [ $4 -eq 4 ]; then
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
                        *) [ $((x + 1)) -ne $1 ] && return 3 ;;
                    esac

                    x=$((x + 1))
                done
                [ -z "$i" ] && i="${3%"$2"}" || i="${3%"$2""$i"}"
            ;;
        esac
    else
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
    fi

    if [ -n "$i" ]; then
        if [ $# -ge 4 ] && [ $4 -eq 0 ]; then
            i="${i#${i%%[![:space:]]*}}"
        elif [ $# -ge 4 ] && [ $4 -eq 1 ]; then
            i="${i%${i##*[![:space:]]}}"
        elif [ $# -ge 4 ] && [ $4 -eq 2 ]; then
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        fi

        if [ $# -ge 5 ] && [ $5 -eq 3 ] || \
           [ $# -ge 4 ] && [ $4 -eq 3 ]; then
            i="$i$2"
        fi

        printf "%s" "$i" && return 0
    fi

    return 1
}
```

## ltl_substr1

```sh
# Description:
# Get positional substring, (from&to LTL) (N)character(s), in a string
#
# Parameters:
# <$1> - from "N" LTR character('0' - max)
# <'$2'> - from "X" character(s)
# <$3> - to "N" $2-TL character('0' - max)
# <'$4'> - to "X" character(s)
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
# (0) substring
# (1) empty expansion (empty, or unspecified, or wrong -- shell specific)
# (2) unspecified expansion (unspecified, or empty, or wrong -- shell specific)
#
# Returns (mode '6'):
# (0) substring
# (1) unspecified expansion ($2 is the first character by the given ruleset)
# (2) unspecified expansion ($2 is not present at all)
# (3) wrong expansion ($1 is greater than the total $2 in the string)
# (4) empty expansion (empty expansion by the given ruleset)
# (5) unspecified expansion ($4 is not present at all)
# (6) wrong expansion ($3 is greater than the total $4 in the string)
#
# Caveats:
# 1. If mode 6 is NOT passed, you should NOT assume the expansion is
#    respectively correct, even if the return code is 0. This is because every
#    shell has its own unique string expander and unspecified results will vary
#    between them; for example, if $2/$4 is not present anymore or at all, one
#    shell might return the same expansion, while another might return empty
#    expansion. Also, the string expander on its own does NOT guarantee correct
#    expansion at all.
#    Fix: Use mode '6'. When mode 6 is passed, the function will do various
#    checks on each expansion to accurately determine what the expansion will
#    result into; this guarantees respectively correct expansion on any shell.
#    Performance cost should be negligible on most shells.
#
ltl_substr1() {
    if [ $# -eq 8 ] || \
       [ $# -ge 7 ] && [ $7 -eq 6 ] || \
       [ $# -ge 6 ] && [ $6 -eq 6 ]; then
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
                        *) [ $((x + 1)) -ne $1 ] && return 3 ;;
                    esac

                    x=$((x + 1))
                done
                [ -z "$i" ] && i="${5%"$2"}" || i="${5%"$2""$i"}"
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
                        *) [ $((x + 1)) -ne $3 ] && return 6 ;;
                    esac

                    x=$((x + 1))
                done
                [ -z "$i" ] && i="${ii#"$4"}" || i="${ii#"$i""$4"}"
            ;;
        esac
    else
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

        [ "$i" = "$5" ] && return 2
    fi

    if [ -n "$i" ]; then
        if [ $# -ge 6 ] && [ $6 -eq 0 ]; then
            i="${i#${i%%[![:space:]]*}}"
        elif [ $# -ge 6 ] && [ $6 -eq 1 ]; then
            i="${i%${i##*[![:space:]]}}"
        elif [ $# -ge 6 ] && [ $6 -eq 2 ]; then
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        fi

        if [ $# -ge 7 ] && [ $7 -eq 3 ] || \
           [ $# -ge 6 ] && [ $6 -eq 3 ]; then
            i="$i$2"
        elif [ $# -ge 7 ] && [ $7 -eq 4 ] || \
             [ $# -ge 6 ] && [ $6 -eq 4 ]; then
            i="$4$i"
        elif [ $# -ge 7 ] && [ $7 -eq 5 ] || \
             [ $# -ge 6 ] && [ $6 -eq 5 ]; then
            i="$4$i$2"
        fi

        printf "%s" "$i" && return 0
    fi

    return 1
}
```

## ltr_substr0

```sh
# Description:
# Get positional substring, (from LTR) (N)character(s), in a string
#
# Parameters:
# <$1> - from "N" LTR character('0' - max)
# <'$2'> - from "X" character(s)
# <"$3"> - string
# [$4] - mode('0' - strip all leading whitespace characters,
#             '1' - strip all trailing whitespace characters,
#             '2' - strip all leading/trailing whitespace characters)
# [$5] - mode('3' - keep $2)
# [$6] - mode('4' - verify the expansion)
#
# Returns:
# (0) substring
# (1) empty expansion (empty, or unspecified, or wrong -- shell specific)
# (2) unspecified expansion (unspecified, or empty, or wrong -- shell specific)
#
# Returns (mode '4'):
# (0) substring
# (1) empty expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion ($2 is not present at all)
# (3) wrong expansion ($1 is greater than the total $2 in the string)
#
# Caveats:
# 1. If mode 4 is NOT passed, you should NOT assume the expansion is
#    respectively correct, even if the return code is 0. This is because every
#    shell has its own unique string expander and unspecified results will vary
#    between them; for example, if $2 is not present anymore or at all, one
#    shell might return the same expansion, while another might return empty
#    expansion. Also, the string expander on its own does NOT guarantee correct
#    expansion at all.
#    Fix: Use mode '4'. When mode 4 is passed, the function will do various
#    checks on each expansion to accurately determine what the expansion will
#    result into; this guarantees respectively correct expansion on any shell.
#    Performance cost should be negligible on most shells.
#
ltr_substr0() {
    if [ $# -eq 6 ] || \
       [ $# -ge 5 ] && [ $5 -eq 4 ] || \
       [ $# -ge 4 ] && [ $4 -eq 4 ]; then
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
                        *) [ $((x + 1)) -ne $1 ] && return 3 ;;
                    esac

                    x=$((x + 1))
                done
            ;;
        esac
    else
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
    fi

    if [ -n "$i" ]; then
        if [ $# -ge 4 ] && [ $4 -eq 0 ]; then
            i="${i#${i%%[![:space:]]*}}"
        elif [ $# -ge 4 ] && [ $4 -eq 1 ]; then
            i="${i%${i##*[![:space:]]}}"
        elif [ $# -ge 4 ] && [ $4 -eq 2 ]; then
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        fi

        if [ $# -ge 5 ] && [ $5 -eq 3 ] || \
           [ $# -ge 4 ] && [ $4 -eq 3 ]; then
            i="$2$i"
        fi

        printf "%s" "$i" && return 0
    fi

    return 1
}
```

## ltr_substr1

```sh
# Description:
# Get positional substring, (from&to LTR) (N)character(s), in a string
#
# Parameters:
# <$1> - from "N" LTR character('0' - max)
# <'$2'> - from "X" character(s)
# <$3> - to "N" $2-TR character('0' - max)
# <'$4'> - to "X" character(s)
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
# (0) substring
# (1) empty expansion (empty, or unspecified, or wrong -- shell specific)
# (2) unspecified expansion (unspecified, or empty, or wrong -- shell specific)
#
# Returns (mode '6'):
# (0) substring
# (1) unspecified expansion ($2 is the last character by the given ruleset)
# (2) unspecified expansion ($2 is not present at all)
# (3) wrong expansion ($1 is greater than the total $2 in the string)
# (4) empty expansion (empty expansion by the given ruleset)
# (5) unspecified expansion ($4 is not present at all)
# (6) wrong expansion ($3 is greater than the total $4 in the string)
#
# Caveats:
# 1. If mode 6 is NOT passed, you should NOT assume the expansion is
#    respectively correct, even if the return code is 0. This is because every
#    shell has its own unique string expander and unspecified results will vary
#    between them; for example, if $2/$4 is not present anymore or at all, one
#    shell might return the same expansion, while another might return empty
#    expansion. Also, the string expander on its own does NOT guarantee correct
#    expansion at all.
#    Fix: Use mode '6'. When mode 6 is passed, the function will do various
#    checks on each expansion to accurately determine what the expansion will
#    result into; this guarantees respectively correct expansion on any shell.
#    Performance cost should be negligible on most shells.
#
ltr_substr1() {
    if [ $# -eq 8 ] || \
       [ $# -ge 7 ] && [ $7 -eq 6 ] || \
       [ $# -ge 6 ] && [ $6 -eq 6 ]; then
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
                        *) [ $((x + 1)) -ne $1 ] && return 3 ;;
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
                        *) [ $((x + 1)) -ne $3 ] && return 6 ;;
                    esac

                    x=$((x + 1))
                done
                [ -z "$i" ] && i="${ii%"$4"}" || i="${ii%"$4""$i"}"
            ;;
        esac
    else
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

        [ "$i" = "$5" ] && return 2
    fi

    if [ -n "$i" ]; then
        if [ $# -ge 6 ] && [ $6 -eq 0 ]; then
            i="${i#${i%%[![:space:]]*}}"
        elif [ $# -ge 6 ] && [ $6 -eq 1 ]; then
            i="${i%${i##*[![:space:]]}}"
        elif [ $# -ge 6 ] && [ $6 -eq 2 ]; then
            i="${i#${i%%[![:space:]]*}}"
            i="${i%${i##*[![:space:]]}}"
        fi

        if [ $# -ge 7 ] && [ $7 -eq 3 ] || \
           [ $# -ge 6 ] && [ $6 -eq 3 ]; then
            i="$2$i"
        elif [ $# -ge 7 ] && [ $7 -eq 4 ] || \
             [ $# -ge 6 ] && [ $6 -eq 4 ]; then
            i="$i$4"
        elif [ $# -ge 7 ] && [ $7 -eq 5 ] || \
             [ $# -ge 6 ] && [ $6 -eq 5 ]; then
            i="$2$i$4"
        fi

        printf "%s" "$i" && return 0
    fi

    return 1
}
```

## parse

```sh
# Description:
# Parse the content of file
#
# Parameters:
# <$1> - mode('0' - one-to-one copy,
#             '1' - wrap parsed lines in one leading/trailing whitespace char,
#             '2' - wrap parsed lines in two leading/trailing whitespace chars,
#             '3' - skip empty lines,
#             '4' - strip trailing/leading whitespace chars)
# <"$2"> - file
#
# Returns:
# (0) output | empty output (file)
# (1) file does not exist | file permission error
#
parse() {
    case $1 in
        0)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                printf "%s\n" "$LINE"
            done < "$2"
        ;;
        1)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                printf " %s \n" "$LINE"
            done < "$2"
        ;;
        2)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                printf "  %s  \n" "$LINE"
            done < "$2"
        ;;
        3)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                [ -n "$LINE" ] && printf "%s\n" "$LINE"
            done < "$2"
        ;;
        4)
            while read -r LINE || [ -n "$LINE" ]; do
                printf "%s\n" "$LINE"
            done < "$2"
        ;;
    esac

    return $?
}
```

## parse_fd1

```sh
# Description:
# Parse the content of stdin
#
# Parameters:
# <$1> - mode('0' - one-to-one copy,
#             '1' - wrap parsed lines in one leading/trailing whitespace char,
#             '2' - wrap parsed lines in two leading/trailing whitespace chars,
#             '3' - skip empty lines,
#             '4' - strip trailing/leading whitespace chars)
#
# Returns:
# (0) output | empty output (stdin)
#
parse_fd1() {
    case $1 in
        0)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                printf "%s\n" "$LINE"
            done
        ;;
        1)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                printf " %s \n" "$LINE"
            done
        ;;
        2)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                printf "  %s  \n" "$LINE"
            done
        ;;
        3)
            while IFS= read -r LINE || [ -n "$LINE" ]; do
                [ -n "$LINE" ] && printf "%s\n" "$LINE"
            done
        ;;
        4)
            while read -r LINE || [ -n "$LINE" ]; do
                printf "%s\n" "$LINE"
            done
        ;;
    esac

    return 0
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
# (1) line does not exist | file permission error
#
pline() {
    x=0

    while read -r LINE || [ -n "$LINE" ]; do
        x=$((x + 1))
        [ $x -eq $1 ] && printf "%s" "$LINE" && return 0
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
pline_fd1() {
    x=0

    while read -r LINE || [ -n "$LINE" ]; do
        x=$((x + 1))
        [ $x -eq $1 ] && printf "%s" "$LINE" && return 0
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
# <'$1'> - character(s)
# <"$2"> - string
#
# Returns:
# $2 without $1 characters,
# $2
#
# Caveats:
# 1. Multibyte characters are not suitable as a delimiter ($1), as the removal
#    of the characters is done by IFS splitting; any characters in a multibyte
#    character will be stripped from the string. Essentially, this means that,
#    for example, a multibyte character consisting of a whitespace character
#    will remove all the whitespace characters in the string ($2).
#    Fix: none; shell limitation.
# 2. If the delimiter ($1) contains <whitespace> (' ') and the string ($2)
#    contains multibyte characters consisting of a whitespace character, the
#    multibyte characters will be rendered incorrect, resulting in a bad string.
#    Fix: none; shell limitation.
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
# <'$1'> - substring
# <"$2"> - string
# <$3> - mode('0' - remove the first occurrence,
#             '1' - remove the last occurrence,
#             '2' - remove all occurrences)
# [$4] - mode('3' - whitespace is delimiter)
#
# Returns:
# (0) stripped string
# (1) $1 is, on its own, $2
# (2) $1 not present in $2
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

    [ -n "$i" ] && printf "%s" "$i" || return 1
}
```

## replstr

```sh
# Description:
# Replace a substring of a string with character(s)
#
# Parameters:
# <'$1'> - substring
# <"$2"> - string
# <'$3'> - character(s)
# <$4> - mode('0' - replace the first occurrence,
#             '1' - replace the last occurrence,
#             '2' - replace all occurrences)
#
# Returns:
# (0) replaced string
# (1) $1 is, on its own, $2
# (2) $1 not present in $2
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
# <'$1'> - character(s)
# <"$2"> - string
#
# Returns:
# (0) stripped string
# (1) no $1 at the end of $2
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
# <'$1'> - string
# ['$2'] - mode('0X' - escape only specified "X" character(s))
# [$3] - mode('1' - strip the characters)
#
# Returns:
# (0) escaped/stripped string
# (1) no meta characters in $1
#
# Caveats:
# 1. Only printable ASCII characters shall be specified and escaped.
#
safe_str() {
    i="$1"
    ii="${2#?}" && ii="${2%"$ii"}"
    unset iiii

    set -f

    if [ $# -ge 2 ] && [ $ii -eq 0 ]; then
        iii="${2#?}"
        if [ ${#iii} -ne 1 ]; then
            iii=$(
                LC_CTYPE=C; IFS=" "; set -- $iii; i=$(printf "%s" "$@")

                i=$(ii="${i#?}"; iii="${i%"$ii"}"; iiii="$iii"
                case "$iii" in
                [[:print:]]) printf "%s" "$iii" && iiiii=1 ;;
                *) iiiii=0 ;;
                esac
                i="$ii"
                while [ -n "$i" ]; do
                ii="${i#?}"; iii="${i%"$ii"}"
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
                iiii="$iiii$iii"; i="$ii"
                done
                )

                printf "%s" "$i"
            )
        fi
    else
        iii='\ | & ; < > ( ) $ ` " '\'' * ? [ ] # ~ = %'
    fi

    case "$iii" in
    *'\'*)
        ii='\'

        case "$i" in
            *"$ii"*) : ;;
            *) continue ;;
        esac

        if [ $# -ge 2 ] && [ $2 = 1 ] || \
           [ $# -eq 3 ]; then
            iiii="${i%%"$ii"*}"
            i="${iiii}${i#*"$ii"}"
        else
            iiii="${i%%"$ii"*}\\$ii"
            i="${iiii}${i#*"$ii"}"
        fi

        if [ $# -ge 2 ] && [ $2 = 1 ] || \
           [ $# -eq 3 ]; then
            while :; do case "$i" in
                "$iiii"*"$ii"*)
                    iiiii="${i#*"$iiii"}" && iiiii="${iiiii%%"$ii"*}"
                    iiii="${iiii}${iiiii}"
                    iiiii="${i#*"$iiii"}"
                    i="${iiii}${iiiii#*"$ii"}"
                ;;
                *) break ;;
            esac done
        else
            while :; do case "$i" in
                "$iiii"*"$ii"*)
                    iiiii="${i#*"$iiii"}" && iiiii="${iiiii%%"$ii"*}"
                    iiii="${iiii}${iiiii}"
                    iiiii="${i#*"$iiii"}"
                    i="${iiii}\\$ii${iiiii#*"$ii"}"
                    iiii="${iiii}\\$ii"
                ;;
                *) break ;;
            esac done
        fi
    ;;
    esac

    for ii in $iii; do
        case "$ii" in
            *'\'*) continue ;;
        esac

        case "$i" in
            *"$ii"*) : ;;
            *) continue ;;
        esac

        if [ $# -ge 2 ] && [ $2 = 1 ] || \
           [ $# -eq 3 ]; then
            iiii="${i%%"$ii"*}"
            i="${iiii}${i#*"$ii"}"
        else
            iiii="${i%%"$ii"*}\\$ii"
            i="${iiii}${i#*"$ii"}"
        fi

        if [ $# -ge 2 ] && [ $2 = 1 ] || \
           [ $# -eq 3 ]; then
            while :; do case "$i" in
                "$iiii"*"$ii"*)
                    iiiii="${i#*"$iiii"}" && iiiii="${iiiii%%"$ii"*}"
                    iiii="${iiii}${iiiii}"
                    iiiii="${i#*"$iiii"}"
                    i="${iiii}${iiiii#*"$ii"}"
                ;;
                *) break ;;
            esac done
        else
            while :; do case "$i" in
                "$iiii"*"$ii"*)
                    iiiii="${i#*"$iiii"}" && iiiii="${iiiii%%"$ii"*}"
                    iiii="${iiii}${iiiii}"
                    iiiii="${i#*"$iiii"}"
                    i="${iiii}\\$ii${iiiii#*"$ii"}"
                    iiii="${iiii}\\$ii"
                ;;
                *) break ;;
            esac done
        fi
    done

    set +f

    printf "%s" "$i"
    [ -n "$iiii" ] && return 1 || return 0
}
```

## str_to_chars

```sh
# Description:
# Convert a string to whitespace-separated characters
#
# Parameters:
# <'$1'> - string
# [$2] - mode('0' - remove duplicate characters)
#
# Returns:
# (0) characters
# (1) $1 is, on its own, a character
# (2) no characters in $1
#
# Caveats:
# 1. A character is considered to be any character (except whitespace) in the
#    printable set of ASCII characters ('[:print:]'). This means that characters
#    outside this set will not be printed. Multibyte characters consisting of
#    characters in the printable set will only have those characters printed.
#    Fix: none; shell limitation.
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

    [ -z "$i" ] && return 2
    [ "${#i}" -eq 1 ] && return 1

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

    printf "%s" "$i" && return 0
}
```

## warn

```sh
# Description:
# Print a warning
#
# Parameters:
# <"$1+"> - text
#
warn() {
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
# <"$2+"> - text
#
warn_px() {
    i="$1" && shift
    printf "%bWARNING:%b $i%s\n" "\033[1;33m" "\033[0m" "$*"
}
```
