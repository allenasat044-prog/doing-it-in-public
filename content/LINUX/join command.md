[[LINUX/]]

## `join` Command in macOS

The **`join` command** is used to **combine lines from two files based on a common field**.

### Syntax

```bash
join [options] file1 file2
```

### Important Options

|Option|Meaning|
|---|---|
|`-1 field`|Use the specified field from file 1|
|`-2 field`|Use the specified field from file 2|
|`-a 1`|Include unmatched lines from file 1|
|`-a 2`|Include unmatched lines from file 2|
|`-v 1`|Display only unmatched lines from file 1|
|`-v 2`|Display only unmatched lines from file 2|
|`-e string`|Replace missing fields with a specified string|
|`-t char`|Specify the field separator|

### One Example

If `students.txt` and `marks.txt` both contain student IDs:

```bash
join students.txt marks.txt
```

The command combines records having the **same ID**.

> **Important:** The files should normally be **sorted by the join field** before using `join`.