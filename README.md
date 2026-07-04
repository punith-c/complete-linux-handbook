# 🐧 The Complete Linux Handbook

> A friendly, no-nonsense guide to understanding and using Linux — from your very first `ls` command to managing disks, users, networks, and full-blown shell scripts.

This guide was written for people who want to actually **understand** Linux, not just copy-paste commands blindly. Every section explains the *why* before the *how*, so you build real intuition instead of memorizing syntax you'll forget in a week.

Whether you're a student setting up your first VM, a developer deploying to a server, or a sysadmin brushing up on fundamentals — this handbook has you covered.

---

## 📖 Table of Contents

1. [What Is Linux?](#1-what-is-linux)
2. [The Linux Filesystem Hierarchy](#2-the-linux-filesystem-hierarchy)
3. [The Command Line — Complete Mastery](#3-the-command-line--complete-mastery)
4. [User & Group Management](#4-user--group-management)
5. [Package Management](#5-package-management-all-major-distros)
6. [Networking](#6-networking--complete-guide)
7. [Storage & Filesystems](#7-storage--filesystems)
8. [Services & Systemd](#8-services--systemd)
9. [Shell Scripting](#9-shell-scripting)
10. [System Administration](#10-system-administration)
11. [Security](#11-security)
12. [Advanced Topics](#12-advanced-topics)
13. [Troubleshooting Playbook](#13-troubleshooting-playbook)
14. [Learning Path & Next Steps](#14-learning-path--next-steps)
15. [Glossary](#15-glossary)
16. [Quick Reference Card](#16-quick-reference-card)

---

## 1. What Is Linux?

Linux isn't a single operating system — it's a **family** of operating systems, all built around one shared core: the **Linux kernel**, originally written by Linus Torvalds in 1991 as a personal project. That kernel has since grown into the backbone of most of the internet.

To put its reach in perspective:

| Where Linux Runs | Scale |
|---|---|
| 🌐 Top 1 million web servers | ~96% |
| 🖥️ Top 500 supercomputers | 100% |
| 📱 Android phones | Linux kernel under the hood |
| 📡 Routers, smart TVs, embedded devices | Everywhere |
| 💻 Desktops | Small but steadily growing |

### Why Linux Feels Different (The Philosophy)

If you're coming from Windows or macOS, Linux will feel unusual at first. That's because it follows a different design philosophy — one that rewards curiosity:

- **Everything is a file.** Hardware, running processes, even kernel settings are represented as files you can read or write.
- **Small tools, single purpose.** Instead of one giant program that does everything, Linux favors many small tools that each do one job *well*.
- **Chain tools together.** Pipes (`|`) let you connect these small tools into powerful custom workflows.
- **Text is the universal interface.** Configuration lives in plain text files — no hidden binary settings you can't inspect.
- **You're in control.** Linux rarely assumes what you want; it expects you to configure it explicitly.

### Understanding Distributions ("Distros")

A "distro" bundles the Linux kernel together with a package manager, a desktop environment, and a curated set of applications. Think of the kernel as the engine, and the distro as the entire car built around it.

| Distro | Based On | Package Manager | Best For |
|---|---|---|---|
| **Ubuntu** | Debian | `apt` | Beginners, general desktop use |
| **Debian** | — | `apt` | Rock-solid stability, servers |
| **Fedora** | — | `dnf` | Cutting-edge software, Red Hat ecosystem |
| **CentOS / RHEL** | — | `yum` / `dnf` | Enterprise servers |
| **openSUSE** | — | `zypper` | System administrators |
| **Linux Mint** | Ubuntu | `apt` | Windows users switching over |
| **Kali Linux** | Debian | `apt` | Penetration testing / security research |
| **Manjaro** | Arch | `pacman` | Arch's power with easier setup |
| **Slackware** | — | `pkgtools` | Learning Unix the "hard" (educational) way |

> 💡 **Note:** This guide focuses on commands and concepts that work across **all** distros. Wherever a command differs (like `apt` vs `dnf`), both versions are shown.

---

## 2. The Linux Filesystem Hierarchy

Unlike Windows, Linux has **no drive letters** (no `C:`, `D:`). Everything — every disk, every USB stick, every network share — is attached somewhere under a single root directory: `/`.

Think of it like a tree with one trunk. Every branch, no matter which physical disk it actually lives on, is reachable by following a path down from `/`.

```
/                              → Root of everything
├── bin → /usr/bin             → Essential command binaries (symlink on modern systems)
├── boot                       → Boot loader, kernel, initramfs
│   ├── vmlinuz-linux          → The Linux kernel itself
│   ├── initramfs-linux.img    → Initial RAM filesystem
│   └── grub/                  → GRUB bootloader configuration
├── dev                        → Device files (hardware interfaces)
│   ├── sda                    → First SATA disk
│   ├── sda1                   → First partition on first disk
│   ├── tty0                   → Terminal device
│   ├── null                   → Discards all data written to it
│   ├── zero                   → Produces an endless stream of null bytes
│   └── random                 → Random number generator
├── etc                        → System-wide configuration files
│   ├── passwd                 → User accounts
│   ├── shadow                 → Encrypted passwords
│   ├── group                  → User groups
│   ├── fstab                  → Filesystem mount table
│   ├── hosts                  → Hostname-to-IP mappings
│   ├── hostname                → System hostname
│   ├── resolv.conf            → DNS servers
│   ├── ssh/                   → SSH server configuration
│   ├── nginx/                 → Nginx web server config
│   ├── systemd/               → Systemd unit files
│   ├── cron.d/                → Scheduled cron jobs
│   ├── sudoers                → Sudo permission rules
│   └── apt/ (Ubuntu/Debian)   → APT package manager config
├── home                       → User home directories
│   └── username/
│       ├── Documents/
│       ├── Downloads/
│       ├── .bashrc            → Bash configuration
│       ├── .profile           → Login shell configuration
│       ├── .ssh/               → SSH keys
│       └── .local/            → User-installed local programs
├── lib → /usr/lib             → Shared libraries (symlink)
├── media                      → Removable media mount points
│   └── username/
│       ├── CDROM/
│       └── USB_DRIVE/
├── mnt                        → Temporary manual mount points
├── opt                        → Optional / third-party software
│   └── google-chrome/         → Example: Chrome installs here
├── proc                       → Virtual filesystem exposing running processes
│   ├── cpuinfo                → CPU information
│   ├── meminfo                → Memory information
│   ├── uptime                 → System uptime
│   └── 1234/                  → Info for process #1234
├── root                       → The root user's home directory
├── run                        → Runtime variable data (in-memory)
│   └── user/1000/             → Per-user runtime directory
├── sbin → /usr/sbin           → System binaries (usually root-only)
├── srv                        → Data for services (HTTP, FTP servers)
├── sys                        → Virtual filesystem exposing kernel info
├── tmp                        → Temporary files (wiped on reboot)
├── usr                        → User system resources ("the second root")
│   ├── bin/                   → Most user commands live here
│   ├── lib/                   → Libraries
│   ├── local/                 → Locally compiled/installed software
│   ├── share/                 → Shared data: man pages, icons, docs
│   └── src/                   → Source code (sometimes)
└── var                        → Data that changes as the system runs
    ├── log/                   → Log files
    │   ├── syslog             → System log
    │   ├── auth.log           → Authentication logs
    │   ├── kern.log           → Kernel logs
    │   └── nginx/             → Web server access/error logs
    ├── cache/                 → Application cache data
    ├── lib/                   → Dynamic application state
    ├── spool/                 → Print/email queues
    ├── tmp/                   → Temp files that persist across reboots
    └── www/                   → Web server document root (common)
```

**A simple way to remember it:** `/etc` is config, `/var` is stuff that changes, `/home` is your personal space, `/usr` is where programs actually live, and `/dev` and `/proc` are windows into the hardware and kernel itself.

### Understanding Mount Points

Because there are no drive letters, every disk (internal, external, or network) has to be "mounted" — attached — to some folder in this tree before you can access it.

```bash
# ── SEE WHAT'S MOUNTED ──
df -h
# Filesystem      Size  Used Avail Use% Mounted on
# /dev/sda1       234G   45G  189G  19% /          ← Root filesystem
# /dev/sdb1       1.8T  300G  1.5T  17% /data      ← Second hard drive
# /dev/sdc1       120G   80G   40G  67% /mnt/backup ← External drive
tmpfs             16G  2.1G   14G  13% /tmp        ← RAM-based temp storage

# ── MOUNT COMMANDS ──
mount                              # Show all currently mounted filesystems
mount /dev/sdb1 /mnt/data          # Attach a partition to a folder
mount -t ntfs /dev/sdc1 /mnt/usb   # Mount an NTFS-formatted USB drive
umount /mnt/data                   # Detach it
umount -l /mnt/data                # "Lazy" unmount, for when the device is busy

# ── FIND A DRIVE'S UUID (its unique fingerprint) ──
blkid
# /dev/sda1: UUID="abc123-..." TYPE="ext4"
ls -l /dev/disk/by-uuid/           # Same info, shown as symlinks

# ── /etc/fstab — mounts that persist across reboots ──
cat /etc/fstab
# Format:
# <device>    <mountpoint>    <fstype>    <options>    <dump>    <pass>
UUID=abc123   /               ext4        defaults     0         1
UUID=def456   /boot           vfat        defaults     0         2
UUID=ghi789   swap            swap        defaults     0         0
//192.168.1.5/shared  /mnt/nfs  nfs  defaults  0  0
```

> ⚠️ **Tip:** Always back up `/etc/fstab` before editing it. A typo here can prevent your system from booting.


---

## 3. The Command Line — Complete Mastery

### 3.1 What Exactly Is "The Shell"?

The **shell** is the program that sits between you and the operating system — it reads what you type, interprets it, and tells the kernel what to do. The most common shell is **Bash** (Bourne Again SHell), but others exist too.

```bash
# Your prompt structure:
username@hostname:current_directory$
# ^user    ^computer  ^where you are    ^ $ = normal user, # = root

# Shell types:
echo $SHELL             # Your current shell
cat /etc/shells         # Shells available on this system
# /bin/bash
# /bin/zsh
# /bin/fish
# /bin/dash
# /bin/sh

# Switch your default shell:
chsh -s /bin/zsh        # Switch to Z shell
chsh -s /bin/fish       # Switch to Fish shell
```

### 3.2 Navigation — Finding Your Way Around

These are the commands you'll type hundreds of times a day, so they're worth knowing cold.

```bash
pwd                     # Print Working Directory — shows where you currently are
                        # Output: /home/username/Documents

ls                      # List what's in the current directory
ls -l                   # Long format: permissions, owner, size, date
ls -a                   # ALL files, including hidden ones (starting with .)
ls -la                  # Combined: long format + hidden files
ls -lh                  # Human-readable file sizes (K, M, G instead of raw bytes)
ls -lS                  # Sort by size, largest first
ls -lt                  # Sort by modification time, newest first
ls -lr                  # Reverse whatever sort order is active
ls -R                   # Recursive — show subdirectories too
ls -d */                # Show only directories

cd /path/to/dir         # Change directory
cd ~                    # Jump to your home directory
cd                      # Same as cd ~ (shorthand)
cd -                    # Go back to the PREVIOUS directory you were in
cd ..                   # Move up to the parent directory
cd ../..                # Move up two levels
cd /                    # Jump to the root directory

tree                    # Show directory structure as a visual tree
                        # (install first: sudo apt install tree)
tree -L 2               # Limit depth to 2 levels
tree -d                 # Directories only, no files
tree -a                 # Include hidden files
tree -h                 # Human-readable sizes
```

### 3.3 Viewing & Editing Files

```bash
# ── VIEWING ──
cat file.txt                    # Dump the entire file to the terminal
cat -n file.txt                 # Same, but with line numbers

less file.txt                   # Page through a file (q=quit, /=search, g=top, G=bottom)
more file.txt                   # An older pager — less is generally better

head file.txt                   # Show the first 10 lines
head -n 50 file.txt             # Show the first 50 lines

tail file.txt                   # Show the last 10 lines
tail -n 100 file.txt            # Show the last 100 lines
tail -f log.txt                 # "Follow" mode — watch a file update live (Ctrl+C to stop)
tail -f -n 50 log.txt           # Start with the last 50 lines, then follow

nl file.txt                     # Number every line

od -c file.txt                  # Octal dump — see the raw bytes
od -x file.txt                  # Hex dump

xxd file.txt                    # Another hex dump tool

# ── EDITING ──
nano file.txt                   # Beginner-friendly editor (Ctrl+O to save, Ctrl+X to exit)
vim file.txt                    # Modal editor (i = insert mode, Esc = normal mode, :wq = save & quit)
emacs file.txt                  # Emacs editor
code file.txt                   # Opens in VS Code, if installed
gedit file.txt                  # A GUI text editor (GNOME)
```

> 💡 **New to terminal editors?** Start with `nano` — it shows you the keyboard shortcuts right on screen. `vim` is more powerful but has a steeper learning curve (fun fact: half of all "how do I exit vim" internet searches exist for a reason 😄).

### 3.4 Creating, Copying, Moving & Deleting

```bash
# ── CREATE ──
touch file.txt                  # Create an empty file (or update its timestamp if it exists)
touch file{1,2,3}.txt           # Creates file1.txt, file2.txt, file3.txt
touch {a,b,c}.txt               # Creates a.txt, b.txt, c.txt
touch {1..10}.txt               # Creates 1.txt through 10.txt

mkdir directory                 # Create a new directory
mkdir -p a/b/c/d                # Create nested directories in one shot
mkdir -p project/{src,bin,docs,test}  # Create several subfolders at once

echo "hello" > file.txt         # Write "hello" into file.txt (OVERWRITES existing content)
echo "hello" >> file.txt        # Append "hello" to file.txt (keeps existing content)

# ── COPY ──
cp source.txt dest.txt          # Copy a file
cp -i source.txt dest.txt       # Ask before overwriting
cp -r source_dir/ dest_dir/     # Copy a directory RECURSIVELY (needed for folders)
cp -a source_dir/ dest_dir/     # "Archive" copy — preserves permissions, ownership, timestamps
cp -u source.txt dest.txt       # Only copy if the source is newer

# ── MOVE / RENAME ──
mv old.txt new.txt              # Rename a file (mv is also how you rename in Linux!)
mv file.txt /target/dir/        # Move a file into a directory
mv -i file.txt target/          # Ask before overwriting
mv -u file.txt target/          # Only move if source is newer
mv *.txt target/                # Move every .txt file at once

# ── DELETE ──
rm file.txt                     # Delete a file — PERMANENT, there's no recycle bin
rm -i file.txt                  # Ask for confirmation first
rm -f file.txt                  # Force delete, no questions asked
rm -r directory/                # Delete a directory and everything inside it
rm -rf directory/               # Force + recursive (use with real caution)
rm -rf *                        # Deletes EVERYTHING in the current directory
rm -rf /                        # 🚨 Deletes your ENTIRE system. NEVER run this.
```

> ⚠️ **A word of caution:** Linux doesn't ask "are you sure?" the way graphical file managers do. There's no undo for `rm`. Always double-check your path before hitting Enter, especially with `-rf`.

### 3.5 File Permissions — Understanding Ownership

Every file on Linux has three things attached to it: an **owner**, a **group**, and a set of **permissions** describing what the owner, the group, and everyone else are allowed to do with it.

```bash
ls -l
# -rwxr-xr-x  1  alice  developers  4096  Jun 15 10:00  script.sh
# ^^^^^^ ^^^^  ^  ^^^^^  ^^^^^^^^^^
# |      |||   |  owner  group
# |      ||others (r-x)
# |      |group (r-x)
# |owner (rwx)
# file type (- = file, d = directory, l = symlink, c = character device, b = block device)
```

Each permission has a numeric value:

```
r = 4  (read)
w = 2  (write)
x = 1  (execute)

Common combinations:
7 = rwx (4+2+1)  — read, write, execute
6 = rw- (4+2)    — read, write
5 = r-x (4+1)    — read, execute
4 = r-- (4)      — read only
0 = --- (0)      — no permissions
```

```bash
# ── CHANGING PERMISSIONS (chmod) ──

# Symbolic mode — adjust permissions relatively:
chmod u+x file.sh               # Add execute for the owner ("user")
chmod g+w file.txt              # Add write for the group
chmod o-r file.txt              # Remove read for others
chmod a+x file.sh               # Add execute for everyone
chmod u=rwx,g=rx,o=r file.sh    # Set exact permissions for each
chmod a=rx file.sh              # Everyone gets read + execute
chmod go-w file.txt             # Remove write from group AND others

# Numeric mode — set permissions absolutely:
chmod 755 script.sh             # rwxr-xr-x — owner: full, group/others: read+execute
chmod 644 file.txt              # rw-r--r-- — a typical "normal file" permission
chmod 600 private.txt           # rw------- — only the owner can read/write
chmod 700 private_dir/          # rwx------ — only the owner can enter
chmod 777 file.sh               # rwxrwxrwx — anyone can do anything (generally INSECURE)
chmod 400 key.pem               # r-------- — read-only for owner (typical for SSH keys)

# ── CHANGING OWNERSHIP (chown) ──
sudo chown alice file.txt               # Change the owner
sudo chown alice:developers file.txt    # Change owner and group together
sudo chown :developers file.txt         # Change just the group
sudo chown -R alice:alice /home/alice   # Recursively change an entire folder tree

# ── SPECIAL PERMISSIONS ──
chmod u+s script.sh             # SUID — the script runs as its owner, not the caller
chmod g+s directory/            # SGID — new files inherit the directory's group
chmod +t directory/             # Sticky bit — only file owners can delete their own files
                                 # (this is how /tmp stays safe on shared systems)
chmod 4755 script.sh            # Numeric SUID (4 = SUID, 755 = permissions)
chmod 2775 directory/           # Numeric SGID (2 = SGID)
chmod 1777 /tmp                 # Numeric sticky bit (1 = sticky)
```

### 3.6 Finding Things

```bash
# ── FIND FILES (find) ──
find /home -name "*.txt"                # All .txt files under /home
find /home -iname "*.TXT"               # Case-insensitive version
find / -name "config" -type f           # Files (not folders) named "config"
find / -name "config" -type d           # Directories named "config"
find . -size +100M                      # Files larger than 100MB
find . -size -1k                        # Files smaller than 1KB
find . -size 1024k                      # Files that are exactly 1MB
find . -mtime -7                        # Modified in the last 7 days
find . -mmin -60                        # Modified in the last 60 minutes
find . -atime +30                       # Accessed more than 30 days ago
find . -perm 644                        # Files with exactly permission 644
find . -perm -4000                      # SUID files (worth auditing for security)
find . -empty                           # Empty files and directories
find . -maxdepth 2 -name "*.conf"       # Limit how deep the search goes

# Execute a command on every result:
find . -name "*.log" -exec rm {} \;     # Delete every .log file found
find . -name "*.py" -exec wc -l {} +    # Count lines across every Python file
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;

# A faster modern alternative:
sudo apt install fd-find        # Debian/Ubuntu
sudo dnf install fd-find        # Fedora
fd "*.txt"                      # Equivalent to: find . -name "*.txt"
fd -e pdf                       # All PDF files
fd -t d "config"                # Directories named "config"
fd -x wc -l                     # Run a command on every result

# ── SEARCH INSIDE FILES (grep) ──
grep "pattern" file.txt                 # Find lines containing "pattern"
grep -i "pattern" file.txt              # Case-insensitive
grep -r "password" /etc/                # Search recursively through a directory
grep -r -l "TODO" ~/projects/           # Show only the FILENAMES that match
grep -n "error" log.txt                 # Show line numbers alongside matches
grep -v "exclude" file.txt              # Invert match — show lines WITHOUT the pattern
grep -c "error" log.txt                 # Count how many lines match
grep -w "word" file.txt                 # Match whole words only
grep -o "pattern" file.txt              # Print only the matched part of each line
grep -A 5 "error" log.txt               # Show 5 lines AFTER each match
grep -B 3 "error" log.txt               # Show 3 lines BEFORE each match
grep -C 2 "error" log.txt               # Show 2 lines both before AND after

# grep with regular expressions:
grep "^error" log.txt                   # Lines STARTING with "error"
grep "error$" log.txt                   # Lines ENDING with "error"
grep "^[A-Z]" file.txt                  # Lines starting with a capital letter
grep "err[0o]r" file.txt                # Matches "error" or "err0r"
grep -E "error|warning|fail" log.txt    # Extended regex — match any of several patterns
grep -P "\d{3}-\d{3}-\d{4}" file.txt    # Perl regex — e.g. phone numbers
grep "[0-9]\{3\}\.[0-9]\{3\}\.[0-9]\{3\}" file.txt  # IP-address-shaped patterns

# Practical everyday examples:
ps aux | grep firefox                   # Is Firefox running?
history | grep ssh                      # Find past SSH commands you've run
cat /var/log/syslog | grep -i "error"   # Scan the system log for errors
find /etc -name "*.conf" | xargs grep "Listen"  # Find "Listen" across every config file
```

### 3.7 Text Processing Power Tools

These tools look intimidating at first, but they're what make the Linux command line genuinely powerful for working with data — logs, CSVs, config files, anything text-based.

```bash
# ── SORT ──
sort file.txt                           # Alphabetical sort
sort -n file.txt                        # Numeric sort
sort -r file.txt                        # Reverse order
sort -k2 file.txt                       # Sort by the 2nd column
sort -t, -k2 -n data.csv                # Sort a CSV by column 2, numerically
sort -u file.txt                        # Sort and remove duplicates

# ── UNIQ (only works well on sorted input) ──
sort file.txt | uniq                    # Remove consecutive duplicate lines
sort file.txt | uniq -c                 # Count how many times each line appears
sort file.txt | uniq -d                 # Show ONLY lines that are duplicated
sort file.txt | uniq -u                 # Show ONLY lines that appear once

# ── CUT (grab specific columns) ──
cut -d',' -f1,3 data.csv                # Columns 1 and 3 from a CSV
cut -d: -f1 /etc/passwd                 # Just the usernames from /etc/passwd
cut -c1-10 file.txt                     # First 10 characters of every line
cut -c20-30 file.txt                    # Characters 20 through 30

# ── PASTE (merge files side by side) ──
paste file1.txt file2.txt               # Merge lines, tab-separated
paste -d',' file1.txt file2.txt         # Comma-separated
paste -d'|' file1.txt file2.txt         # Pipe-separated

# ── JOIN (SQL-style merge on a shared column) ──
join -t, -1 1 -2 1 file1.csv file2.csv

# ── WC (word count) ──
wc file.txt                             # Lines, words, and characters
wc -l file.txt                          # Just line count
wc -w file.txt                          # Just word count
wc -c file.txt                          # Byte count
wc -m file.txt                          # Character count (safe with multibyte text)

# ── TR (translate or delete characters) ──
echo "HELLO" | tr 'A-Z' 'a-z'           # → hello
echo "hello world" | tr ' ' '\n'        # Replace spaces with newlines
echo "a,b,c" | tr ',' '\t'              # Commas → tabs
cat file.txt | tr -d '\r'               # Strip Windows-style line endings
cat file.txt | tr -s ' '                # Collapse repeated spaces into one
cat file.txt | tr '[:space:]' '\n'      # Split on any whitespace
cat file.txt | tr '[:lower:]' '[:upper:]'  # Uppercase everything

# ── SED (the "stream editor" — find & replace at scale) ──
sed 's/old/new/' file.txt               # Replace the FIRST match on each line
sed 's/old/new/g' file.txt              # Replace ALL matches (global)
sed 's/old/new/gi' file.txt             # Case-insensitive, global
sed 's/old/new/' file.txt > new.txt     # Save the result to a new file
sed -i 's/old/new/g' file.txt           # Edit the file IN PLACE
sed -i.bak 's/old/new/g' file.txt       # Edit in place, keeping a .bak backup

# Line selection:
sed -n '5,10p' file.txt                 # Print only lines 5–10
sed '5,10d' file.txt                    # Delete lines 5–10
sed -n '/pattern/p' file.txt            # Print only lines matching a pattern
sed '/pattern/d' file.txt               # Delete lines matching a pattern

# Advanced sed:
sed 's/^/PREFIX: /' file.txt            # Add "PREFIX: " to the start of every line
sed 's/$/ :SUFFIX/' file.txt            # Add " :SUFFIX" to the end of every line
sed '/^$/d' file.txt                    # Delete blank lines
sed 's/  */ /g' file.txt                # Collapse multiple spaces into one
sed 's/<[^>]*>//g' file.html            # Strip HTML tags

# ── AWK (a full text-processing language in one command) ──
awk '{print $1}' file.txt               # Print the first "field" (word) of each line
awk '{print $1, $3}' file.txt           # Print fields 1 and 3
awk '{print NF}' file.txt               # Number of fields per line
awk '{print NR, $0}' file.txt           # Line number + the whole line
awk -F',' '{print $1}' data.csv         # Use comma as the field separator
awk -F: '{print $1}' /etc/passwd        # Usernames, from a colon-delimited file

# Pattern matching:
awk '/error/ {print}' log.txt           # Print lines containing "error"
awk '/error/,/end/' log.txt             # Print everything between "error" and "end"
awk '$2 > 100 {print}' data.txt         # Print lines where column 2 is greater than 100
awk '$3 ~ /pattern/ {print}' data.txt   # Print if column 3 matches a pattern

# BEGIN / END blocks (run once at start / end):
awk 'BEGIN {print "START"} {print $0} END {print "END"}' file.txt
awk '{sum+=$1} END {print "Total:", sum}' numbers.txt   # Sum a column
awk '{count++} END {print "Count:", count}' file.txt    # Count lines

# Calculations:
awk '{sum+=$1; count++} END {print "Avg:", sum/count}' numbers.txt
awk 'NR==1 {max=$1} $1 > max {max=$1} END {print "Max:", max}' numbers.txt

# Formatted output:
awk '{printf "%-20s %5d\n", $1, $2}' data.txt

# ── TEE (write to a file AND the screen simultaneously) ──
command | tee output.txt                 # Save to file, still see it on screen
command | tee -a output.txt              # Append to file instead of overwrite
command | tee output.txt | grep pattern  # Save everything, but only show filtered results

# ── XARGS (turn piped input into command arguments) ──
find . -name "*.txt" | xargs rm                  # Delete every .txt file found
find . -name "*.log" | xargs grep -l "error"     # Which log files mention "error"?
cat urls.txt | xargs curl -O                     # Download every URL in a list
cat hosts.txt | xargs -I{} ping -c 1 {}          # Ping each host in a list
xargs -n1 -a commands.txt                        # Run each line of a file as a command
xargs -P4 -a urls.txt curl -O                    # Run 4 downloads in parallel
```

### 3.8 Pipes & Redirection — Linux's Secret Sauce

If there's one concept that makes the command line click, it's this: **the output of one command can become the input of another.** This is what turns a handful of small tools into an infinitely flexible toolkit.

```bash
# ── OUTPUT REDIRECTION ──
command > file              # Send stdout to a file (OVERWRITES it)
command >> file             # Send stdout to a file (APPENDS to it)
command 2> file             # Send stderr (errors) to a file
command 2>> file            # Append stderr to a file
command &> file             # Send BOTH stdout and stderr to a file
command &>> file            # Append both to a file
command > file 2>&1         # stdout to file, then redirect stderr to the same place
command 2>&1 | another      # Redirect stderr into stdout, then pipe both onward

# ── INPUT REDIRECTION ──
command < file              # Feed a file's contents in as stdin
command << delimiter        # "Here-document" — type multi-line input directly
> line 1
> line 2
> delimiter

command <<< "string"        # "Here-string" — pass a single string as stdin

# ── PIPES (|) ──
command1 | command2         # command1's output becomes command2's input

# Real-world combinations:
ps aux | grep firefox                                               # Is Firefox running?
cat /var/log/syslog | grep "ERROR" | tail -50                       # Last 50 error lines
history | awk '{print $2}' | sort | uniq -c | sort -rn | head -20   # Your 20 most-used commands
ls -la | sort -k5 -n                                                # Files sorted by size
cat file.txt | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head    # Word frequency count

# Pipe BOTH stdout and stderr:
command |& grep error

# ── DISCARDING OUTPUT ──
command > /dev/null            # Throw away stdout
command 2> /dev/null           # Throw away stderr
command &> /dev/null           # Throw away everything

# ── /dev/null — the "black hole" of Linux ──
find / -name "*.conf" 2>/dev/null       # Search everywhere, hide permission errors
curl -s https://site.com > /dev/null    # Just check if a site responds, no output
yes | apt-get install package           # Auto-answer "yes" to every prompt
```

### 3.9 Process Management

```bash
# ── VIEWING PROCESSES ──
ps                          # Processes in your current shell only
ps aux                      # ALL processes on the system (BSD-style output)
ps -ef                      # ALL processes (standard/System V style output)
ps aux --sort=-%cpu         # Sorted by CPU usage, highest first
ps aux --sort=-%mem         # Sorted by memory usage, highest first
ps aux | wc -l              # Count total running processes
ps aux | grep username      # Only this user's processes
ps -u username              # Same thing, cleaner syntax

pstree                      # Tree view showing parent/child process relationships
pstree -p                   # Same, with process IDs shown
pstree username             # Tree view for a specific user

# ── INTERACTIVE PROCESS VIEWERS ──
top                         # The classic built-in process viewer (press q to quit)
htop                        # A much nicer version (install: apt install htop)
btm                         # "Bottom" — modern, Rust-based
glances                     # Full system overview in one screen

# Inside htop:
# F3 = search   F4 = filter   F5 = tree view   F6 = sort   F9 = kill
# Space = select a process    u = filter by user

# ── SENDING SIGNALS (kill) ──
kill PID                    # Politely ask a process to stop (SIGTERM)
kill -15 PID                # Same as above, explicit signal number
kill -9 PID                 # Force kill — the process can't ignore this
kill -1 PID                 # SIGHUP — tell a process to reload its config
kill -2 PID                 # SIGINT — same as pressing Ctrl+C
kill -3 PID                 # SIGQUIT — quit and dump core (for debugging)
kill -19 PID                # SIGSTOP — pause a process
kill -18 PID                # SIGCONT — resume a paused process

# Find and kill by name instead of PID:
kill $(pgrep firefox)       # Kill by process name
pkill firefox               # Simpler way to do the same thing
pkill -f "pattern"          # Kill anything matching a pattern in the full command line
killall firefox             # Kill every process named "firefox"

# ── PROCESS PRIORITY (nice/renice) ──
nice -n 19 command           # Run with the LOWEST priority (nice, polite to others)
nice -n -20 command          # Run with the HIGHEST priority (needs root)
renice -n 5 -p 1234          # Change the priority of an already-running process

# ── BACKGROUND / FOREGROUND JOBS ──
command &                    # Start a command running in the background
Ctrl+Z                       # Pause (suspend) whatever's running in the foreground
bg                           # Resume a suspended job, but in the background
fg                           # Bring a background job back to the foreground
jobs                         # List your current background jobs
jobs -l                      # Same, with PIDs shown
kill %1                      # Kill background job #1
fg %2                        # Bring job #2 to the foreground
disown %1                    # Detach a job so closing the terminal won't kill it

# ── KEEPING PROCESSES ALIVE AFTER LOGOUT ──
nohup command &                     # Keeps running even if you log out
nohup command > output.log 2>&1 &   # Same, but also logs output to a file

# ── TERMINAL MULTIPLEXERS (screen / tmux) ──
screen -S work              # Start a named session
Ctrl+A D                    # Detach from it (it keeps running)
screen -ls                  # List existing sessions
screen -r work              # Reattach to it

tmux new -s work            # Start a named tmux session
tmux ls                     # List sessions
tmux attach -t work         # Reattach to a session
```

> 💡 **Why this matters:** `screen` and `tmux` are lifesavers when working on a remote server over SSH. If your connection drops, anything running inside a detached session keeps going — you just reconnect and pick up where you left off.

### 3.10 Disk Usage & Management

```bash
# ── DISK USAGE ──
df -h                        # Free/used space on all mounted drives (human-readable)
df -h /                      # Just the root partition
df -h --total                # Include a grand total
df -h -t ext4                # Only show ext4 filesystems

du -sh /home/username/       # Total size of a directory
du -sh *                     # Size of each item in the current directory
du -sh * | sort -h           # Same, sorted smallest to largest
du -h --max-depth=1 /home    # One level deep only
du -ah /tmp | sort -rh | head -20  # The 20 largest files in /tmp

# Hunting down what's eating your disk:
ncdu                         # Interactive, visual disk usage explorer
sudo du -x / | sort -rn | head -20  # 20 largest directories from root

# ── DISK CHECKING ──
sudo fdisk -l                # List every disk and its partitions
sudo fdisk -l /dev/sda       # Just one specific disk

lsblk                        # Tree view of all block devices
lsblk -f                     # Same, plus filesystem info
lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT  # Pick exactly which columns to show

sudo blkid                   # UUID and filesystem type for every device

# ── CHECK FILESYSTEM HEALTH ──
sudo fsck /dev/sda1          # Check and repair a filesystem (unmount it first!)
sudo fsck -f /dev/sda1       # Force a check even if it looks clean
sudo fsck -n /dev/sda1       # Read-only check, no repairs made

# ── DISK PERFORMANCE ──
sudo hdparm -t /dev/sda      # Read speed test
sudo hdparm -T /dev/sda      # Cache speed test
dd if=/dev/zero of=test bs=1M count=100 conv=fdatasync  # Write speed test
```


---

## 4. User & Group Management

Linux was built from day one to be multi-user, which means it has a robust system for managing who can do what. Understanding this is essential if you're running a server that more than one person touches.

### 4.1 User Accounts

```bash
# ── VIEW USER INFO ──
whoami                       # Your current username
id                            # Your user ID, group ID, and group memberships
id -u                         # Just your UID (user ID number)
id -g                         # Just your primary group ID
id -Gn                        # Names of every group you belong to

who                           # Who's currently logged in
w                             # Who's logged in, and what they're doing
users                         # Just the usernames of logged-in users
last                          # Login history (reads from /var/log/wtmp)
lastlog                       # When each user last logged in

# ── CREATING USERS ──
sudo useradd username                    # Create a user (no home directory by default!)
sudo useradd -m username                 # Create a user WITH a home directory
sudo useradd -m -s /bin/bash username    # ...and set their shell
sudo useradd -m -G sudo,developers username  # ...and add them to groups
sudo useradd -m -u 1500 username         # ...with a specific user ID

# A complete, realistic example:
sudo useradd -m -s /bin/bash -G sudo,developers,docker -c "John Doe" johndoe
sudo passwd johndoe

# ── MODIFYING USERS ──
sudo usermod -aG docker username         # Add to a group (always use -a with -G!)
sudo usermod -g newgroup username        # Change primary group
sudo usermod -l newname username         # Rename the login
sudo usermod -d /new/home username       # Change home directory
sudo usermod -s /bin/zsh username        # Change default shell
sudo usermod -L username                 # Lock the account (disable login)
sudo usermod -U username                 # Unlock it again

# ── DELETING USERS ──
sudo userdel username                    # Delete the user (keeps their home folder)
sudo userdel -r username                 # Delete the user AND their home directory

# ── PASSWORD MANAGEMENT ──
passwd                                   # Change your own password
sudo passwd username                     # Change someone else's password
sudo passwd -l username                  # Lock a password
sudo passwd -u username                  # Unlock a password
sudo passwd -d username                  # Remove the password entirely (passwordless login)
sudo passwd -S username                  # Check password status
sudo chage -l username                   # See password aging/expiry info
sudo chage -M 90 username                # Force password expiry every 90 days
sudo chage -E 2025-12-31 username        # Set an account expiry date
```

> ⚠️ **Common gotcha:** Always add `-a` when using `usermod -G`. Without it, you'll *replace* all of a user's existing group memberships instead of adding a new one.

### 4.2 Group Management

```bash
# ── VIEWING GROUPS ──
groups username                  # Groups a specific user belongs to
getent group                     # Every group on the system
cat /etc/group                   # The raw group file
cat /etc/group | cut -d: -f1     # Just the group names

# ── CREATING / MODIFYING GROUPS ──
sudo groupadd developers          # Create a new group
sudo groupadd -g 1500 developers  # Create with a specific group ID
sudo groupmod -n newname oldname  # Rename a group
sudo groupmod -g 1600 developers  # Change a group's ID
sudo groupdel developers          # Delete a group

# ── ADDING / REMOVING USERS FROM GROUPS ──
sudo gpasswd -a username docker   # Add a user to a group
sudo gpasswd -d username docker   # Remove a user from a group
sudo usermod -aG docker username  # Alternative way to add
usermod -G "" username            # Strip ALL supplementary group memberships
```

### 4.3 The `sudo` System

`sudo` lets a regular user temporarily act with root (administrator) privileges — a much safer alternative to logging in as root all the time.

```bash
# ── BASIC USAGE ──
sudo command                     # Run a single command as root
sudo -u username command         # Run a command as a different, specific user
sudo -i                          # Start a full root login shell
sudo -s                          # Start a root shell in your current directory
sudo -l                          # See what sudo privileges you actually have

# ── EDITING SUDOERS SAFELY ──
sudo visudo                      # Always use this instead of editing the file directly —
                                  # it checks your syntax before saving, preventing lockouts

# Common sudoers entries:
# username     ALL=(ALL:ALL) ALL                        # Full sudo access
# %wheel       ALL=(ALL:ALL) ALL                         # Everyone in the "wheel" group
# %admin       ALL=(ALL:ALL) ALL                         # Everyone in the "admin" group
# username     ALL=(ALL) NOPASSWD: ALL                   # No password prompt
# username     ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl  # Only specific commands
# %developers  ALL=(ALL) /usr/bin/systemctl restart *    # A pattern-matched command

# ── /etc/sudoers.d/ — cleaner, drop-in rule files ──
echo "username ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/username
sudo chmod 440 /etc/sudoers.d/username
```

---

## 5. Package Management (All Major Distros)

Every distro has its own way of installing, updating, and removing software — but they all solve the same problem: fetching software from trusted repositories and tracking dependencies for you, so you're not manually chasing down libraries.

### 5.1 Debian / Ubuntu (`apt`)

```bash
# ── KEEPING THE SYSTEM UP TO DATE ──
sudo apt update                        # Refresh the list of available packages
sudo apt upgrade                       # Upgrade everything installed
sudo apt full-upgrade                  # Upgrade, resolving dependency changes too
sudo apt autoremove                    # Clean up packages nothing depends on anymore
sudo apt clean                         # Clear the downloaded .deb file cache

# ── SEARCHING & INSTALLING ──
apt search package-name               # Search for a package by name/description
apt show package-name                  # See details about a package
sudo apt install package-name          # Install it
sudo apt install package1 package2     # Install several at once
sudo apt install ./file.deb            # Install a downloaded .deb file
sudo apt install package-name=1.2.3    # Install a specific version

# ── REMOVING ──
sudo apt remove package-name           # Remove the package, keep its config files
sudo apt purge package-name            # Remove the package AND its config
sudo apt autoremove                    # Clean up now-unneeded dependencies

# ── LISTING WHAT'S INSTALLED ──
apt list --installed                   # Everything currently installed
apt list --upgradable                  # Everything with an update available
apt list "python*"                     # Anything matching a name pattern
dpkg -l                                # Another way to list installed packages
dpkg -L package-name                   # Which files a package installed
dpkg -S /path/to/file                  # Which package owns this file?
dpkg --get-selections | wc -l          # Count total installed packages

# ── FIXING A BROKEN SYSTEM ──
sudo dpkg --configure -a               # Fix half-configured installs
sudo apt --fix-broken install          # Fix broken dependency chains
```

### 5.2 Red Hat / Fedora (`dnf` / `yum`)

```bash
sudo dnf check-update                  # Check for available updates
sudo dnf upgrade                       # Install all updates
sudo dnf install package-name          # Install a package
sudo dnf remove package-name           # Remove a package
sudo dnf autoremove                    # Clean up orphaned dependencies
sudo dnf search keyword                # Search for packages
sudo dnf info package-name             # Package details
sudo dnf list installed                # Everything installed
sudo dnf list available                # Everything available to install
sudo dnf provides /path/to/file        # Which package owns this file?
sudo dnf history                       # See past package transactions
sudo dnf history undo 5                # Undo transaction #5
sudo dnf groupinstall "Development Tools"  # Install a whole toolset at once
sudo dnf reinstall package-name        # Reinstall a package
sudo dnf clean all                     # Clear the cache
```

### 5.3 openSUSE (`zypper`)

```bash
sudo zypper refresh                    # Refresh repository metadata
sudo zypper update                     # Update everything
sudo zypper install package            # Install a package
sudo zypper remove package             # Remove a package
sudo zypper search keyword             # Search
sudo zypper info package               # Package details
sudo zypper packages --installed-only  # List installed packages
sudo zypper ps                         # Processes still using deleted files
```

### 5.4 Universal Formats: Snap & Flatpak

Beyond distro-specific package managers, two cross-distro formats have become popular — useful when an app isn't in your distro's default repositories.

```bash
# ── SNAP (originally from Canonical/Ubuntu) ──
sudo snap install package
sudo snap remove package
sudo snap list
snap find keyword
snap refresh                          # Update all installed snaps
snap changes                          # See recent snap activity

# ── FLATPAK (cross-distro, sandboxed apps) ──
flatpak install flathub package
flatpak remove package
flatpak list
flatpak search keyword
flatpak update
flatpak run com.company.App
```


---

## 6. Networking — Complete Guide

### 6.1 Network Configuration

```bash
# ── VIEWING INTERFACES ──
ip addr                             # All network interfaces and their IPs (replaces old ifconfig)
ip -br addr                         # A shorter, cleaner version
ip link                             # Interfaces and their up/down state
ip route                            # Routing table (replaces old route -n)
ip neigh                            # ARP table — devices on your local network

# ── CONFIGURING INTERFACES ──
sudo ip link set eth0 up            # Bring an interface up
sudo ip link set eth0 down          # Take it down
sudo ip addr add 192.168.1.100/24 dev eth0  # Assign a static IP
sudo ip addr del 192.168.1.100/24 dev eth0  # Remove that IP
sudo ip route add default via 192.168.1.1   # Set the default gateway

# ── DNS ──
cat /etc/resolv.conf                # Which DNS servers you're using
# nameserver 8.8.8.8
# nameserver 1.1.1.1

# ── NETWORKMANAGER (used by most modern distros) ──
nmcli device status                 # Overview of network devices
nmcli device wifi list              # Scan for nearby Wi-Fi networks
nmcli device wifi connect "SSID" password "pass"
nmcli connection show               # Saved network connections
nmcli connection up "connection-name"
nmcli connection down "connection-name"
nmcli connection modify "name" ipv4.dns 8.8.8.8
nmcli connection modify "name" ipv4.method manual \
  ipv4.addresses 192.168.1.100/24 \
  ipv4.gateway 192.168.1.1
```

### 6.2 Network Diagnostics

When something's not connecting, this is your troubleshooting toolkit, roughly in the order you'd reach for them.

```bash
# ── PING (is anything even reachable?) ──
ping 8.8.8.8                        # Test connectivity to Google's DNS
ping -c 4 8.8.8.8                   # Send exactly 4 packets, then stop
ping -i 0.5 8.8.8.8                 # Ping every half-second
ping -s 1472 8.8.8.8                # Test with a specific packet size (useful for MTU issues)

# ── TRACEROUTE (which path does traffic take?) ──
traceroute 8.8.8.8                  # Show every hop along the way
traceroute -n 8.8.8.8               # Skip hostname resolution (faster)
mtr 8.8.8.8                         # A live, continuously updating traceroute (q to quit)

# ── DNS LOOKUPS ──
dig google.com                      # Full, detailed DNS query
dig +short google.com               # Just the IP address, nothing else
dig -x 8.8.8.8                      # Reverse lookup (IP → hostname)
dig google.com MX                   # Mail server records
dig google.com NS                   # Name server records
dig google.com ANY                  # All record types at once

nslookup google.com                 # A simpler DNS lookup tool
nslookup 8.8.8.8                    # Reverse lookup

host google.com                     # Quick, minimal DNS lookup
host -t MX google.com               # MX record lookup

# ── IS A SITE ACTUALLY UP? ──
curl -I google.com                  # Fetch just the HTTP headers
curl -o /dev/null -s -w "%{http_code}\n" google.com  # Just the HTTP status code
wget --spider google.com            # Check a URL exists, without downloading it

# ── PORT SCANNING ──
nc -zv google.com 80                # Is port 80 open?
nc -zv google.com 80 443            # Check multiple ports at once
nc -zvn 192.168.1.1 1-1000          # Scan a range of ports

# ── WHAT'S USING THE NETWORK? ──
ss -tuln                            # Every port currently listening
ss -tup                             # Active connections
ss -tup | grep :80                  # Just connections on port 80
netstat -tuln                       # Older equivalent of ss
sudo lsof -i :80                    # What process is using port 80?
sudo lsof -i -P -n                  # All network connections, unfiltered
```

### 6.3 Firewall (`ufw` — Uncomplicated Firewall)

```bash
sudo ufw status                     # Check whether the firewall is active
sudo ufw enable                     # Turn it on
sudo ufw disable                    # Turn it off
sudo ufw default deny               # Deny all incoming traffic by default
sudo ufw default allow outgoing     # Allow all outgoing traffic by default

sudo ufw allow ssh                  # Allow SSH (uses the standard rule name)
sudo ufw allow 80/tcp                # Allow HTTP
sudo ufw allow 443/tcp               # Allow HTTPS
sudo ufw allow 22                    # Allow port 22, any protocol
sudo ufw deny 23                     # Block Telnet
sudo ufw deny from 1.2.3.4          # Block a specific IP address
sudo ufw allow from 192.168.1.0/24  # Allow an entire subnet

sudo ufw delete allow 80/tcp        # Remove a rule
sudo ufw reset                      # Reset everything to defaults
sudo ufw status numbered            # Show rules with index numbers
sudo ufw delete 3                   # Delete rule #3 by its number
```

---

## 7. Storage & Filesystems

### 7.1 Partitioning

```bash
# ── VIEWING & EDITING PARTITIONS ──
sudo fdisk -l                       # List all disks and partitions
sudo fdisk /dev/sda                 # Interactive partition editor
# n = new partition   d = delete   t = change type
# w = write changes   q = quit without saving

# Modern alternative for GPT disks:
sudo gdisk /dev/sda

# Non-interactive partitioning:
sudo parted /dev/sda mklabel gpt    # Create a GPT partition table
sudo parted /dev/sda mkpart primary ext4 1MiB 100%  # One partition spanning the whole disk
sudo parted /dev/sda mkpart primary fat32 1MiB 512MiB  # An EFI system partition

# ── FORMATTING ──
sudo mkfs.ext4 /dev/sda1            # Format as ext4 (the common Linux default)
sudo mkfs.ext4 -L "MYDRIVE" /dev/sda1  # ...with a volume label
sudo mkfs.xfs /dev/sda1             # XFS filesystem
sudo mkfs.btrfs /dev/sda1           # Btrfs filesystem
sudo mkfs.ntfs /dev/sda1            # NTFS, for Windows compatibility
sudo mkfs.vfat -F32 /dev/sda1       # FAT32, common for EFI/USB drives
sudo mkswap /dev/sda2               # Set up a swap partition
```

### 7.2 Mounting

```bash
# ── TEMPORARY MOUNTS ──
sudo mount /dev/sda1 /mnt
sudo mount /dev/sda1 /mnt -t ext4
sudo mount -o ro /dev/sda1 /mnt         # Mount read-only
sudo mount -o rw,noexec /dev/sda1 /mnt  # Mount without allowing execution
sudo umount /mnt                        # Unmount
sudo umount -l /mnt                     # Lazy unmount, for a busy device

# ── PERMANENT MOUNTS (via /etc/fstab) ──
sudo vim /etc/fstab
# Add a line like:
# UUID=xxxx-xxxx  /mnt/data  ext4  defaults  0  2

sudo mount -a                       # Mount everything listed in fstab
sudo mount /mnt/data                # Mount just one fstab entry

# ── MOUNTING USB DRIVES ──
sudo mkdir /media/usb
sudo mount /dev/sdb1 /media/usb
# Most modern desktop distros auto-mount USB drives under /media/username/ automatically.
```

### 7.3 LVM (Logical Volume Manager)

LVM adds a flexible layer between your physical disks and your filesystems, letting you resize, combine, or extend storage without repartitioning from scratch.

```bash
sudo apt install lvm2               # Install the LVM tools

# ── SETTING UP LVM ──
sudo pvcreate /dev/sda1 /dev/sdb1          # Mark disks as "physical volumes"
sudo vgcreate vg_data /dev/sda1 /dev/sdb1  # Combine them into a "volume group"
sudo lvcreate -L 100G vg_data -n lv_data   # Carve out a 100GB "logical volume"
sudo lvcreate -l 100%FREE vg_data -n lv_home  # Use all remaining free space

sudo mkfs.ext4 /dev/vg_data/lv_data
sudo mount /dev/vg_data/lv_data /mnt/data

# ── MANAGEMENT ──
sudo pvs                            # Physical volume status
sudo vgs                            # Volume group status
sudo lvs                            # Logical volume status

# Growing a volume on the fly:
sudo lvextend -L +50G /dev/vg_data/lv_data  # Add 50GB to it
sudo resize2fs /dev/vg_data/lv_data         # Resize the filesystem to match
# For XFS filesystems instead: sudo xfs_growfs /mount/point
```

### 7.4 RAID

RAID combines multiple physical disks into one logical unit, either for speed, redundancy, or both.

```bash
sudo apt install mdadm              # Install the software RAID tool

# ── CREATING A RAID ARRAY ──
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1
# RAID 0 (striping, speed, no redundancy):     --level=0
# RAID 1 (mirroring, redundancy):              --level=1
# RAID 5 (striping + parity, 3+ disks):        --level=5 --raid-devices=3
# RAID 6 (like RAID 5, survives 2 failures):   --level=6 --raid-devices=4

sudo mkfs.ext4 /dev/md0
sudo mount /dev/md0 /mnt/raid

# ── MONITORING ──
cat /proc/mdstat                    # Current RAID status
sudo mdadm --detail /dev/md0        # Detailed array information

# ── HANDLING A FAILED DISK ──
sudo mdadm --manage /dev/md0 --fail /dev/sda1
sudo mdadm --manage /dev/md0 --remove /dev/sda1
sudo mdadm --manage /dev/md0 --add /dev/sdc1
```

---

## 8. Services & Systemd

Most modern distros use **systemd** to manage background services (things like web servers, databases, and daemons that run continuously).

### 8.1 Managing Services

```bash
# ── START / STOP ──
sudo systemctl start nginx          # Start a service right now
sudo systemctl stop nginx           # Stop it
sudo systemctl restart nginx        # Stop then start again
sudo systemctl reload nginx         # Reload its config with zero downtime
sudo systemctl reload-or-restart nginx  # Reload if supported, otherwise restart

# ── ENABLE / DISABLE (controls startup at boot) ──
sudo systemctl enable nginx         # Start automatically at boot
sudo systemctl disable nginx        # Don't start automatically
sudo systemctl enable --now nginx   # Enable AND start it immediately
sudo systemctl disable --now nginx  # Disable AND stop it immediately

# ── CHECKING STATUS ──
sudo systemctl status nginx         # Full status, plus recent log lines
systemctl is-active nginx           # active or inactive?
systemctl is-enabled nginx          # enabled or disabled?
systemctl is-failed nginx           # failed or fine?
```

### 8.2 Creating Your Own Service

If you're deploying your own app, you'll often want systemd to manage it — auto-restarting it on crashes, starting it at boot, etc.

```bash
sudo vim /etc/systemd/system/myapp.service
```

```ini
[Unit]
Description=My Custom Application
After=network.target
Wants=network.target

[Service]
Type=simple
User=myuser
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/python3 /opt/myapp/main.py
Restart=on-failure
RestartSec=5
Environment="MY_VAR=value"
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload        # Tell systemd about the new file
sudo systemctl enable --now myapp   # Start it now, and on every future boot
```

### 8.3 Reading Logs with `journalctl`

```bash
journalctl                          # Every log entry systemd has recorded
journalctl -u nginx                 # Just logs for one specific service
journalctl -u nginx -u sshd         # Logs from multiple services at once
journalctl -f                       # Follow live, like tail -f
journalctl -n 50                    # Just the last 50 lines
journalctl --since "1 hour ago"
journalctl --since "2025-06-01" --until "2025-06-02"
journalctl -p err                   # Only error-level and above
journalctl -p warning               # Warnings and above
journalctl -x                       # Add extra explanatory context
journalctl -xe                      # Jump to the end, with explanations — great for debugging crashes
journalctl -b                       # Logs since the most recent boot
journalctl -b -1                    # Logs from the previous boot
journalctl --disk-usage             # How much space the logs are taking up

# Cleaning up old logs:
sudo journalctl --vacuum-size=100M  # Keep only the most recent 100MB
sudo journalctl --vacuum-time=2weeks  # Keep only the last 2 weeks
```

---

## 9. Shell Scripting

Once you're comfortable with individual commands, the natural next step is stringing them together into **scripts** — reusable, automatable programs written in the shell's own language.

### 9.1 Your First Script

```bash
#!/bin/bash
# The "shebang" line above tells the system which interpreter to use

# Comments start with #
echo "Hello, World!"
```

```bash
chmod +x hello.sh       # Make it executable
./hello.sh              # Run it
```

### 9.2 Variables

```bash
#!/bin/bash

# No spaces around the = sign!
name="Alice"
age=30

# Reference a variable with $
echo "$name is $age years old"

# Command substitution — capture a command's output into a variable
current_date=$(date)
echo "Today is $current_date"

# Arithmetic
sum=$((5 + 3))
echo "5 + 3 = $sum"

# Read-only constants
readonly PI=3.14159

# Local variables (scoped to a function)
myfunc() {
    local local_var="I'm local"
    echo $local_var
}
```

### 9.3 Reading User Input

```bash
read -p "Enter your name: " name
echo "Hello, $name!"

# Hide input as it's typed (for passwords)
read -s -p "Enter password: " password
echo
echo "Password length: ${#password}"

# A simple choice menu
echo "1) Option A"
echo "2) Option B"
read -p "Choose (1-2): " choice
case $choice in
    1) echo "You chose A" ;;
    2) echo "You chose B" ;;
    *) echo "Invalid choice" ;;
esac
```

### 9.4 Conditionals

```bash
#!/bin/bash

# ── IF STATEMENTS ──
if [ "$1" = "hello" ]; then
    echo "You said hello!"
elif [ "$1" = "bye" ]; then
    echo "Goodbye!"
else
    echo "I don't understand"
fi

# ── FILE TESTS ──
if [ -f "/etc/passwd" ]; then
    echo "File exists"
fi
if [ -d "/home" ]; then
    echo "Directory exists"
fi
if [ -x "/usr/bin/python3" ]; then
    echo "Python is executable"
fi
if [ -r "$file" ]; then echo "Readable"; fi
if [ -w "$file" ]; then echo "Writable"; fi
if [ -s "$file" ]; then echo "Not empty"; fi

# ── STRING TESTS ──
if [ -z "$var" ]; then          # True if the string is empty
if [ -n "$var" ]; then          # True if the string is NOT empty
if [ "$a" = "$b" ]; then        # Equal
if [ "$a" != "$b" ]; then       # Not equal
if [[ "$a" < "$b" ]]; then      # Alphabetically before

# ── NUMERIC TESTS ──
if [ "$age" -eq 18 ]; then      # Equal
if [ "$age" -ne 18 ]; then      # Not equal
if [ "$age" -gt 18 ]; then      # Greater than
if [ "$age" -ge 18 ]; then      # Greater than or equal
if [ "$age" -lt 18 ]; then      # Less than
if [ "$age" -le 18 ]; then      # Less than or equal

# ── LOGICAL OPERATORS ──
if [ "$a" = "yes" ] && [ "$b" = "yes" ]; then  # AND
if [ "$a" = "yes" ] || [ "$b" = "yes" ]; then  # OR
if ! [ -f "$file" ]; then                       # NOT
```

### 9.5 Loops

```bash
#!/bin/bash

# ── FOR LOOP ──
# Over a fixed list:
for fruit in apple banana cherry; do
    echo "I like $fruit"
done

# Over a numeric range:
for i in {1..5}; do
    echo "Number $i"
done

# C-style syntax:
for ((i=0; i<10; i++)); do
    echo "i = $i"
done

# Over the output of a command (like a glob):
for file in *.txt; do
    echo "Processing $file"
done

# ── WHILE LOOP ──
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Read a file line by line:
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# ── UNTIL LOOP (runs until a condition becomes true) ──
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done

# ── BREAK AND CONTINUE ──
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue        # Skip this iteration
    fi
    if [ $i -eq 8 ]; then
        break            # Exit the loop entirely
    fi
    echo "$i"
done
```

### 9.6 Functions

```bash
#!/bin/bash

# Define a function
greet() {
    echo "Hello, $1! You are $2 years old."
}

# Call it with arguments
greet "Alice" 30
greet "Bob" 25

# Return an exit code (0-255 only)
add() {
    return $(($1 + $2))
}
add 5 3
echo "Result: $?"

# For actual return VALUES, print to stdout and capture it:
multiply() {
    echo $(($1 * $2))
}
result=$(multiply 4 5)
echo "4 * 5 = $result"
```

### 9.7 A Real-World Script Example

Here's a practical system-information script that ties a lot of the above together:

```bash
#!/bin/bash

# System Information Script

RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

echo -e "${GREEN}═══════════════════════════════════════════${NC}"
echo -e "${GREEN}  System Information Report${NC}"
echo -e "${GREEN}  $(date)${NC}"
echo -e "${GREEN}═══════════════════════════════════════════${NC}"
echo ""

# HOSTNAME
echo -e "${YELLOW}Hostname:${NC} $(hostname)"
echo ""

# OS
echo -e "${YELLOW}Operating System:${NC}"
if [ -f /etc/os-release ]; then
    cat /etc/os-release | grep "^PRETTY_NAME" | cut -d'"' -f2
fi
echo ""

# KERNEL
echo -e "${YELLOW}Kernel:${NC} $(uname -r)"
echo ""

# UPTIME
echo -e "${YELLOW}Uptime:${NC} $(uptime -p)"
echo ""

# CPU
echo -e "${YELLOW}CPU:${NC} $(grep "model name" /proc/cpuinfo | head -1 | cut -d: -f2 | xargs)"
echo -e "${YELLOW}Cores:${NC} $(nproc)"
echo ""

# MEMORY
mem_total=$(free -h | grep Mem | awk '{print $2}')
mem_used=$(free -h | grep Mem | awk '{print $3}')
mem_percent=$(free | grep Mem | awk '{printf "%.1f", $3/$2 * 100}')
echo -e "${YELLOW}Memory:${NC} $mem_used / $mem_total ($mem_percent%)"
echo ""

# DISK
echo -e "${YELLOW}Disk Usage:${NC}"
df -h | grep "^/dev" | awk '{print "  " $6 " : " $3 " / " $2 " (" $5 ")"}'
echo ""

# NETWORK
echo -e "${YELLOW}IP Addresses:${NC}"
ip -br addr | grep -v lo | awk '{print "  " $1 " : " $3}'
echo ""

# LOGGED IN USERS
echo -e "${YELLOW}Logged in users:${NC} $(who | wc -l)"
echo ""

# RECENT LOGIN FAILURES
echo -e "${YELLOW}Recent SSH failures:${NC}"
if [ -f /var/log/auth.log ]; then
    grep "Failed password" /var/log/auth.log | tail -3
elif [ -f /var/log/secure ]; then
    grep "Failed password" /var/log/secure | tail -3
else
    echo "  No auth log found"
fi

echo ""
echo -e "${GREEN}═══════════════════════════════════════════${NC}"
```

> 💡 **Try it yourself:** Save this as `sysinfo.sh`, run `chmod +x sysinfo.sh`, then `./sysinfo.sh`. It's a great template to extend with your own checks (disk temperature, running Docker containers, whatever you need).


---

## 10. System Administration

### 10.1 Scheduling Tasks with Cron

`cron` is Linux's built-in task scheduler — perfect for backups, cleanup jobs, or anything that needs to run automatically on a schedule.

```bash
# ── MANAGING CRON JOBS ──
crontab -e                      # Edit your own scheduled jobs
crontab -l                      # List your scheduled jobs
crontab -r                      # Remove ALL of your cron jobs
sudo crontab -e                 # Edit root's crontab

# ── CRON SYNTAX ──
# Minute Hour DayOfMonth Month DayOfWeek Command
# 0-59   0-23 1-31       1-12  0-7       command

# Examples:
*/5 * * * * /path/to/script.sh          # Every 5 minutes
0 * * * * /path/to/script.sh            # Every hour, on the hour
0 2 * * * /path/to/script.sh            # Every day at 2 AM
0 0 * * 0 /path/to/script.sh            # Every Sunday at midnight
0 0 1 * * /path/to/script.sh            # On the 1st of every month
0 6 * * 1-5 /path/to/script.sh          # Weekdays at 6 AM
```

> 💡 **Tip:** Not sure your cron syntax is right? Sites like crontab.guru let you enter a schedule and see it translated into plain English before you commit it.

### 10.2 System Monitoring

```bash
# ── REAL-TIME MONITORING ──
top                    # Built-in process viewer (1=per-core CPU, M=sort by memory, P=sort by CPU)
htop                   # A friendlier version (install: apt install htop)
btm                    # "Bottom" — modern, Rust-based alternative
glances                # A full system overview in one screen

# ── CPU ──
lscpu                  # CPU architecture summary
cat /proc/cpuinfo      # Full, detailed CPU info
mpstat -P ALL 1        # Per-core usage, refreshed every second (install: sysstat)

# ── MEMORY ──
free -h                # Memory usage, human-readable
free -h -t             # ...including totals
cat /proc/meminfo      # Full memory details
vmstat 2               # Virtual memory stats, refreshed every 2 seconds

# ── DISK I/O ──
iostat -x 2            # Disk I/O stats (install: sysstat)
iotop                  # Per-process disk I/O (install: iotop)
sudo iotop -o          # Only show processes actively doing I/O

# ── NETWORK ──
iftop                  # Bandwidth usage per connection (install: iftop)
sudo iftop -i eth0     # Monitor a specific network interface
nethogs                # Bandwidth usage per process (install: nethogs)
sudo nethogs eth0
bmon                   # A general bandwidth monitor (install: bmon)

# ── PROCESS LISTS ──
ps aux --sort=-%cpu | head -20      # Top 20 CPU-hungry processes
ps aux --sort=-%mem | head -20      # Top 20 memory-hungry processes
ps aux | grep -c defunct            # Count "zombie" processes
```

### 10.3 Log Management

```bash
# ── COMMON LOG LOCATIONS ──
/var/log/syslog          # System log (Ubuntu/Debian)
/var/log/messages        # System log (RHEL/Fedora)
/var/log/auth.log        # Authentication logs (Ubuntu/Debian)
/var/log/secure          # Authentication logs (RHEL/Fedora)
/var/log/kern.log        # Kernel messages
/var/log/dmesg           # Kernel ring buffer (boot-time messages)
/var/log/nginx/          # Nginx access/error logs
/var/log/apache2/        # Apache access/error logs
/var/log/mysql/          # MySQL logs
/var/log/faillog         # Failed login attempts

# ── VIEWING LOGS ──
tail -f /var/log/syslog                    # Watch the system log update live
tail -n 100 /var/log/syslog                # Just the last 100 lines
grep -i "error" /var/log/syslog            # Search for errors
journalctl -xe                             # systemd logs with helpful context
journalctl -u nginx --since "1 hour ago"   # A specific service's recent logs

# ── LOG ROTATION (preventing logs from growing forever) ──
ls /etc/logrotate.d/                       # See existing rotation rules
cat /etc/logrotate.d/nginx                 # Example config

# Force a manual rotation:
sudo logrotate -f /etc/logrotate.conf
```

### 10.4 Backup & Restore

```bash
# ── RSYNC (the go-to tool for efficient file syncing) ──
# Local sync:
rsync -av source_dir/ dest_dir/
rsync -av --delete source/ dest/          # Also delete files in dest that no longer exist in source
rsync -av --progress source/ dest/        # Show a progress bar

# Remote sync (over SSH):
rsync -avz /local/dir/ user@host:/remote/dir/
rsync -avz -e "ssh -p 2222" /local/ user@host:/remote/

# Incremental backups using hard links (like Apple's Time Machine):
rsync -av --link-dest=/backup/yesterday/ /source/ /backup/today/

# ── TAR (classic archiving & compression) ──
tar -czf backup.tar.gz /home/username/          # Create a compressed backup
tar -czf backup_$(date +%Y%m%d).tar.gz /data    # A dated backup file
tar -xzf backup.tar.gz                          # Restore/extract it

# Exclude certain paths:
tar --exclude='*.log' --exclude='cache/' -czf backup.tar.gz /var/www

# ── DD (raw disk cloning — powerful and dangerous) ──
sudo dd if=/dev/sda of=/dev/sdb bs=4M status=progress  # Clone one disk to another
sudo dd if=/dev/sda of=disk_image.img bs=4M            # Create a disk image file
sudo dd if=disk_image.img of=/dev/sda bs=4M            # Restore from that image
```

> ⚠️ **`dd` is famously called "disk destroyer"** for a reason — double-check your `if=` (input) and `of=` (output) before hitting Enter. Getting them backwards can wipe the wrong disk.

---

## 11. Security

### 11.1 File Integrity — Checksums

```bash
md5sum file.txt           # MD5 hash (fast, but not cryptographically strong anymore)
sha1sum file.txt          # SHA-1 hash
sha256sum file.txt        # SHA-256 (the recommended default today)
sha512sum file.txt        # SHA-512

# Verify against a known hash:
echo "hash_value  filename" | sha256sum -c

# Generate and later verify a whole set of checksums:
sha256sum * > checksums.sha256
sha256sum -c checksums.sha256
```

### 11.2 Encryption

```bash
# ── GPG (for encrypting/signing individual files) ──
gpg --gen-key                                    # Generate a new key pair
gpg --encrypt --recipient user@email file.txt    # Encrypt a file for someone
gpg --decrypt file.txt.gpg                       # Decrypt it
gpg --sign file.txt                              # Digitally sign a file
gpg --verify file.txt.sig                        # Verify a signature
gpg --export --armor user@email > public.key     # Export your public key to share
gpg --import public.key                          # Import someone else's public key

# ── OPENSSL (another common encryption tool) ──
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc      # Encrypt
openssl enc -d -aes-256-cbc -in file.enc -out file.txt         # Decrypt

# Generate strong random passwords:
openssl rand -base64 32     # 32 random bytes, base64-encoded
openssl rand -hex 16        # 16 random bytes, hex-encoded

# ── LUKS (full disk/partition encryption) ──
sudo cryptsetup luksFormat /dev/sda1           # Encrypt a whole partition
sudo cryptsetup open /dev/sda1 secret          # Unlock it and map it as /dev/mapper/secret
sudo mkfs.ext4 /dev/mapper/secret              # Create a filesystem inside
sudo mount /dev/mapper/secret /mnt/secret      # Mount it normally
sudo cryptsetup close secret                   # Lock it back up when done
```

### 11.3 Hardening SSH

SSH is usually the first thing attackers probe on an internet-facing server, so it's worth locking down.

```bash
sudo vim /etc/ssh/sshd_config
```

```
Port 2222                           # Move off the default port 22
PermitRootLogin no                  # Never allow direct root login
PasswordAuthentication no           # Require SSH keys instead of passwords
PubkeyAuthentication yes
AllowUsers alice bob                # Only allow specific named users
MaxAuthTries 3                      # Limit login attempts before disconnecting
ClientAliveInterval 300             # Disconnect idle sessions after 5 minutes
ClientAliveCountMax 0
Protocol 2                          # Only allow the modern SSH protocol
```

```bash
sudo systemctl restart sshd

# ── SETTING UP KEY-BASED LOGIN ──
ssh-keygen -t ed25519               # Generate a modern, recommended key type
ssh-keygen -t rsa -b 4096           # RSA, for maximum compatibility with older systems
ssh-copy-id user@hostname           # Copy your public key onto a remote server
```

> ⚠️ **Before disabling password authentication**, make sure your SSH key actually works and you can log in with it. Test in a second terminal window *before* closing your current session — locking yourself out of a remote server is a very real (and very common) mistake.

---

## 12. Advanced Topics

### 12.1 Process Substitution

```bash
# Compare the output of two commands directly, without temp files:
diff <(ls /dir1) <(ls /dir2)

# Merge two sorted files:
join <(sort file1.txt) <(sort file2.txt)

# Feed a command's output into a loop:
while read line; do
    echo "File 1: $line"
done < <(command1)
```

### 12.2 Regular Expressions — The Full Picture

```bash
# ── BASIC REGEX ──
grep 'hello' file.txt           # Literal match for "hello"
grep '^hello' file.txt          # Lines STARTING with "hello"
grep 'hello$' file.txt          # Lines ENDING with "hello"
grep '^$' file.txt              # Empty lines
grep 'h.llo' file.txt           # "." matches any single character
grep 'h[ae]llo' file.txt        # Matches "hallo" or "hello"
grep '[0-9]' file.txt           # Any digit
grep '[a-z]' file.txt           # Any lowercase letter
grep '[A-Z]' file.txt           # Any uppercase letter
grep '[^0-9]' file.txt          # Anything that's NOT a digit
grep 'colou\?r' file.txt        # "color" or "colour" (the ? makes 'u' optional)

# ── EXTENDED REGEX (grep -E) ──
grep -E 'hello|world' file.txt          # Match either word (OR)
grep -E 'colou?r' file.txt              # ? = zero or one occurrence
grep -E 'colou*r' file.txt              # * = zero or more
grep -E 'colou+r' file.txt              # + = one or more
grep -E '[0-9]{3}' file.txt             # Exactly 3 digits
grep -E '[0-9]{3,5}' file.txt           # 3 to 5 digits
grep -E '[0-9]{3,}' file.txt            # 3 or more digits
grep -E '(foo|bar)123' file.txt         # "foo123" or "bar123"

# ── PERL-COMPATIBLE REGEX (grep -P) ──
grep -P '\d{3}-\d{3}-\d{4}' file.txt    # Phone numbers like 555-123-4567
grep -P '\w+@\w+\.\w+' file.txt         # A simple email pattern
grep -P 'https?://[\w\.]+' file.txt     # URLs
```

### 12.3 Virtualization & Containers

```bash
# ── DOCKER ──
docker pull ubuntu                      # Download an image
docker run -it ubuntu bash              # Run an interactive container
docker run -d -p 80:80 nginx            # Run in the background, mapping a port
docker ps                               # List running containers
docker ps -a                            # List ALL containers, including stopped ones
docker stop container_id                # Stop a container
docker rm container_id                  # Remove a stopped container
docker images                           # List downloaded images
docker rmi image_name                   # Remove an image
docker exec -it container_id bash       # Open a shell inside a running container
docker logs container_id                # View a container's logs
docker-compose up -d                    # Start a whole multi-container app defined in a compose file

# ── VAGRANT (for spinning up disposable VMs) ──
vagrant init ubuntu/focal64             # Create a Vagrantfile for this box
vagrant up                              # Boot the VM
vagrant ssh                             # SSH into it
vagrant halt                            # Shut it down
vagrant destroy                         # Delete it completely
```

---

## 13. Troubleshooting Playbook

A handy, symptom-first cheat sheet for when something's gone wrong. Read the "Symptom" and jump straight to a diagnostic command.

| Symptom | Where to Look |
|---|---|
| System feels slow | `top` / `htop` — check CPU and memory hogs |
| Out of disk space | `df -h` then `du -sh * \| sort -h` to find the culprit |
| A service won't start | `sudo systemctl status <service>` then `journalctl -u <service> -xe` |
| Can't connect to the internet | `ip addr` → `ping 8.8.8.8` → `dig google.com` (isolates hardware vs. DNS issues) |
| "Permission denied" errors | `ls -l` on the file, check owner/group/mode with `id` |
| A port isn't reachable | `ss -tuln` to confirm it's listening, `sudo ufw status` to check the firewall |
| Server ran out of memory | `free -h`, then `ps aux --sort=-%mem \| head` |
| Boot failure after editing `/etc/fstab` | Boot into recovery mode / live USB, comment out the bad line, reboot |
| Locked out of SSH after hardening it | Use your provider's console/rescue mode, review `/etc/ssh/sshd_config` |
| "No space left on device" but `df` shows free space | You may be out of **inodes** — check with `df -i` |
| Command not found | Check `$PATH` with `echo $PATH`, confirm the package is installed |
| Zombie processes piling up | Usually harmless, but check the parent process with `ps -ef \| grep defunct` |

### General Debugging Method

1. **Reproduce** the problem and note the exact error message.
2. **Check logs first** — `journalctl -xe`, `/var/log/syslog`, or the app's own log file.
3. **Isolate the layer** — is it hardware, OS, network, or application?
4. **Change one thing at a time** and re-test — resist the urge to fix five things at once.
5. **Document the fix** — future-you (or a teammate) will thank you.

---

## 14. Learning Path & Next Steps

If you're working through this guide as a beginner, here's a sensible order to actually *practice* things in — reading alone won't build muscle memory.

1. **Get comfortable navigating** — Parts 1–3.2 (filesystem + basic navigation) until `cd`, `ls`, and `pwd` feel automatic.
2. **Learn to read and edit files** — `cat`, `less`, `nano`, then graduate to `vim` once you're ready for more power.
3. **Understand permissions early** — a huge fraction of "why won't this work?!" moments trace back to permissions.
4. **Practice pipes and redirection** — this is the single highest-leverage skill on this list. Spend real time here.
5. **Write your first few scripts** — automate something small and annoying in your own workflow.
6. **Learn systemd and process management** — essential once you're running anything long-lived.
7. **Get networking fundamentals down** — `ip`, `ping`, `dig`, `ss` — you'll use these constantly.
8. **Only then go deep on LVM/RAID/security hardening** — these matter most once you're managing real infrastructure.

### Good Habits to Build Early

- **Read man pages.** `man <command>` is faster and more authoritative than most blog posts, including this one.
- **Use `--help`.** Almost every command supports it: `command --help`.
- **Keep a personal notes file** of commands you look up more than once — you'll stop looking them up.
- **Test destructive commands on throwaway VMs** before running them on anything real.
- **Version control your dotfiles and configs** (`.bashrc`, `/etc` changes) so you can always roll back.

---

## 15. Glossary

| Term | Meaning |
|---|---|
| **Kernel** | The core of the OS that talks directly to hardware and manages resources |
| **Shell** | The program that interprets the commands you type (e.g., Bash, Zsh) |
| **Daemon** | A background process/service (often ends in "d", like `sshd`) |
| **PID** | Process ID — a unique number identifying a running process |
| **Root** | The administrator account with unrestricted system access |
| **Symlink** | A "shortcut" file that points to another file or directory |
| **Mount point** | A directory used as the attachment point for a disk/partition |
| **Inode** | A data structure storing metadata about a file (not its name or content) |
| **Package** | A bundled, installable unit of software plus its metadata |
| **Repository** | A server hosting packages that your package manager can install from |
| **Environment variable** | A named value (like `$PATH`) available to processes in a shell |
| **Pipe** | The `\|` operator, connecting one command's output to another's input |
| **Cron job** | A task scheduled to run automatically at set times |
| **Systemd unit** | A configuration file describing a service, mount, or timer for systemd |

---

## 16. Quick Reference Card

| Task | Command |
|---|---|
| List files | `ls -la` |
| Current directory | `pwd` |
| Change directory | `cd /path` |
| Create file | `touch file` |
| Copy file | `cp src dst` |
| Move/rename | `mv old new` |
| Delete file | `rm file` |
| Delete directory | `rm -rf dir` |
| View file | `cat file`, `less file` |
| Find text | `grep "text" file` |
| Find files | `find . -name "*.txt"` |
| Install package (Debian) | `sudo apt install pkg` |
| Install package (Fedora) | `sudo dnf install pkg` |
| Update system (Debian) | `sudo apt update && sudo apt upgrade` |
| Update system (Fedora) | `sudo dnf upgrade` |
| Process list | `ps aux` |
| Kill process | `kill PID` |
| Disk usage | `df -h` |
| Directory size | `du -sh dir` |
| Permissions | `chmod 755 file` |
| Ownership | `chown user:group file` |
| Run as root | `sudo command` |
| Network info | `ip addr` |
| DNS lookup | `dig domain.com` |
| Port status | `ss -tuln` |
| Service start | `sudo systemctl start svc` |
| Enable service at boot | `sudo systemctl enable svc` |
| View logs | `journalctl -u svc` |
| Schedule task | `crontab -e` |
| Search packages | `apt search keyword` |
| Shell history | `history` |

---

## 🤝 Contributing

Found a mistake, or want to add a command you rely on daily? Contributions are welcome — open an issue or submit a pull request.

## 📄 License

This guide is free to use, share, and adapt. If you found it useful, a ⭐ on the repo is always appreciated.
