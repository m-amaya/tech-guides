# `grep` or `grep -G`

Basic `grep`, or `grep -G`, is the default pattern matching type that is used when calling `grep`.

In basic `grep`, the "extended" regular expressions metacharacters--`?`, `+`, `{`, `}`, `|`, `(`, and `)`--do not work. The functions provided by those characters exist if you preface them with an escape.

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
    Instead of the normal output, you receive just a count of how many lines matched in each input file.

    ```bash
    $ grep -c -v contact.html <access.log>
    ```
    Returns a count of all the lines that do *NOT* match the given string.

* `--color[=WHEN]`, `--colour[=WHEN]`

    ```bash
    $ grep -color[=auto] <regexp> <filename>
    ```
    Assuming the terminal can support color, `grep` will colorize the pattern in the output. This is done by surrounding the matched (nonempty) string, matching lines, context lines, filenames, line numbers, byte offsets, and separators with escape sequences that the terminal recognizes as color markers. Color is defined by the environment variable `GREP_COLORS`.

    `WHEN` has three options:
    1. `never`
    2. `always`
    3. `auto`

* `-l`, `--files-with-matches`

    ```bash
    $ grep -l "ERROR:" *.log
    ```
    Instead of normal output, prints just the names of input files containing the pattern. As with `-L`, the search stops on the first match. This is often referred to as *"lazy matching"*.

* `-L`, `--files-without-match`

    ```bash
    $ grep -L 'ERROR:' *.log
    ```
    Instead of normal output, prints just the name of input files that contain no matches.

* `-m <NUM>`, `--max-count=<NUM>`

    ```bash
    $ grep -m 10 'ERROR:' *.log
    ```
    This option tells `grep` to stop reading a file after `<NUM>` lines are matched.

* `-o`, `--only-matching`

    ```bash
    $ grep -o <pattern> <filename>
    ```
    Prints only the text that matches, instead of the whole line of input.

* `-q`, `--quiet`, `--silent`

    ```bash
    $ grep -q <pattern> <filename>
    ```
    Suppresses output. The command still conveys useful information because the `grep` command's exit status can be checked.

    Exit codes:
    * `0` - success, match found
    * `1` - success, no match found
    * `2` - fail, program cannot run

* `-s`, `--no-messages`

    ```bash
    $ grep -s <pattern> <filename>
    ```
    Silently discards any error messages resulting from non-existent files or permissions errors. It will also suppress useful diagnostic information, which could mean that problems may not be discovered.

## Output Line Prefix Control

* `-b`, `--byte-offset`

    ```bash
    $ grep -b <pattern> <filename>
    ```
    Displays the byte offset of each matching text instead of the line number.

    ```bash
    $ grep -b -o <pattern> <filename>
    ```
    An `-o` option prints the offset along with the matched pattern itself and not the whole matched line containing the pattern.

* `-H`, `--with-filename`

    ```bash
    $ grep -H <pattern> <filename>
    ```
    Includes the name of the file before each line printed, and is the default when more than one file is input to the search.

    > **NOTE:** Uses the relative (not absolute) paths and filenames.

* `-h`, `--no-filename`

    ```bash
    $ grep -h <pattern> *
    ```
    The opposite of `-H`. When more than one file is involved, it suppresses printing the filename before each output. It is the default when only one file or standard input is involved.

* `--label=<LABEL>`

    ```bash
    $ gzip -cd file.gz | grep --label=<LABEL> <pattern>
    ```
    When the input is taken from standard input, the `--label` option will prefix the line with `<LABEL>`.

* `-n`, `--line-number`

    ```bash
    $ grep -n <pattern> <filename>
    ```
    Includes the line number of each line displayed, where the first line of the file is 1.

* `-T`, `--initial-tab`

    ```bash
    $ grep -T <pattern> <filename>
    ```
    Inserts a tab before each matching line, putting the tab between the information generated by `grep` and the matching lines.

* `-u`, `--unix-byte-offsets`

    ```bash
    $ grep -u -b <pattern> <filename>
    ```
    This option only works under the MS-DOS and Microsoft Windows platforms and needs to be invoked with `-b`. This option will computer the byte-offset as if it were running under a Unix system and strip out carriage return characters.

* `-Z`, `--null`

    ```bash
    $ grep -Z <pattern> <filename>
    ```
    Prints an ASCII NUL (a zero byte) after each filename. This is useful when processing filenames that may contain special characters (such as carriage returns).

## Context Line Control

* `-A <NUM>`, `--after-context=<NUM>`

    ```bash
    $ grep -A 3 Copyright <filename>
    ```
    Offers a context for matching lines by printing the `NUM` lines that follow each match. This is useful when searching through source code.

* `-B <NUM>`, `--before-context=<NUM>`

    ```bash
    $ grep -B 3 Copyright <filename>
    ```
    Same concept as the `-A <NUM>` option, except that it prints the lines *before* the match instead of after it.

