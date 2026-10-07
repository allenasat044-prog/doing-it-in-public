[[LINUX/]]


## `nl` Command in macOS

The **`nl` command** is used to **display the contents of a file with line numbers**.

### Basic Syntax

```
nl [options] [file]
```

### Simple Example

Suppose `students.txt` contains:

```
Shashank
Rahul
Aman
Priya
```

Run:

```
nl students.txt
```

Output:

```
     1  Shashank
     2  Rahul
     3  Aman
     4  Priya
```

### Common Options

|Option|Meaning|Example|
|---|---|---|
|`-b a`|Number **all** lines, including blank lines|`nl -b a file.txt`|
|`-b t`|Number only non-empty lines|`nl -b t file.txt`|
|`-v`|Set starting line number|`nl -v 10 file.txt`|
|`-i`|Set line-number increment|`nl -i 2 file.txt`|
|`-w`|Set width of line numbers|`nl -w 3 file.txt`|

### Start Numbering From 10

```
nl -v 10 students.txt
```

Output:

```
    10  Shashank
    11  Rahul
    12  Aman
```

### Number Every Line

```
nl -b a students.txt
```

### Using `nl` with Another Command

For example, display the output of `date` with a line number:

```
date | nl
```

Or number the output of another command:

```
cat students.txt | nl
```

### ⭐ Most Important for Practical

```
nl file.txt
```

**`nl` = Number Lines** → displays text with line numbers.