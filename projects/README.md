# 🐧 Linux System Navigation & Core Utilities Study

A structured technical study and practical analysis of core Linux command-line utilities. This project explores how system navigation tools interact with the Linux Virtual File System (VFS), system calls, inode metadata, and environment variables.

---

## 📋 Table of Contents
- [Overview](#-overview)
- [Core Utilities Overview](#-core-utilities-overview)
- [System Navigation Mechanics](#-system-navigation-mechanics)
  - [1. Absolute vs. Relative Path Resolution](#1-absolute-vs-relative-path-resolution)
  - [2. Symbolic Links vs. Hard Links](#2-symbolic-links-vs-hard-links)
  - [3. Shell Environment Variables](#3-shell-environment-variables)
- [Practical Hands-On Exercise](#-practical-hands-on-exercise)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [License](#-license)

---

## 🔍 Overview

Navigating the Linux operating system efficiently requires an understanding of the underlying kernel and filesystem mechanics. This repository breaks down how common navigational commands interface with:
* The **Virtual File System (VFS)** hierarchy.
* System calls like `chdir()`, `readdir()`, and `getdents()`.
* File metadata, directory structures, and **inodes**.
* Shell process state variables.

---

## 🛠️ Core Utilities Overview

| Utility | Primary Function | Core System Mechanism | Key Flags |
| :--- | :--- | :--- | :--- |
| **`pwd`** | Print Working Directory | Reads shell state (`$PWD`) or resolves the current directory inode path to the VFS root (`/`). | `-P` (Physical path), `-L` (Logical path) |
| **`cd`** | Change Directory | Shell built-in that invokes the `chdir()` system call to modify process execution context. | `-` (Toggle to `$OLDPWD`), `~` (Expand to `$HOME`) |
| **`ls`** | List Directory Contents | Reads directory files (inode-to-filename mappings) via `getdents()` or `readdir()`. | `-a` (Show hidden), `-l` (Long format), `-h` (Human-readable) |
| **`tree`** | Recursive Directory Visualizer | Recursively traverses directory trees to produce a visual hierarchy representation. | `-L <depth>` (Limit depth), `-d` (Directories only) |
| **`realpath`** | Path Canonicalization | Resolves relative paths, `.`, `..`, and symbolic links to determine true absolute paths. | `-f` (Canonicalize missing/existing components) |

---

## 🔬 System Navigation Mechanics

### 1. Absolute vs. Relative Path Resolution
* **Absolute Paths:** Evaluated top-down starting strictly from the root directory (`/`). Path resolution is independent of your current location.
* **Relative Paths:** Evaluated relative to the current working directory (`.`). Resolution depends on VFS pointer entries:
  * `.` refers to the current directory entry.
  * `..` refers to the parent directory entry in the file system tree.

### 2. Symbolic Links vs. Hard Links
* **Hard Links:** Direct references to an existing file's **inode number**. They share permissions, ownership, and data blocks on disk. Hard links cannot cross physical file systems or target directories.
* **Symbolic (Soft) Links:** Pointer files containing a plain text path targeting another file or directory. They receive a unique inode, can cross storage volumes, and break if the target path is moved or deleted.

* ### 3. Shell Environment Variables
The shell maintains specific environment variables to track state:
* **`$PWD`:** Holds the logical current working directory path.
* **`$OLDPWD`:** Stores the previous working directory (accessed via `cd -`).
* **`$HOME`:** Defines the path to the current user's default home directory.
* **`$PATH`:** Colon-separated list of directories searched when executing binary commands.

* ## 🧪 Practical Hands-On Exercise

Run this verification script in any Linux terminal to observe inode allocation, hard vs. soft link differences, and dynamic path resolution:

```bash
# 1. Create a structured test workspace
mkdir -p ~/nav_study/dir_a/dir_b

# 2. Create a target file and generate both soft and hard links
touch ~/nav_study/dir_a/dir_b/target.txt
ln -s ~/nav_study/dir_a/dir_b/target.txt ~/nav_study/soft_link
ln ~/nav_study/dir_a/dir_b/target.txt ~/nav_study/hard_link

# 3. Inspect Inodes to compare link types
# (Hard link shares the exact same inode number as target.txt)
ls -li ~/nav_study/dir_a/dir_b/target.txt ~/nav_study/soft_link ~/nav_study/hard_link

# 4. Navigate and test path resolution
cd ~/nav_study/dir_a/dir_b
pwd -L   # Prints logical path
pwd -P   # Prints absolute physical path

# 5. Toggle directories using $OLDPWD
cd /var/log
cd -     # Returns to ~/nav_study/dir_a/dir_b

# 6. Cleanup workspace
rm -rf ~/nav_study
