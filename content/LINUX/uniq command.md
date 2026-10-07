[[LINUX/]]

## `uniq` Command in macOS

The **`uniq` command** is used to **detect or remove repeated consecutive lines** from a file.

### Syntax

```bash
uniq [options] file
```

### Important Options

|Option|Meaning|
|---|---|
|`-c`|Count occurrences of each line|
|`-d`|Display only duplicate lines|
|`-u`|Display only unique lines|
|`-i`|Ignore case differences|
|`-f`|Skip specified number of fields|
|`-s`|Skip specified number of characters|

### One Example

```bash
uniq -c students.txt
```

This displays each **consecutive repeated line along with its count**.

> **Important:** `uniq` only detects duplicates that are **next to each other**. Use `sort` first if duplicates may be scattered:

```bash
sort students.txt | uniq
```
