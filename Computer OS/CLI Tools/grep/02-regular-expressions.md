# Regular Expressions in `grep`

A **regular expression** is the particular search pattern that is entered to find a particular target string. The desired string that someone hopes to find is a **target string**. Regular expressions are comprised of two types of characters:

1. normal text characters (*literals*)
2. special characters (*metacharacters*)

## Quotation Marks

| Type | Use |
|:---- |:--- |
| `''` | placed around the regular expression |
| ` `` ` | executes everything inside as a command, and then uses that as the input string |
| `""` | same as single quotes, but becomes possible to use environment variables as part of a search pattern |

## Regular Expression Precedence

1. Parentheses
2. Repetition
2. Concatenation
3. Alternation

## Regular Expression Metacharacters

<table>
    <thead>
        <th>Metacharacter</th>
        <th>Name</th>
        <th>Matches</th>
        <th>Example(s)</th>
    </thead>
    <tbody>
        <tr>
            <td colspan="4"><em>Items to match a single character</em></td>
        </tr>
        <tr>
            <td><code>.</code></td>
            <td>Dot</td>
            <td>Any one character</td>
            <td><code>'r.d'</code></td>
        </tr>
        <tr>
            <td><code>[...]</code></td>
            <td>Character class</td>
            <td>Any character listed in brackets</td>
            <td><code>'[a-f]'</code><br />
            <code>'[aeiou]'</code></td>
        </tr>
        <tr>
            <td><code>[^...]</code></td>
            <td>Negated character class</td>
            <td>Any character not listed in brackets</td>
            <td><code>'..[^24680]'</code></td>
        </tr>
        <tr>
            <td><code>\char</code></td>
            <td>Escape character</td>
            <td>The character after the slash literally; used when you want to search for a "special" character</td>
            <td><code>'\$'</code></td>
        </tr>
        <tr>
            <td colspan="4"><em>Items that match a position</em></td>
        </tr>
        <tr>
            <td><code>^</code></td>
            <td>Caret</td>
            <td>Start of a line</td>
            <td><code>'^red'</code></td>
        </tr>
        <tr>
            <td><code>$</code></td>
            <td>Dollar sign</td>
            <td>End of a line</td>
            <td><code>'-$'</code></td>
        </tr>
        <tr>
            <td><code>\<</code></td>
            <td>Backslash less-than</td>
            <td>Start of a word</td>
            <td><code>'\&lt;un'</code></td>
        </tr>
        <tr>
            <td><code>\></code></td>
            <td>Backslash greater-than</td>
            <td>End of a word</td>
            <td><code>'ing\>'</code></td>
        </tr>
        <tr>
            <td colspan="4"><em>The quantifiers</em></td>
        </tr>
        <tr>
            <td><code>?</code></td>
            <td>Question mark</td>
            <td>Optional; considered a quantifier</td>
            <td><code>'colors?'</code></td>
        </tr>
        <tr>
            <td><code>&#42;</code></td>
            <td>Asterisk</td>
            <td>Any number (including zero); sometimes used as a general wildcard</td>
            <td><code>'install.&#42;file'</code></td>
        </tr>
        <tr>
            <td><code>+</code></td>
            <td>Plus</td>
            <td>One or more of the preceding expression</td>
            <td><code>'150+'</code></td>
        </tr>
        <tr>
            <td><code>{N}</code></td>
            <td>Match exactly</td>
            <td>Match exactly <code>N</code> times</td>
            <td><code>'150{3}\b'</code></td>
        </tr>
        <tr>
            <td><code>{N,}</code></td>
            <td>Match at least</td>
            <td>Match at least <code>N</code> times</td>
            <td><code>'150{3,}\b'</code></td>
        </tr>
        <tr>
            <td><code>{min,max}</code></td>
            <td>Specified range</td>
            <td>Match between <code>min</code> and <code>max</code> times</td>
            <td><code>'150{2,3}\b'</code></td>
        </tr>
        <tr>
            <td colspan="4"><em>Other</em></td>
        </tr>
        <tr>
            <td><code>|</code></td>
            <td>Alternation</td>
            <td>Matches either expression given</td>
            <td><code>'apple|orange|banana|peach'</code></td>
        </tr>
        <tr>
            <td><code>-</code></td>
            <td>Dash</td>
            <td>Indicates a range</td>
            <td><code>'[0-5]'</code></td>
        </tr>
        <tr>
            <td><code>(...)</code></td>
            <td>Parentheses</td>
            <td>Used to limit scope of alternation</td>
            <td><code>'(red|blue) plate'</code><br />
            <code>'(150){3}'</code></td>
        </tr>
        <tr>
            <td><code>\1</code>, <code>\2</code>, <code>...</code></td>
            <td>Backreference</td>
            <td>Matches text previously matched within parentheses (e.g., first set, second set, etc.)</td>
            <td></td>
        </tr>
        <tr>
            <td><code>\b</code></td>
            <td>Word boundary</td>
            <td>Batches characters that typically mark the end of a word (e.g., space, period, etc.)</td>
            <td><code>'\bheart\b'</code></td>
        </tr>
        <tr>
            <td><code>\B</code></td>
            <td>Backslash</td>
            <td>This is an alternative to using "\\" to match a backslash, used for readability.</td>
            <td><code>'c:\Bwindows'</code></td>
        </tr>
        <tr>
            <td><code>\w</code></td>
            <td>Word character</td>
            <td>This is used to match any "word" character (i.e., any letter, number, and the underscore character)</td>
            <td></td>
        </tr>
        <tr>
            <td><code>\W</code></td>
            <td>Non-word character</td>
            <td>This matches any character that isn't used in words (i.e., not a letter, number, or underscore)</td>
            <td></td>
        </tr>
        <tr>
            <td><code>\`</code></td>
            <td>Start of buffer</td>
            <td>Match the start of a buffer sent to <code>grep</code></td>
            <td></td>
        </tr>
        <tr>
            <td><code>\'</code></td>
            <td>End of buffer</td>
            <td>Matches the end of a buffer sent to to <code>grep</code></td>
            <td></td>
        </tr>
    </tbody>
</table>

## POSIX Character Definitions

Additionally, regular expressions come with a set of POSIX character definitions that create shortcuts to find certain classes of characters. POSIX is a set of standards created by the Institute of Electrical and Electronics Engineers (IEEE) to describe how Unix-style operating systems should behave. Among other things, POSIX has definitions on how regular expressions should work with shell utilities such as `grep`.

| POSIX Definition | Contents of character definition |
| :------------- | :------------- |
| `[:alpha:]` | Any alphabetical character, regardless of case |
| `[:digit:]` | Any numerical character |
| `[:alnum:]` | Any alphabetical or numerical character |
| `[:blank:]` | Space or tab characters |
| `[:xdigit:]` | Hexadecimal characters; any number or `A-F` or `a-f` |
| `[:punct:]` | Any punctuation symbol |
| `[:print:]` | Any printable character (not control characters) |
| `[:space:]` | Any whitespace character |
| `[:graph:]` | Exclude whitespace characters |
| `[:upper:]` | Any uppercase letter |
| `[:lower:]` | Any lowercase letter |
| `[:cntrl:]` | Control characters |
