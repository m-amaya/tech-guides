# `grep` or `grep -G`

## Match Control

* `-e <pattern>`, `--regex=<pattern>`

    ```bash
    $ grep -e -style doc.txt
    ```
    Ensures that `grep` recognizes the pattern as the regular expression argument. Useful if the regular expression begins with a hyphen.

* `-f <file>`, `--file=<file>`

    ```bash
    $ grep -f pattern.txt searchhere.txt
    ```
    Takes patterns from `file`. This option allows you to input all the patterns you want to match into a file (`pattern.txt`). Then, `grep` searches for all the patterns from `pattern.txt` in the designated file `searchhere.txt`. `grep` return every line that matches any pattern. The pattern file must list one pattern per line. If `pattern.txt` is empty, nothing will match.

* `-i`, `--ignore-case`

    ```bash
    $ grep -i 'help' me.txt
    ```
    Ignores capitalization in the given regular expressions, either via the command line or in a file of regular expressions (`-f`). A similar but obsolete synonym to this option is `-y`.

* `-v`, `--invert-match`

    ```bash
    $ grep -v oranges <filename>
    ```
    Returns lines that do *NOT* match, instead of lines that do.

* `-w`, `--word-regexp`

    ```bash
    $ grep -w 'xyz' <filename>
    ```
    Matches only when the input text consists of full words. Letters, digits, and the underscore character are all considered part of a word; any other character is considered a word boundary, as are the start and end of the line. This is the equivalent of putting `\b` at the beginning and end of the regular expression.

* `-x`, `--line-regexp`

    ```bash
    $ grep -x 'Hello, world!' <filename>
    ```
    Like `-w`, but must match an entire line. Lines that have additional content will not be matched. This can be useful for parsing logfiles for specific content that might include cases you are not interested in seeing.

## General Output Control

* `-c`, `--count`

    ```bash
    $ grep -c contact.html <access.log>
    ```

    ```bash
    $ grep -c -v contact.html <access.log>
    ```

* `--color[=WHEN]`, `--colour[=WHEN]`

    ```bash
    $ grep -color[=auto] <regexp> <filename>
    ```

* `-l`, `--files-with-matches`

    ```bash
    $ grep -l "ERROR:" *.log
    ```

* `-L`, `--files-without-match`

    ```bash
    $ grep -L 'ERROR:' *.log
    ```

* `-m <NUM>`, `--max-count=<NUM>`

    ```bash
    $ grep -m 10 'ERROR:' *.log
    ```

* `-o`, `--only-matching`

    ```bash
    $ grep -o <pattern> <filename>
    ```

* `-q`, `--quiet`, `--silent`

    ```bash
    $ grep -q <pattern> <filename>
    ```

* `-s`, `--no-messages`

    ```bash
    $ grep -s <pattern> <filename>
    ```

## Output Line Prefix Control

* `-b`, `--byte-offset`

    ```bash
    $ grep -b <pattern> <filename>
    ```

    ```bash
    $ grep -b -o <pattern> <filename>
    ```

* `-H`, `--with-filename`

    ```bash
    $ grep -H <pattern> <filename>
    ```

* `-h`, `--no-filename`

    ```bash
    $ grep -h <pattern> *
    ```

* `--label=<LABEL>`

    ```bash
    $ gzip -cd file.gz | grep --label=<LABEL> <pattern>
    ```

* `-n`, `--line-number`

    ```bash
    $ grep -n <pattern> <filename>
    ```

* `-T`, `--initial-tab`

    ```bash
    $ grep -T <pattern> <filename>
    ```

* `-u`, `--unix-byte-offsets`

    ```bash
    $ grep -u -b <pattern> <filename>
    ```

* `-Z`, `--null`

    ```bash
    $ grep -Z <pattern> <filename>
    ```

## Context Line Control

* `-A <NUM>`, `--after-context=<NUM>`

    ```bash
    $ grep -A 3 Copyright <filename>
    ```

* `-B <NUM>`, `--before-context=<NUM>`

    ```bash
    $ grep -B 3 Copyright <filename>
    ```

* `-C <NUM>`, `--context=<NUM>`

    ```bash
    $ grep -C 3 Copyright <filename>
    ```

## File and Directory Selection

* `-a`, `--text`

    ```bash
    $ grep -a <pattern> <filename>
    ```

* `--binary-files=<TYPE>`

    ```bash
    $ grep --binary-files=<TYPE> <pattern> <filename>
    ```

* `-D <ACTION>`, `--devices=<ACTION>`

    ```bash
    $ grep -D read 123-45-6789 /dev/hda1
    ```

* `-d <ACTION>`, `--directories=<ACTION>`

    ```bash
    $ grep -d <ACTION> <pattern> <path>
    ```

* `--exclude=<GLOB>`

    ```bash
    $ grep --exclude=<PATTERN> <path>
    ```

* `--exclude-from=<FILE>`

    ```bash
    $ grep --exclude-from=<FILE> <path>
    ```

* `--exclude-dir=<DIR>`

    ```bash
    $ grep --exclude-dir=<DIR> <pattern> <path>
    ```

* `-l`

    ```bash
    $ grep -l <pattern> <filename>
    ```

* `--include=<GLOB>`

    ```bash
    $ grep --include=*.log <pattern> <filename>
    ```

* `-R`, `-r`, `--recursive`

    ```bash
    $ grep -R <pattern> <path>
    $ grep -r <pattern> <path>
    ```

## Other Options

* `--line-buffered`

    ```bash
    $ grep --line-buffered <pattern> <filename>
    ```

* `--mmap`

    ```bash
    $ grep --mmap <pattern> <filename>
    ```

* `-U`, `--binary`

    ```bash
    $ grep -U <pattern> <filename>
    ```

* `-V`, `--version`
* `-z`, `--null-data`

    ```bash
    $ grep -z <pattern>
    ```
