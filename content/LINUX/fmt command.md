[[LINUX/]]


## `fmt` Command in macOS

The **`fmt` command** is used to **format and rearrange text** into lines of a specified width. It is mainly used to make long text easier to read.

### Basic Syntax

```
fmt [options] [file]
```

### Common Options

|Option|Meaning|Example|
|---|---|---|
|`-w width`|Set maximum line width|`fmt -w 40 file.txt`|
|`-c`|Preserve indentation|`fmt -c file.txt`|
|`-s`|Split long lines but don't join short lines|`fmt -s file.txt`|
|`-u`|Uniform spacing between words|`fmt -u file.txt`|

### Examples

**1. Format a text file**

```
fmt file.txt
```

**2. Set line width to 40 characters**

```
fmt -w 40 file.txt
```

**3. Format text using a pipe**

```
echo "This is a very long sentence that needs to be formatted properly." | fmt -w 30
```

Output will be arranged approximately like:

```
This is a very long sentence
that needs to be formatted
properly.
```

### Useful Practical Example

If you want to format the output of another command:

```
cat students.txt | fmt -w 50
```

Or combine it with `cut`:

```
cut -d "," -f 2 students.csv | fmt -w 30
```

**In short:**  
`fmt` → **formats text by adjusting line breaks and spacing.**