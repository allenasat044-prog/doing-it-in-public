
[[LINUX/]]
## `bc` Command in macOS

The **`bc` (Basic Calculator)** command is used for **performing mathematical calculations** in the terminal.

### Important Options

|Option|Meaning|
|---|---|
|`-l`|Enables the math library and provides decimal calculations|
|`-q`|Quiet mode; doesn't display the welcome message|

### One Example

```bash
echo "25 * 4 + 10" | bc
```

Output:

```text
110
```

---

## `tree` Command in macOS

The **`tree` command** displays **files and directories in a tree-like structure**.

> `tree` may not be installed by default on macOS. Install it using Homebrew:

```bash
brew install tree
```

### Important Options

|Option|Meaning|
|---|---|
|`-a`|Show hidden files|
|`-d`|Show directories only|
|`-L`|Limit directory depth|
|`-f`|Show full path|
|`-h`|Show file sizes in human-readable format|
|`-i`|Don't show indentation lines|

### One Example

```bash
tree -L 2
```

This displays the current directory and its contents up to **2 levels deep**.