* `-C <NUM>`, `--context=<NUM>`

    ```bash
    $ grep -C 3 Copyright <filename>
    ```
    Operates as if the user entered both the `-A <NUM>` and `-B <NUM>` options. It will displays `NUM` lines before and after the match. A group separator (`--`) is placed between each set of matches.

## File and Directory Selection

* `-a`, `--text`

    ```bash
    $ grep -a <pattern> <filename>
    ```
    Equivalent to the `--binary-files=text` option, allowing a binary file to be processed as if it were a text file.

* `--binary-files=<TYPE>`

    ```bash
    $ grep --binary-files=<TYPE> <pattern> <filename>
    ```
    When `grep` first examines a file, it determines whether the file is a "binary" file (a file primarily composed of non-human-readable text) and changes its output accordingly.

    `TYPE` can be:
    1. `binary` - displays the message "Binary file `<somefile.bin>` matches".
    2. `without-match` - does not search the binary file, and proceeds as if it had no matches (equivalent to the `-l` option).
    3. `text` - processes the binary file like text (equivalent to the `-a` option)

* `-D <ACTION>`, `--devices=<ACTION>`

    ```bash
    $ grep -D read 123-45-6789 /dev/hda1
    ```
    If the input file is a special file, such as a FIFO or a socket, this flag tells `grep` how to proceed. By default, `grep` will process these files as if they were normal files on a system.

    `ACTION` can be:
    1. `skip` - silently ignores the file
    2. `read` - reads through the device as if it were a normal file

* `-d <ACTION>`, `--directories=<ACTION>`

    ```bash
    $ grep -d <ACTION> <pattern> <path>
    ```
    Tells `grep` how to process directories submitted as input files.

    `ACTION` can be:
    1. `read` - reads the directory as if it were a file
    2. `recurse` - searches the files within that directory (same as the `-R` option)
    3. `skip` - skips the directory without searching it

* `--exclude=<GLOB>`

    ```bash
    $ grep --exclude=<PATTERN> <path>
    ```
    Refinex the list of input files by telling `grep` ti ignore files whose names match the specified pattern.

    `PATTERN` can be:
    1. an entire filename
    2. can contain the typical "file-globbing" wildcards the shell uses when matching files (`*`, `?`, and `[]`)

* `--exclude-from=<FILE>`

    ```bash
    $ grep --exclude-from=<FILE> <path>
    ```
    Similar to the `--exclude` option, except that it takes a list of patterns from a specified filename, which lists each pattern on a separate line. `grep` will ignore all files that match any lines in the list of patterns given.

* `--exclude-dir=<DIR>`

    ```bash
    $ grep --exclude-dir=<DIR> <pattern> <path>
    ```
    Any directories in the path matching the pattern `DIR` will be excluded from recursive searches. In this case, the actual directory name (relative name or absolute path name) has to be included to be ignored. This option must be used with the `-r` option or the `-d recurse` option in order to be relevant.

* `-l`

    ```bash
    $ grep -l <pattern> <filename>
    ```
    Same as the `--binary-files=without-match` option. When `grep` finds a binary file, it will assume there is no match in the file.

* `--include=<GLOB>`

    ```bash
    $ grep --include=*.log <pattern> <filename>
    ```
    Limits searches to input files whose names match the given pattern. This option is particularly useful when searching directories using the `-R` option. Files no matching the given pattern will be ignored. An entire filename can be specified, or can contain the typical "file-globbing" wildcards the shell uses when matching files (`*`, `?`, and `[]`).

* `-R`, `-r`, `--recursive`

    ```bash
    $ grep -R <pattern> <path>
    $ grep -r <pattern> <path>
    ```
    Searches all files underneath each directory submitted as an input file to `grep`.

## Other Options

* `--line-buffered`

    ```bash
    $ grep --line-buffered <pattern> <filename>
    ```
    Uses line buffering for the output. Line bufferring output usually leads to a decrease in performance. The default behavior of `grep` is to use unbuffered output. This is generally a matter of preference.

* `--mmap`

    ```bash
    $ grep --mmap <pattern> <filename>
    ```
    Uses the `mmap()` function instead of the `read()` function to process data. This can lead to a performance improvement, but may cause errors if there is an I/O problem or the file shrinks while being searched.

* `-U`, `--binary`

    ```bash
    $ grep -U <pattern> <filename>
    ```
    An MS-DOS/Windows-specific option that causes `grep` to treat all files as binary. Normally, `grep` would strip out carriage returns before doing pattern matching; this option overrides that behavior. This does, however, require you to be more thoughtful when writing patterns.

* `-V`, `--version`

    Simply outputs the version information about `grep` and then exits.

* `-z`, `--null-data`

    ```bash
    $ grep -z <pattern>
    ```
    Input lines are treated as though each one ends with a zero byte, or the ASCII NUL character, instead of a newline. Similar to the `-Z` or `--null` options, except this option works with input, not output.
