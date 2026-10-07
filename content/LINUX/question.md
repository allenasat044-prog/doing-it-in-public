[[LINUX/]]



from the student .txt display the student names in alphabetical order and remove  duplicant s name 

from the student.txt and display the student only contains "an" and arrange it in alphabetical order and remove  duplicant s name 
```

grep -i "an" s2.txt | sort | uniq
grep -i "an" s2.txt | sort -u | uniq
grep -i "an" s2.txt | sort -u -r | uniq
```


question 2 
create 2 file conataing student id and name and another file containing 
student id and name 
perform the following 
use paste combine the file side by side 
sort the files and use to combine date based on common student id 
create 2 sorted and use comm to identify common and unique entries.



question 3
use this sample stu.txt file for the question 
alice 21 bangalpre
bob 19 mysore
anil 25bangalore
arun 25 chennai
amit 30mumbai
priya 21 bangalore
pranav 24 pune
ravi 18 chennai
rahul 27 delhi
sneha 23 mumbai
sonia 29 bangalore


write a grep command to find lines where the name starts with a and has exactly 4 letters 
write a command to find names ending with a or i
find lines containing either an or ri 
find name start with p and end with a 
find the lines containg 2 consecutive  a character
find students whose names start with A or R
names containg exactly 4 character
student whoses age start with 2 
age b/w 20 - 19 
student age is ether 18 , 19 , 21, 22 

**1. Name starts with `a` and has exactly 4 letters**

```
grep -E '^a[a-z]{3}\b' stu.txt
```

**2. Names ending with `a` or `i`**

```
grep -E '^[a-z]+[ai]\b' stu.txt
```

**3. Lines containing either `an` or `ri`**

```
grep -E 'an|ri' stu.txt
```

**4. Name starts with `p` and ends with `a`**

```
grep -E '^p[a-z]*a\b' stu.txt
```

**5. Lines containing 2 consecutive `a` characters**

```
grep -E 'aa' stu.txt
```

**6. Students whose names start with `A` or `R`**  
Your file has lowercase names, so use:

```
grep -Ei '^[ar]' stu.txt
```

**7. Names containing exactly 4 characters**

```
grep -E '^[a-z]{4}\b' stu.txt
```

**8. Students whose age starts with `2`**

```
grep -E '^[a-z]+ 2[0-9]' stu.txt
```

**9. Age between 19–20**

```
grep -E '^[a-z]+ (19|20)\b' stu.txt
```

**10. Student age is either 18, 19, 21, or 22**

```
grep -E '^[a-z]+ (18|19|21|22)\b' stu.txt
```


<h2>New question </h2>


1. Write a command to find lines containing exactly two digits consecutively.

```
grep -E '[0-9]{2}' stu.txt
```

2. Write a command to find lines containing three or more consecutive digits.

```
grep -E '[0-9]{3,}' stu.txt
```

3. Write a command to find names containing the letter `a` at least two times.

```
grep -Ei '^[^ ]*a[^ ]*a' stu.txt
```

4. Write a command to find lines ending with exactly three digits.

```
grep -E '[0-9]{3}$' stu.txt
```

5. Write a command to find names that start with A, have exactly 4 letters, and end with either 1 or n.

```
grep -E '^A[a-z]{2}[1n]' stu.txt
```

6. Write a command to find names that start with P, contain 4-6 characters, and end with either a or v.

```
grep -E '^P[a-z]{2,4}[av]' stu.txt
```

7. Write a command to find lines where the name starts with an uppercase letter, contains at least two lowercase letters, and is followed by a two-digit age.

```
grep -E '^[A-Z][a-z]+ [0-9]{2}' stu.txt
```

8. Write a command to find students whose names start with A, P, or R and whose age starts with 2.

```
grep -E '^[APR][a-z]* 2[0-9]' stu.txt
```

9. Write a command to find lines where the name contains exactly one `o`.

```
grep -Ei '^[^ ]*o[^ o]* ' stu.txt
```

10. Write a command to find names where `a` appears somewhere before `n`.

```
grep -Ei '^[^ ]*a[^ ]*n' stu.txt
```


<h4>exp</h4> 
```

# List disks and partitions
diskutil list

# Show detailed information about a disk
diskutil info /dev/disk2

# Show disk/volume information
diskutil apfs list

# Create an APFS volume (similar purpose to creating a logical volume)
diskutil apfs addVolume disk2 APFS LVMData

# List APFS volumes
diskutil apfs list

# Create a mount directory
sudo mkdir -p /mnt/lvmdata

# Mount the volume
diskutil mount LVMData

# Check disk usage
df -h /mnt/lvmdata

# Increase APFS volume size
sudo diskutil apfs resizeContainer disk2 20G

# Show the current disk/volume sizes
diskutil apfs list
```

### Linux → macOS equivalent

|Linux command|macOS equivalent|
|---|---|
|`lsblk`|`diskutil list`|
|`pvcreate /dev/sdb`|**No direct equivalent**|
|`pvs`|`diskutil apfs list`|
|`vgcreate myvg /dev/sdb`|**No direct equivalent**|
|`vgdisplay`|`diskutil apfs list`|
|`lvcreate -L 10G -n mylv myvg`|`diskutil apfs addVolume ...`|
|`lvs`|`diskutil apfs list`|
|`mkfs.ext4`|`diskutil eraseVolume APFS ...`|
|`mkdir /mnt/lvmdata`|`sudo mkdir -p /mnt/lvmdata`|
|`mount ...`|`diskutil mount ...`|
|`df -h`|`df -h`|
|`lvextend`|`diskutil apfs resizeVolume`|
|`resize2fs`|**Not needed for APFS**|

**Important:** `/dev/sdb`, `myvg`, and `mylv` are Linux LVM concepts. On macOS, use **APFS containers and volumes** instead.
```
# Show CPU information
sysctl -n machdep.cpu.brand_string

# Show memory/RAM information
vm_stat

# Show PCI devices
system_profiler SPPCIDataType

# Show USB devices
system_profiler SPUSBDataType

# List disks and partitions
diskutil list

# Show disk/partition information
diskutil info /dev/disk2

# Show filesystem disk usage
df -h

# Show partition information
diskutil list

# Show mounted filesystems
mount

# Show detailed information about a device
diskutil info /dev/disk2
```


<h4>Exp7</h4>

```
# Create a test directory
mkdir test

# Create a backup directory
mkdir backup

# Create a test file
echo "Backup test" > test/file.txt

# Copy files using cp
cp -R test/. backup/

# Create a compressed backup using tar.gz
tar -czvf backup.tar.gz test/

# Inspect the backup archive
tar -tzvf backup.tar.gz

# Restore files from the backup archive
mkdir restore
tar -xzvf backup.tar.gz -C restore/

# Sync using rsync
rsync -av test/ backup/

# Verify backup files
ls -la backup/

# Compare files
diff test/file.txt backup/file.txt

# Open the user's crontab
crontab -e

# Create a scheduled backup (runs every day at 8 PM)
0 20 * * * rsync -av ~/test/ ~/backup/

# Display scheduled cron tasks
crontab -l

# Verify the scheduled backup
crontab -l
```




