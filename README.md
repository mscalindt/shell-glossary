* [ERR()](https://github.com/mscalindt/shell-glossary#err)

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
