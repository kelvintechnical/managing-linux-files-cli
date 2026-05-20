<div align="center">

# 🗂 Managing Files from the Linux CLI

### A complete beginner-to-intermediate reference for Linux file systems,
### file management, links, I/O redirection, and pipes — all from the command line.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Stars](https://img.shields.io/github/stars/kelvintechnical/managing-linux-files-cli?style=social)](https://github.com/kelvintechnical/managing-linux-files-cli)

> Built for learners worldwide. Star ⭐ this repo if it helped you.

</div>

---

## 📚 Table of Contents

- [What is a File System?](#-what-is-a-file-system)
- [Linux Directory Structure](#-linux-directory-structure)
- [Directory Listing Attributes](#-directory-listing-attributes)
- [Creating Files and Directories](#-creating-files-and-directories)
- [File Maintenance Commands](#-file-maintenance-commands)
- [Soft Links and Hard Links](#-soft-links-and-hard-links)
- [Input & Output Redirection](#-input--output-redirection)
- [Pipes](#-pipes)
- [Quick Reference Commands](#-quick-reference-commands)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [Author](#-author)

---

## 🗄 What is a File System?

A **file system** is the method an operating system uses to organize, store, and retrieve files on a disk.

> 💡 **Analogy:** Think of a closet. If you organize your clothes by category — shirts, pants, shoes — you find things fast. If you throw everything in one pile, it's chaos. A file system is the organizer.

### File System Types

| OS | File System |
|---|---|
| Linux | `ext3`, `ext4`, `xfs` |
| Windows | `NTFS`, `FAT` |

Each new OS release typically ships with an improved or enhanced file system version.

---

## 🌲 Linux Directory Structure

In Linux, **everything starts from `/`** (root directory) — just like Windows starts from `C:\`.

```bash
cd /
ls -l
```

| Directory | Purpose |
|---|---|
| `/bin` | Essential user binaries (commands) |
| `/boot` | Boot loader files |
| `/etc` | System configuration files |
| `/home` | User home directories |
| `/opt` | Optional/third-party programs |
| `/sbin` | System administration commands and scripts |
| `/tmp` | Temporary files |
| `/var` | Variable data — log files, etc. |

---

## 📋 Directory Listing Attributes

When you run `ls -l`, each line has **8 fields**:

```
drwxr-xr-x  2  iafzal  iafzal  4096  May 20  14:00  myfolder
```

| Field | What It Means |
|---|---|
| `d` / `l` / `-` | **Type**: `d` = directory, `l` = link, `-` = regular file |
| `2` | Number of hard links |
| `iafzal` | Owner (user) |
| `iafzal` | Group owner |
| `4096` | Size in bytes |
| `May 20` | Last modified date |
| `14:00` | Last modified time |
| `myfolder` | File or directory name |

### Useful `ls` Variants

```bash
ls -l        # Long listing (alphabetical)
ls -ltr      # Long listing sorted by time, oldest first
ls -la       # Long listing including hidden files (dotfiles)
ls -ltri     # Long listing with inode numbers
ll           # Shorthand alias for ls -l (on most distros)
```

---

## 📁 Creating Files and Directories

### Check Where You Are

```bash
whoami       # Who am I logged in as?
pwd          # What directory am I in?
```

### Create Files — 3 Methods

**Method 1: `touch` (creates empty file)**

```bash
touch jerry
touch kramer george

# Create multiple files at once:
touch bart lisa marge
```

**Method 2: `cp` (copy an existing file)**

```bash
cp jerry lex      # Copy jerry → new file named lex
cp jerry clark
cp jerry lois
```

**Method 3: `vi` (create and edit in text editor)**

```bash
vi homer
```

> Inside vi — to save and quit immediately without editing:
```
:wq!
```
> ⚠️ `vi` is powerful but has a learning curve. We'll cover it in detail in a separate module.

---

### Create Directories

```bash
mkdir seinfeld             # Create one directory
mkdir superman simpson     # Create multiple at once
```

### Verify Your Work

```bash
ls -ltr     # List all files and dirs, newest at the bottom
```

> ⚠️ You cannot create files/directories in locations you don't own (e.g., `/etc`) without `sudo` or root access.

---

## 🔧 File Maintenance Commands

| Command | Purpose |
|---|---|
| `cp` | Copy a file |
| `rm` | Remove (delete) a file |
| `mv` | Move or rename a file |
| `mkdir` | Create a directory |
| `rmdir` | Remove an empty directory |
| `rm -r` | Remove a directory and all its contents |
| `chgrp` | Change group ownership of a file |
| `chown` | Change user ownership of a file |

---

### `cp` — Copy

```bash
cp george david           # Copy george → new file named david
cp david /tmp             # Copy david to the /tmp directory
```

### Add content to a file

```bash
echo "david puddy is Elaine's boyfriend" > david
cat david                 # Verify content
```

---

### `rm` — Remove

```bash
rm apoho                  # Delete file named apoho
ls -ltr                   # Confirm it's gone
```

---

### `mv` — Move or Rename

```bash
# Rename a file (within same directory):
mv lex luther

# Move a file to a different location:
mv puddy /tmp

# Move it back:
mv /tmp/puddy /home/your_username/
```

---

### `mkdir` / `rmdir` / `rm -r` — Directories

```bash
mkdir gameofthrone         # Create directory
rmdir gameofthrone         # Remove empty directory
rm -r gameofthrone         # Remove directory and all contents
```

---

### `chgrp` and `chown` — Ownership

> Must be `root` to change ownership to root.

```bash
sudo chgrp root puddy              # Change group owner to root
sudo chown root puddy              # Change user owner to root

# Change both user and group at once:
sudo chown iafzal:iafzal puddy     # Restore to original owner
```

---

## 🔗 Soft Links and Hard Links

### What is an inode?

Every file on disk is assigned a unique number by the OS called an **inode**. The OS uses this number — not the filename — to find the file.

```bash
ls -ltri     # Show inode numbers
```

---

### Soft Link (Symbolic Link)

> Like a **Windows desktop shortcut** — points to the original file through its name.

```bash
# Create a soft link in /tmp pointing to hulk in home directory:
ln -s /home/iafzal/hulk /tmp/hulk
```

**Behavior:**
- If the **source file is deleted** → the soft link **breaks** (dangling link)
- The link and source file have **different inode numbers**

```bash
cat /tmp/hulk    # Works while source exists
rm ~/hulk        # Delete source
cat /tmp/hulk    # ERROR: No such file or directory
```

---

### Hard Link

> A **direct pointer to the inode** — not the filename.

```bash
# Create a hard link (no -s flag):
ln /home/iafzal/hulk /tmp/hulk
```

**Behavior:**
- If the **source file is deleted** → the hard link **still works**
- Both files share the **same inode number**
- Changes to the source are reflected in the hard link

```bash
echo "hulk is a superhero" > ~/hulk
echo "123" >> ~/hulk
cat /tmp/hulk      # Shows both lines — synced!

rm ~/hulk          # Delete source
cat /tmp/hulk      # Still works! Hard link survives.
```

---

### Soft vs Hard Link Summary

| Feature | Soft Link | Hard Link |
|---|---|---|
| Created with | `ln -s source dest` | `ln source dest` |
| Points to | Filename | inode directly |
| Source deleted | Link breaks | Link survives |
| inode number | Different | Same |
| Cross filesystem | ✅ Yes | ❌ No |
| Analogous to | Desktop shortcut | Full copy (shared data) |

---

## ↔️ Input & Output Redirection

In Linux, there are **three standard I/O streams**:

| Stream | Name | File Descriptor | Default |
|---|---|---|---|
| stdin | Standard Input | `0` | Keyboard |
| stdout | Standard Output | `1` | Terminal screen |
| stderr | Standard Error | `2` | Terminal screen |

---

### stdout — Output Redirection

**Single redirect `>` — overwrite**

```bash
ls -l > listings          # Save ls output to file (overwrites)
pwd > findpath            # Save current path to file
```

**Double redirect `>>` — append**

```bash
ls -la >> listings        # Append ls -la output to listings
echo "Hello World" >> findpath   # Append echo output to findpath
```

> ⚠️ Using `>` on an existing file **destroys** its previous content. Use `>>` to preserve it.

---

### stdin — Input Redirection

```bash
cat < findpath            # Feed file contents into cat
```

Useful when piping file contents into programs (e.g., `mail`):

```bash
mail -s "Office memo" all@company.com < memo.txt
```

---

### stderr — Error Redirection

```bash
ls -l /root 2> errorfile         # Route error to file (not screen)
cat errorfile                    # View the captured error

telnet localhost 2> errorfile    # Route connection errors to file
```

> File descriptor `2` = stderr. This is especially useful in **shell scripts** for clean logging.

---

## 🔀 Pipes

A **pipe `|`** connects the output of one command directly into the input of another.

```
command1 | command2
```

---

### Common Pipe Use Cases

**View long output one page at a time:**

```bash
ls -ltr | more      # Space bar to page through, Q to quit
```

**Get the last line of output:**

```bash
ls -l | tail -1
```

**Count files in a directory:**

```bash
ls | wc -l
```

**Search for a specific file:**

```bash
ls -l | grep "hulk"
```

> 💡 **Tip:** You can chain multiple pipes together:
```bash
ls -ltr /etc | grep conf | more
```

---

## ⚡ Quick Reference Commands

| Command | Purpose |
|---|---|
| `pwd` | Print current working directory |
| `whoami` | Show current user |
| `ls -l` | Long listing |
| `ls -ltr` | Long listing sorted by time (oldest first) |
| `ls -la` | Long listing including hidden files |
| `ls -ltri` | Long listing with inode numbers |
| `touch filename` | Create empty file |
| `cp src dest` | Copy file |
| `mv src dest` | Move or rename file |
| `rm filename` | Delete file |
| `mkdir dirname` | Create directory |
| `rmdir dirname` | Remove empty directory |
| `rm -r dirname` | Remove directory and contents |
| `chgrp group file` | Change group owner |
| `chown user:group file` | Change user and group owner |
| `ln -s src dest` | Create soft link |
| `ln src dest` | Create hard link |
| `cat filename` | View file contents |
| `echo "text" > file` | Write text to file (overwrite) |
| `echo "text" >> file` | Append text to file |
| `command > file` | Redirect stdout to file |
| `command >> file` | Append stdout to file |
| `command 2> file` | Redirect stderr to file |
| `command1 \| command2` | Pipe output to next command |
| `\| more` | Page through output |
| `\| tail -1` | Show last line of output |

---

## 🔧 Troubleshooting

### Permission denied when creating files

```
bash: /etc/test123: Permission denied
```

```bash
# You are not root. Either use sudo or work in your home dir:
cd ~
touch myfile     # ✅ Works
```

---

### Soft link broken / "No such file or directory"

```bash
# The source file was deleted. Remove the dangling link:
rm /tmp/hulk

# Recreate if needed:
ln -s /path/to/source /tmp/hulk
```

---

### `rmdir` fails — "Directory not empty"

```bash
rmdir myfolder        # ❌ Fails if folder has contents

rm -r myfolder        # ✅ Force removes folder + all contents
```

> ⚠️ `rm -r` is irreversible. Double-check before running.

---

## 🤝 Contributing

Pull requests welcome! This is a free, open learning resource.

```bash
git checkout -b feature/your-topic
# make changes
git commit -m "add: your topic"
# submit PR
```

**What we welcome:**
- ✅ Additional commands or flags not covered
- ✅ OS-specific notes (Debian, Arch, RHEL)
- ✅ Shell scripting examples using redirection/pipes
- ✅ Translations

See [CONTRIBUTING.md](CONTRIBUTING.md) for full guidelines.

---

## 👤 Author

**Kelvin R. Tobias**
Software Engineer | AI Engineering Student | Linux & Cloud Practitioner

- 🌐 [kelvinintech.com](https://kelvinintech.com)
- 💻 [github.com/kelvintechnical](https://github.com/kelvintechnical)
- 💼 [Tech Affiliates Community](https://www.linkedin.com/in/kelvintobias)

---

<div align="center">

**If this helped you, please ⭐ star the repo and share it with someone learning Linux.**

*Part of the Linux Ops learning series*

</div>
