[[LINUX/]]



## `cut` Command in macOS

The `cut` command is used in the **Terminal** to extract specific sections (characters, bytes, or fields) from each line of text.

### Basic Syntax

```
cut [options] [file]
```

### Common Options

|Option|Meaning|Example|
|---|---|---|
|`-c`|Extract characters|`cut -c 1-5 file.txt`|
|`-b`|Extract bytes|`cut -b 1-5 file.txt`|
|`-d`|Specify delimiter|`cut -d "," -f 1 file.csv`|
|`-f`|Extract fields/columns|`cut -f 1 file.txt`|
|`-s`|Skip lines without delimiter|`cut -d "," -s -f 1 file.csv`|

### Examples

**1. Extract first 5 characters**

```
cut -c 1-5 names.txt
```

**2. Extract the first character**

```
cut -c 1 names.txt
```

**3. Extract characters 2 to 6**

```
cut -c 2-6 names.txt
```

**4. Extract the first field separated by comma**

```
cut -d "," -f 1 students.csv
```