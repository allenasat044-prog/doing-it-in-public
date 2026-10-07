[[LINUX/]]

## File Permission Commands in macOS

File permissions control **who can read, write, or execute** a file.

### Main Commands

|Command|Purpose|
|---|---|
|`ls -l`|View file permissions|
|`chmod`|Change file permissions|
|`chown`|Change file owner|
|`chgrp`|Change group ownership|
|`umask`|Set default permissions for new files|

### Permission Types

|Symbol|Meaning|Number|
|---|---|--:|
|`r`|Read|4|
|`w`|Write|2|
|`x`|Execute|1|
|`-`|No permission|0|

### One Example

```bash
chmod 755 script.sh
```

This gives:

```text
Owner  → rwx (7)
Group  → r-x (5)
Others → r-x (5)
```

So the permission becomes:

```text
-rwxr-xr-x
```

**`chmod` = Change file permissions.**

## File Permission Symbols

|Symbol|Meaning|Number|
|---|---|--:|
|`r`|Read permission|4|
|`w`|Write permission|2|
|`x`|Execute permission|1|
|`-`|No permission|0|

### Permission Combinations

|Symbol|Meaning|Number|
|---|---|--:|
|`---`|No permissions|0|
|`--x`|Execute only|1|
|`-w-`|Write only|2|
|`-wx`|Write + Execute|3|
|`r--`|Read only|4|
|`r-x`|Read + Execute|5|
|`rw-`|Read + Write|6|
|`rwx`|Read + Write + Execute|7|

### Permission Groups

For:

```text
-rwxr-xr--
```

|Part|Meaning|
|---|---|
|`-`|File type|
|`rwx`|Owner permissions|
|`r-x`|Group permissions|
|`r--`|Others' permissions|

So `rwxr-xr--` = **7-5-4**.