[[LINUX/]]

## `expand` and `unexpand` Commands in macOS

These commands are used to **convert between tabs and spaces** in text files.

---

# 1. `expand` Command

The **`expand` command converts TAB characters into spaces**.

### Syntax

```
expand [options] [file]
```

### Example

Suppose `file.txt` contains tabs:

```
Name    Age    Course
John    20     IT
```

Run:

```
expand file.txt
```

The TAB characters are converted into spaces.

### Set Tab Width

```
expand -t 4 file.txt
```

Here, `-t 4` means tabs are expanded using **4 spaces**.

### Using Pipe

```
cat file.txt | expand
```

### Save the Output

```
expand file.txt > newfile.txt
```

---

# 2. `unexpand` Command

The **`unexpand` command converts spaces into TAB characters**.

### Syntax

```
unexpand [options] [file]
```

### Example

```
unexpand file.txt
```

This converts appropriate sequences of spaces into tabs.

### Convert Leading Spaces to Tabs

```
unexpand -a file.txt
```

`-a` means **convert all spaces that can be represented as tabs**, not just leading spaces.

### Set Tab Width

```
unexpand -t 4 file.txt
```

This uses a tab width of **4 columns**.

---

## `expand` vs `unexpand`

|Command|Conversion|
|---|---|
|`expand`|TAB → Spaces|
|`unexpand`|Spaces → TAB|

### ⭐ Easy way to remember

```
expand    → TAB becomes SPACE
unexpand  → SPACE becomes TAB
```

For practical exams, the most important commands are:

```
expand file.txt
expand -t 4 file.txt

unexpand file.txt
unexpand -a file.txt
unexpand -t 4 file.txt
```