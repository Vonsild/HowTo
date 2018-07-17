# HowTo
A collection of solutions to various tasks, that I keep forgetting how to solve

## Bash
Counting the number of lines in a file:

    wc -l < FILENAME

[Echo](https://linux.die.net/man/1/echo) using color [List of formats](https://misc.flogisoft.com/bash/tip_colors_and_formatting):

    # The -e argument to echo enables interpretation of backslash escapes
    # \e[FORMATm turns on a given format. The code 0 (zero) clears all formatting
    echo -e "The following is \[e[31mred\e[0m, and this is back to default"
