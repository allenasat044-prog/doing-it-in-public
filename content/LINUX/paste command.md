[[LINUX/]]

## `paste` Command in macOS

The **`paste` command** is used to **merge lines from two or more files horizontally**.

### Syntax

```bash
paste [options] file1 file2
```

### Important Options

|Option|Meaning|
|---|---|
|`-d`|Specify a delimiter between columns|
|`-s`|Paste files serially (one file at a time)|
|`-`|Read input from standard input|

### One Example

```bash
paste -d "," names.txt marks.txt
```

This combines corresponding lines from `names.txt` and `marks.txt`, separating them with a **comma**.

**`paste` = Combine files horizontally (column-wise).**