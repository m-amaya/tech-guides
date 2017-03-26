# Regular Expressions in `grep`

A **regular expression** is the particular search pattern that is entered to find a particular target string. The desired string that someone hopes to find is a **target string**. Regular expressions are comprised of two types of characters:

1. normal text characters (*literals*)
2. special characters (*metacharacters*)

## Quotation Marks

| Type | Use |
|:---- |:--- |
| `''` | placed around the regular expression |
| ```` | executes everything inside as a command, and then uses that as the input string |
| `""` | same a single quotes, but becomes possible to use environment variables as part of a search pattern |

## Regular Expression Metacharacters

<table>
  <tr>
    <td>Metacharacter</td>
    <td>Name</td>
    <td>Matches</td>
  </tr>
  <tr>
    <td colspan="3">**Items to match a single character**</td>
  </tr>
  <tr>
    <td>`.`</td>
    <td>Dot</td>
    <td>Any one character</td>
  </tr>
</table>
