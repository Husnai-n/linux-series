# Linux Fundamentals - Part 2

## Overview

This module covers essential concepts for operational management in Linux systems: remote access using SSH, working with command switches/flags, filesystem interactions, file permissions and security models, user switching, and the Linux Filesystem Hierarchy (FSH).

---

## Remote Access via Secure Shell (SSH)

SSH (Secure Shell) is a cryptographic network protocol used to securely access and manage remote machines over untrusted networks.

### Core Security Guarantees

* **Confidentiality:** End-to-end encryption prevents eavesdropping.
* **Integrity:** Cryptographic hashing ensures transmitted data cannot be altered in transit.
* **Authentication:** Verifies identity using passwords or cryptographic key pairs.

### Establishing an SSH Connection

```bash
# Connect to a remote machine using a username and IP address
ssh username@192.168.1.100

# Connect specifying a custom port (default is 22)
ssh -p 2222 username@192.168.1.100

# Connect using an SSH private key file
ssh -i ~/.ssh/id_rsa username@192.168.1.100
```

---

## Command Flags & Documentation

Command behavior can be extended or modified using switches and flags (prefixed by `-` or `--`).

### Help & Manual Pages

To inspect available switches or detailed options for any command:

```bash
# Display built-in quick help summary
ls --help

# Open the comprehensive manual page for a command
man ls
man chmod
```

---

## Filesystem Interaction Commands

Basic commands used to manage files and directories across the Linux filesystem:

| Command | Full Name | Description | Example Usage |
| :--- | :--- | :--- | :--- |
| `touch` | touch | Creates an empty file or updates timestamps | `touch notes.txt` |
| `mkdir` | make directory | Creates a new directory | `mkdir -p /tmp/test/logs` |
| `cp` | copy | Copies files or directories | `cp report.txt /tmp/` |
| `mv` | move | Moves or renames files/directories | `mv old_name.txt new_name.txt` |
| `rm` | remove | Removes files or directories | `rm -rf /tmp/test` |
| `file` | file | Identifies file type based on headers | `file binary_sample` |

> **Tip:** File paths can be supplied as relative paths (`./file.txt`) or absolute paths (`/home/user/file.txt`).

---

## Linux File Permissions & Access Control

Linux controls file access using a strict permission architecture assigned to three user classes: Owner (User), Group, and Others.

### Structure Breakdown

Viewing file permissions via `ls -l`:

```bash
ls -l
# Output example:
# -rwxr-xr-- 1 user group 4096 Sep 28 10:00 script.sh
```

```text
  -  rwx  r-x  r--
  |   |    |    |
  |   |    |    +---> Others permissions (Read only)
  |   |    +--------> Group permissions (Read & Execute)
  |   +-------------> Owner permissions (Read, Write, Execute)
  +-----------------> File type (- = Regular file, d = Directory)
```

### Numeric (Octal) Permission Representation

Permissions are represented numerically by summing assigned bit values:

| Permission | Symbol | Binary Bit | Value |
| :--- | :---: | :---: | :---: |
| Read | `r` | `100` | **4** |
| Write | `w` | `010` | **2** |
| Execute | `x` | `001` | **1** |

#### Common Octal Configurations

| Symbolic Notation | Octal Notation | Owner | Group | Others | Common Use Case |
| :--- | :---: | :---: | :---: | :---: | :--- |
| `rwxrwxrwx` | **777** | `rwx` (7) | `rwx` (7) | `rwx` (7) | Full access (Security Risk) |
| `rwxr-xr-x` | **755** | `rwx` (7) | `r-x` (5) | `r-x` (5) | Executable scripts, binaries |
| `rw-r--r--` | **644** | `rw-` (6) | `r--` (4) | `r--` (4) | Standard configuration files |
| `rwx------` | **700** | `rwx` (7) | `---` (0) | `---` (0) | Private user tools/scripts |
| `rw-------` | **600** | `rw-` (6) | `---` (0) | `---` (0) | Sensitive files (e.g., SSH keys) |

#### Modifying Permissions (`chmod`)

```bash
# Grant execution rights to owner, read/execute to group and others
chmod 755 script.sh

# Restrict sensitive system note to owner read/write only
chmod 600 secrets.txt

# Apply permissions recursively to an entire directory tree
chmod -R 750 /opt/custom_app/
```

---

## Switching User Contexts

To switch active user sessions or escalate privileges safely:

```bash
# Switch to another user and switch into their home directory
su -l user2

# Execute a command with elevated superuser privileges
sudo systemctl restart network

# Switch directly to the root administrative context
sudo su -
```

---

## Filesystem Hierarchy (FSH) Reference

Linux organizes files inside a single structured tree starting at the root directory (`/`).

| Directory | Full Name / Description |
| :--- | :--- |
| `/` | **Root:** Top-level directory of the entire filesystem structure. |
| `/bin` | **Essential Binaries:** Core binary commands required for system boots (e.g., `ls`, `cp`). |
| `/sbin` | **System Binaries:** Administrative binaries used by root (e.g., `fdisk`, `iptables`). |
| `/etc` | **Configuration:** System-wide configuration files and startup scripts. |
| `/home` | **User Home Directories:** Personal data storage for regular users. |
| `/root` | **Root Home:** Dedicated home directory for the root superuser account. |
| `/var` | **Variable Data:** Dynamic files including system logs (`/var/log`), mail, and spool files. |
| `/tmp` | **Temporary Files:** Storage for temporary files, frequently cleared upon reboot. |
| `/usr` | **User Programs:** Secondary hierarchy containing user binaries, documentation, and libraries. |
| `/lib` | **Shared Libraries:** System libraries required by binaries in `/bin` and `/sbin`. |
| `/boot` | **Boot Files:** Bootloader files, Linux kernel images, and initrd files. |
| `/dev` | **Device Files:** Hardware abstraction points (e.g., `/dev/sda`, `/dev/null`). |
| `/proc` | **Process Info:** Virtual pseudo-filesystem exposing running kernel and process states. |
| `/sys` | **System & Hardware Info:** Virtual filesystem exporting kernel parameters and hardware devices. |
| `/media` | **Removable Media:** Automated mount points for removable storage (USBs, optical drives). |
| `/mnt` | **Manual Mount:** Temporary mount point for filesystems mounted manually by administrators. |
| `/opt` | **Optional Software:** Third-party add-on application software packages. |
| `/run` | **Runtime Data:** Volatile data describing system state since the last boot. |