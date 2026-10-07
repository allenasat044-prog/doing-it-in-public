[[LINUX/]]

## `find` Command in macOS

The **`find` command** is used to **search for files and directories** based on name, type, size, time, permissions, etc.

### Important Options

|Option|Meaning|
|---|---|
|`-name`|Search by file name|
|`-type f`|Search for files|
|`-type d`|Search for directories|
|`-size`|Search by file size|
|`-mtime`|Search by modification time|
|`-user`|Search by owner|
|`-perm`|Search by permissions|
|`-exec`|Execute a command on found files|

### One Example

```bash
find . -name "*.txt"
```

This searches the **current directory and its subdirectories** for all `.txt` files.

---

## `grep` Command in macOS

The **`grep` command** is used to **search for a specific pattern or text inside files**.

### Important Options

|Option|Meaning|
|---|---|
|`-i`|Ignore case|
|`-v`|Show lines that don't match|
|`-n`|Show line numbers|
|`-c`|Count matching lines|
|`-r`|Search recursively|
|`-w`|Match whole words|
|`-E`|Use extended regular expressions|
|`-o`|Show only the matching part|

### One Example

```bash
grep -in "error" logfile.txt
```

This searches for **`error`**, ignores uppercase/lowercase differences, and displays the **line numbers** where it occurs.

**Easy difference:**

```text
find → searches for FILES/DIRECTORIES
grep → searches for TEXT inside files
```