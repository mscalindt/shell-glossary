* [ERR()](https://github.com/mscalindt/shell-glossary#err)
* [FCOUNT()](https://github.com/mscalindt/shell-glossary#fcount)
* [GET_FPATH()](https://github.com/mscalindt/shell-glossary#get_fpath)

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
