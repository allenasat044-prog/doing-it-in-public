[[LINUX/]]
## `sort` Command in macOS

The **`sort` command** is used to **arrange lines of text in a specific order**, usually alphabetically or numerically.

### Syntax

```bash
sort [options] file
```

### Important Options

|Option|Meaning|
|---|---|
|`-r`|Reverse order|
|`-n`|Numeric sorting|
|`-f`|Ignore case|
|`-u`|Remove duplicate lines|
|`-k`|Sort by a specific field/column|
|`-t`|Specify field separator|
|`-o`|Save sorted output to a file|
|`-b`|Ignore leading spaces|
|`-d`|Dictionary order|
|`-h`|Human-readable numeric sorting|

### One Example

```bash
sort -nr marks.txt
```

This sorts the numbers in `marks.txt` in **numeric descending order**.

**`sort` = Arrange lines in order.**