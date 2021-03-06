# HowTo
A collection of solutions to various tasks, that I keep forgetting how to solve

## Markdown
[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

## Bash
Counting the number of lines in a file:
```bash
wc -l < FILENAME
```

Counting unique lines
```bash
sort FILENAME | uniq -c
```

Sorting lines in a file
```bash
sort FILENAME
```

Sorting by 3rd column
```bash
sort -k 3 FILENAME
```

Count unique errors in error.log (start by cutting out the \[something\] groups at the start of the line)
```bash
sed -E 's/^(\[[^]]+\] )+//' -- FILENAME | sort | uniq -cw 100 | wc -l
```

Fast listing of files in a large directory (fast because it doesn't sort the output)
```bash
ls -f
```

List only files in a directory (-d appends / to directories, `grep -v /` matches only lines without slashes)
```bash
ls -d | grep -v /
```

List only directories in a directory (-d appends / to directories, `grep /` matches only lines without slashes)
```bash
ls -d | grep /
```

Move files like aacd35fe02... into aa/aacd35fe02... subdirectories. Helpful when a small adhoc project grows too much, and you end up with a million images in a single directory. This version only works for files with hexvalues for names.
```bash
ls -fp | grep -v / | grep -E '^[0-9a-f][0-9a-f]' | sed 's/^\(\(..\)..*\)$/mv \1 \2\/\1/' | bash
```

### Echo

[Echo](https://linux.die.net/man/1/echo) using color [List of formats](https://misc.flogisoft.com/bash/tip_colors_and_formatting):
```bash
# The -e argument to echo enables interpretation of backslash escapes
# \e[FORMATm turns on a given format. The code 0 (zero) clears all formatting
echo -e "The following is \[e[31mred\e[0m, and this is back to default"
```

Echo without trailing newline:
```bash
echo -n "This is line one."; echo " This is also line one."; echo "This is line two."
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
### Slooow SSL
When it takes a loooong time to establish SSL-connections, go to `/etc/apache2/mods-enabled/mpm_prefork.conf` (or whatever mpm version in use) and increase `MaxRequestWorkers`. If a number higher than 256 is needed, set `ServerLimit` to the same value as `MaxRequestWorkers`, or the limit will be lowered to 256!

## Apt-get and related
Apt-get reads the `/etc/apt/sources.list` - remember to run `sudo apt-get update` after updating this file.

To install .deb file from disk: `apt install ./<FILENAME>`

To view the description of the .deb file: `dpkg-deb -I ./<FILENAME>`  
To view ONLY the 'Description' field: `dpkg -f general-setup.deb Description`

## Passwords
To do default Debian hashing of passwords, to be used in e.g. the `useradd` command:  
`openssl passwd -crypt` - hit enter, and type the password - out pops a hash, that can be used like `useradd <username> --create-home --password "<hash>" --shell /bin/bash --user-group`
