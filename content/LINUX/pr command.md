
[[LINUX/]]

## `pr` Command in macOS

The `pr` command **formats text files for printing**, including headers, page numbers, columns, and spacing.

### Syntax

```
pr [options] file
```

### All Important Options

|Option|Meaning|
|---|---|
|`-1`|Format into 1 column|
|`-2`|Format into 2 columns|
|`-3`|Format into 3 columns|
|`-4`|Format into 4 columns|
|`-5`|Format into 5 columns|
|`-a`|Print columns across the page|
|`-d`|Double-space output|
|`-f`|Use form-feed instead of page breaks|
|`-h "text"`|Set a custom header|
|`-l number`|Set page length|
|`-n`|Add line numbers|
|`-o number`|Set left margin/indentation|
|`-r`|Omit warning for files that cannot be opened|
|`-s`|Separate columns with a single character|
|`-t`|Remove header and footer|
|`-w number`|Set page width|
|`+page`|Start printing from a specific page|
|`-m`|Merge multiple files side-by-side|
|`-S string`|Specify the column separator|
|`-J`|Join lines instead of truncating them|

### One Example

```
pr -2 -h "Student Details" students.txt
```

This displays `students.txt` in **2 columns** with **"Student Details"** as the header.