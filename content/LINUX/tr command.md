[[LINUX/]]

## `tr` Command in macOS

The **`tr` (translate)** command is used to **translate, replace, or delete characters** from text.

### Syntax

```
tr [options] set1 [set2]
```

### Important Options

|Option|Meaning|
|---|---|
|`-d`|Delete specified characters|
|`-s`|Squeeze repeated characters into one|
|`-c`|Complement the character set|
|`-C`|Complement the character set|
|`-t`|Translate only the first set to the length of the second set|

### One Example

Convert student names from **lowercase to uppercase**:

```
cat students.txt | tr 'a-z' 'A-Z'
```

If `students.txt` contains:

```
shashank
rahul
aman
```

Output:

```
SHASHANK
RAHUL
AMAN
```

**`tr` = Translate characters**.
