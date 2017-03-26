# `grep` or `grep -G`

## Match Control

| Option | Example | Description |
|:------ |:------- |:----------- |
| `-e <pattern>`, `--regex=<pattern>` | `grep -e -style doc.txt` | Ensures that `grep` recognizes the pattern as the regular expression argument. Useful if the regular expression begins with a hyphen. |
| `-f <file>`, `--file=<file>` | `grep -f pattern.txt searchhere.txt` | Takes patterns from `file`. This option allows you to input all the patterns you want to match into a file (`pattern.txt`). Then, `grep` searches for all the patterns from `pattern.txt` in the designated file `searchhere.txt`. `grep` return every line that matches any pattern. The pattern file must list one pattern per line. If `pattern.txt` is empty, nothing will match. |
| `-i`, `--ignore-case` | `grep -i 'help' me.txt` | Ignores capitalization in the given regular expressions, either via the command line or in a file of regular expressions (`-f`). A similar but obsolete synonym to this option is `-y`. |
| `-v`, `--invert-match` | `grep -v oranges <filename>` | Returns lines that do *NOT* match, instead of lines that do. |
| `-w`, `--word-regexp` | `grep -w 'xyz' <filename>` | Matches only when the input text consists of full words. Letters, digits, and the underscore character are all considered part of a word; any other character is considered a word boundary, as are the start and end of the line. This is the equivalent of putting `\b` at the beginning and end of the regular expression. |
| `-x`, `--line-regexp` | `grep -x 'Hello, world!' <filename>` | Like `-w`, but must match an entire line. Lines that have additional content will not be matched. This can be useful for parsing logfiles for specific content that might include cases you are not interested in seeing. |

## General Output Control

## Output Line Prefix Control

## Context Line Control

## File and Directory Selection

## Other Options
