[[LINUX/]]

# 🐧 Linux Commands Cheat Sheet

| Command | Description |
|---------|-------------|
| `ls` | List directory contents |
| `pwd` | Show current directory path |
| `cd` | Change directory |
| `locate` | Search files by name |
| `find` | Search files and directories |
| `mkdir` | Create a directory |
| `rmdir` | Remove an empty directory |
| `rm` | Delete files or directories |
| `cp` | Copy files or directories |
| `mv` | Move or rename files |
| `touch` | Create an empty file |
| `file` | Show file type |
| `zip` | Compress files into a ZIP archive |
| `unzip` | Extract a ZIP archive |
| `tar` | Archive files and directories |
| `nano` | Edit files with Nano |
| `vi` | Edit files with Vi |
| `jed` | Edit files with Jed |
| `cat` | Display file contents |
| `grep` | Search text patterns in files |
| `sed` | Replace or modify text patterns |
| `head` | Show the first lines of a file |
| `tail` | Show the last lines of a file |
| `awk` | Process and analyze text |
| `sort` | Sort file contents |
| `cut` | Extract sections of text |
| `diff` | Compare two files |
| `tee` | Output to terminal and file |
| `sudo` | Run a command as administrator |
| `su` | Switch user account |
| `whoami` | Show the current user |
| `chmod` | Change file permissions |
| `chown` | Change file ownership |
| `useradd` | Create a new user |
| `userdel` | Delete a user account |
| `passwd` | Set or change a password |
| `df` | Show disk space usage |
| `du` | Show directory size |
| `top` | Display running processes |
| `htop` | Interactive process viewer |
| `ps` | Show a process snapshot |
| `uname` | Show system information |
| `hostname` | Show or set the hostname |
| `time` | Measure command execution time |
| `systemctl` | Manage system services |
| `watch` | Run a command repeatedly |
| `jobs` | List shell background jobs |
| `kill` | Terminate a process |
| `shutdown` | Shut down or restart the system |
| `ping` | Test network connectivity |
| `wget` | Download files from the web |
| `curl` | Transfer data via a URL |
| `scp` | Copy files over SSH |
| `rsync` | Synchronize files between systems |
| `ip` | Manage network settings |
| `netstat` | Show network connections |
| `traceroute` | Trace the network packet path |
| `nslookup` | Query DNS records |
| `dig` | Perform a detailed DNS lookup |
| `history` | Show command history |
| `man` | Show a command's manual |
| `echo` | Print text to the terminal |
| `ln` | Create file links |
| `alias` | Create a command shortcut |
| `unalias` | Remove a command shortcut |
| `cal` | Display a calendar |
| `apt` | Manage packages (Debian/Ubuntu) |
| `dnf` | Manage packages (RHEL/Fedora) |

---

## 💡 Quick Notes

