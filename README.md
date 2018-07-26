# HowTo
A collection of solutions to various tasks, that I keep forgetting how to solve

## Markdown
[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

## Bash
Counting the number of lines in a file:
```bash
wc -l < FILENAME
```

[Echo](https://linux.die.net/man/1/echo) using color [List of formats](https://misc.flogisoft.com/bash/tip_colors_and_formatting):
```bash
# The -e argument to echo enables interpretation of backslash escapes
# \e[FORMATm turns on a given format. The code 0 (zero) clears all formatting
echo -e "The following is \[e[31mred\e[0m, and this is back to default"
```

## Apache
Count requests in Apache access log, grouped by IP version (IPv4 vs IPv6)
```bash
cat access.log | cut -d' ' -f1 | sed 's/.*\..*/IPv4/' | sed 's/.*:.*/IPv6/' | sort | uniq -c
#OR use zless, to include all rotated files
zless access.log access.log.* | cut -d' ' -f1 | sed 's/.*\..*/IPv4/' | sed 's/.*:.*/IPv6/' | sort | uniq -c

#Sample output:
52750 IPv4
 7005 IPv6
```
