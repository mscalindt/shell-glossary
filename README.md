# Normal functions:

* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount)
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath)
* [GREP_STR()](https://github.com/mscalindt/shell-glossary#grep_str)
* [INFO()](https://github.com/mscalindt/shell-glossary#info)
* [LTL_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltl_substr0)
* [LTL_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltl_substr1)
* [LTR_SUBSTR0()](https://github.com/mscalindt/shell-glossary#ltr_substr0)
* [LTR_SUBSTR1()](https://github.com/mscalindt/shell-glossary#ltr_substr1)
* [PARSE()](https://github.com/mscalindt/shell-glossary#parse)
* [PLINE()](https://github.com/mscalindt/shell-glossary#pline)
* [REMCHARS()](https://github.com/mscalindt/shell-glossary#remchars)
* [REMSTR()](https://github.com/mscalindt/shell-glossary#remstr)
* [WARN()](https://github.com/mscalindt/shell-glossary#warn)


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
# (2) not a directory / does not exist
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
                set -- "$i"/*/ && i=$# && ii=$((ii - i))
            ;;
            1*)
                ii="${2#1}"
                set -- "$i"/*"$ii" && iii="$1" && iiii=$#
                set -- "$i"/*"$ii"/ && i=$# && ii=$((iiii - i))
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
# (0) absolute path
# (1) path is already absolute
#
get_fpath() {
    case "$1" in "/"*) printf "%s" "$1" && return 1 ;; esac
    printf "%s/%s" "$PWD" "$1" && return 0
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
# (1) no substring
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
# (1) no substring
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
remchars() {
    set -f

    i=$IFS

    IFS=$1
    set -- $2

    IFS=
    set -- $*

    printf "%s" "$*"

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

    printf "%s" "$i"
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