- **Navigation:** `pwd`, `ls`, `cd`
- **File Management:** `touch`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`
- **Viewing Files:** `cat`, `head`, `tail`, `file`
- **Searching:** `find`, `locate`, `grep`
- **Editing:** `nano`, `vi`, `jed`, `sed`
- **Compression:** `zip`, `unzip`, `tar`
- **Permissions:** `chmod`, `chown`, `sudo`
- **Users:** `whoami`, `su`, `useradd`, `userdel`, `passwd`
- **Processes:** `ps`, `top`, `htop`, `kill`, `jobs`
- **Disk Usage:** `df`, `du`
- **Networking:** `ping`, `curl`, `wget`, `scp`, `rsync`, `ip`, `netstat`, `traceroute`, `nslookup`, `dig`
- **System:** `uname`, `hostname`, `systemctl`, `shutdown`, `history`, `man`
# 🐧 Linux Commands Cheat Sheet

> [!info]
> A collection of essential Linux commands with syntax, options, and examples.

---

# 📂 File & Directory Commands

## `ls` — List Files and Directories

**Description**
Displays files and directories.

### Syntax
```bash
ls [options] [directory]
```

### Common Options

| Option | Description |
|---------|-------------|
| `-a` | Show hidden files |
| `-l` | Long listing format |
| `-R` | Recursive listing |

### Example

```bash
ls -Ra /path/to/dir
```

---

## `pwd` — Print Working Directory

### Syntax

```bash
pwd [options]
```

| Option | Description |
|---------|-------------|
| `-L` | Logical path |
| `-P` | Physical path |

### Example

```bash
pwd -P
```

---

## `cd` — Change Directory

### Syntax

```bash
cd [directory]
```

### Shortcuts

```bash
cd
cd ..
cd -
```

### Example

```bash
cd /var/www/html
```

---

## `mkdir` — Create Directories

### Syntax

```bash
mkdir [options] folder_name
```

| Option | Description |
|---------|-------------|
| `-p` | Create parent directories |
| `-m` | Set permissions |

### Example

```bash
mkdir -p project/src
```

---

## `rmdir` — Remove Empty Directory

```bash
rmdir folder_name
```

---

## `rm` — Remove Files & Directories

### Syntax

```bash
rm [options] file
```

| Option | Description |
|---------|-------------|
| `-r` | Recursive |
| `-f` | Force |
| `-i` | Confirm before delete |

### Example

```bash
rm -rf folder
```

---

## `cp` — Copy Files

```bash
cp -R source destination
```

---

## `mv` — Move or Rename

```bash
mv source destination
```

---

## `touch` — Create Empty File

```bash
touch file.txt
```

---

## `file` — Identify File Type

```bash
file filename
```

---

# 📦 Compression & Archives

## ZIP

```bash
zip -r archive.zip folder/
```

```bash
unzip archive.zip
```

---

## TAR

```bash
tar -czf archive.tar.gz folder/
```

Extract

```bash
tar -xzf archive.tar.gz
```

---

# 📝 Text Editors

## Nano

```bash
nano file.txt
```

## Vim

```bash
vi file.txt
```

## Jed

```bash
jed file.txt
```

---

# 📄 Viewing & Editing Files

## cat

```bash
cat file.txt
```

Combine files

```bash
cat file1 file2 > output.txt
```

---

## grep

```bash
grep "text" file.txt
```

Useful Options

- `-i`
- `-r`
- `-n`

---

## sed

```bash
sed 's/old/new/' file.txt
```

---

## head

```bash
head -n 5 file.txt
```

---

## tail

```bash
tail -f log.txt
```

---

## awk

```bash
awk '{print $1}' file.txt
```

---

## sort

```bash
sort file.txt
```

---

## cut

```bash
cut -d',' -f1 file.csv
```

---

## diff

```bash
diff file1 file2
```

---

## tee

```bash
command | tee output.txt
```

---

# 🔍 Search Commands

## locate

```bash
locate filename
```

---

## find

```bash
find . -name "*.txt"
```

---

# 👤 User Management

## sudo

```bash
sudo command
```

---

## su

```bash
su -
```

---

## whoami

```bash
whoami
```

---

## chmod

```bash
chmod 755 file
```

---

## chown

```bash
chown user:group file
```

---

## useradd

```bash
sudo useradd username
```

---

## passwd

```bash
passwd username
```

---

## userdel

```bash
sudo userdel username
```

---

# 💾 Disk Usage

## df

```bash
df -h
```

---

## du

```bash
du -sh folder
```

---

# ⚙️ Process Management

## top

```bash
top
```

---

## htop

```bash
htop
```

---

## ps

```bash
ps -A
```

---

## jobs

```bash
jobs
```

---

## kill

```bash
kill -9 PID
```

---

# 🖥️ System Information

## uname

```bash
uname -a
```

---

## hostname

```bash
hostname
```

---

## time

```bash
time command
```

---

## systemctl

```bash
sudo systemctl status service
```

---

## watch

```bash
watch -n 5 command
```

---

# 🌐 Networking

## ping

```bash
ping google.com
```

---

## wget

```bash
wget https://example.com/file.zip
```

---

## curl

```bash
curl https://example.com
```

---

## scp

```bash
scp file.txt user@server:/path
```

---

## rsync

```bash
rsync -avz source destination
```

---

## ip

```bash
ip address
```

---

## netstat

```bash
netstat -tuln
```

---

## traceroute

```bash
traceroute google.com
```

---

## nslookup

```bash
nslookup google.com
```

---

## dig

```bash
dig google.com
```

---

# 📜 History & Help

## history

```bash
history
```

Run command number:

```bash
!145
```

---

## man

```bash
man ls
```

---

# 💡 Miscellaneous

## echo

```bash
echo "Hello" > file.txt
```

Append

```bash
echo "Hello" >> file.txt
```

---

## ln

Symbolic Link

```bash
ln -s target shortcut
```

---

## alias

```bash
alias ll="ls -la"
```

Remove alias

```bash
unalias ll
```

---

## cal

```bash
cal
```

---

# 📦 Package Managers

## Ubuntu / Debian

```bash
sudo apt update
sudo apt upgrade
sudo apt install package-name
sudo apt remove package-name
```

---

## Fedora / RHEL

```bash
sudo dnf update
sudo dnf install package-name
```

---

# ⭐ Quick Reference

| Category    | Commands                   |
| ----------- | -------------------------- |
| Navigation  | ls, pwd, cd                |
| Files       | cp, mv, rm, touch          |
| Search      | find, locate, grep         |
| Archives    | zip, unzip, tar            |
| Editing     | nano, vi, cat, sed         |
| Permissions | chmod, chown               |
| Users       | sudo, su, passwd           |
| Disk        | df, du                     |
| Processes   | top, htop, ps, kill        |
| Network     | ping, curl, wget, ip       |
| System      | uname, hostname, systemctl |
| Misc        | echo, alias, history, man  |






## Basic commands

- for write anything in specific file 
```
echo "hellow world" > filename
```
- for append ( previous data will be there , new will added in it )
```
echo "hellow world" >> filename
```
- for display data
```
cat filename
```
- Opens the file for editing
```
nano filename
```
- open application
```
open -a "applicationName"
```


ls | wc -l