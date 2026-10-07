[[LINUX/]]

## `wc` Command in macOS

The **`wc` (word count)** command is used to count **lines, words, characters, and bytes** in a file.

### Syntax

```bash
wc [options] file
```

### Important Options

|Option|Meaning|
|---|---|
|`-l`|Count lines|
|`-w`|Count words|
|`-c`|Count bytes|
|`-m`|Count characters|
|`-L`|Display length of the longest line|

### One Example

```bash
wc -lwc students.txt
```

This displays the **number of lines, words, and bytes** in `students.txt`.

**`wc` = Word Count**.