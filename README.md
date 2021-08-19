# Normal functions:

* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount)
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath)
* [GREP_STR()](https://github.com/mscalindt/shell-glossary#grep_str)

# Stdin functions:

* [GREP_STR_FD1()](https://github.com/mscalindt/shell-glossary#grep_str_fd1)

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
