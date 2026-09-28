# Linux Fundamentals - Part 3

## Overview

This guide covers core operational concepts in Linux environments, including terminal text editors, network file retrieval and transfer mechanisms, process management, service control using `systemd`, backgrounding/foregrounding tasks, and scheduled automation via `cron`.

---

## Terminal Text Editors

Editing configuration files and scripts directly in the terminal is a fundamental administrative skill.

### 1. Nano
`nano` is a beginner-friendly, straightforward command-line text editor available on most Linux distributions.

```bash
# Open or create a file in nano
nano myfile.txt
```

**Key Navigation Shortcuts:**
* `Ctrl + O` : Save (Write Out) changes.
* `Ctrl + X` : Exit the editor.
* `Ctrl + W` : Search for text within the file.
* `Ctrl + _` : Jump to a specific line number.
* `Ctrl + C` : View current line number and cursor position.

### 2. VIM (Vi IMproved)
`VIM` is a highly configurable, powerful text editor present across nearly all UNIX-like systems, making it essential when operating in minimal environments without graphical tools or lightweight editors.

```bash
# Open or create a file in VIM
vim myfile.txt
```

**Core Operational Modes:**
* **Normal Mode (Default):** Used for navigation and running editor commands. Press `Esc` to enter Normal mode.
* **Insert Mode:** Used for editing text. Press `i` in Normal mode to start typing.
* **Command Mode:** Used to save or exit. Type `:` from Normal mode:
  * `:w` — Save file.
  * `:q!` — Quit without saving.
  * `:wq` — Save changes and quit.

---

## File Retrieval & Transfer Techniques

Transferring payload files, logs, and security tools across network endpoints is central to systems management and security assessments.

### 1. Web Downloads with `wget`
`wget` retrieves files directly over protocols such as HTTP, HTTPS, and FTP.

```bash
wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt
```

### 2. Secure File Transfer with `scp` (SSH)
`scp` (Secure Copy Protocol) uses the SSH protocol to encrypt file transfers between remote systems securely.

```bash
# Syntax: scp <source_file> <user>@<remote_ip>:<destination_path>
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt

# Download a file from a remote host to local machine
scp ubuntu@192.168.1.30:/var/log/syslog ./syslog_copy.txt
```

### 3. Serving Local Files via Python HTTP Server
To quickly share files across local networks, Python's built-in `http.server` module transforms any directory into a lightweight web server.

#### Step-by-step Setup:
1. **On the host sharing files:** Navigate to the folder containing target files and start the web server on default port `8000`:
   ```bash
   python3 -m http.server 8000
   ```
2. **On the receiving machine:** Fetch the target file using `wget` or `curl`:
   ```bash
   wget http://<host_ip>:8000/sample_file.txt
   ```

---

## Linux Process Management

Every running application or background task in Linux is represented as a process assigned a unique Process Identifier (**PID**).

### Monitoring Active Processes

* **`ps` (Process Status):** Displays active processes associated with the current terminal session.
  ```bash
  ps
  ```
* **`ps aux`:** Displays detailed information for all running processes across all users and system daemons.
  ```bash
  ps aux
  ```
  * `a` — Show processes for all users.
  * `u` — Display user-oriented format (CPU/Memory utilization).
  * `x` — Include processes not attached to a terminal (daemons).

* **`top`:** Interactive real-time process manager updating resource utilization dynamically.
  ```bash
  top
  ```

### Terminating Processes (`kill`)
Processes can be controlled or terminated by sending specific kernel signals using `kill`:

```bash
# Terminate process using its PID safely (SIGTERM)
kill 1234

# Forcefully terminate a stuck process (SIGKILL)
kill -9 1234

# Suspend/pause a running process (SIGSTOP)
kill -18 1234
```

| Signal Name | Code | Behavior |
| :--- | :---: | :--- |
| `SIGTERM` | `15` | Requests elegant process termination, allowing cleanup. |
| `SIGKILL` | `9` | Forces immediate process termination without cleanup. |
| `SIGSTOP` | `19` | Pauses execution of the target process. |

---

## Service Management (`systemd` & `systemctl`)

`systemd` is the default init system in modern Linux distributions responsible for bootstrapping the user space and managing system services (daemons).

### Service Management Commands
To manage services using `systemctl`:

```bash
# Check service operational status
systemctl status apache2

# Start a service
systemctl start apache2

# Stop a running service
systemctl stop apache2

# Enable service to launch automatically on system boot
systemctl enable apache2

# Disable service auto-start at boot
systemctl disable apache2
```

---

## Process Execution Control: Backgrounding & Foregrounding

Linux allows running tasks interactively in the foreground or asynchronously in the background.

```bash
# Run a process directly in the background using the '&' operator
sleep 100 &
# Terminal Output: [1] 2341 (Job ID and PID)

# Pause an active foreground process and send to background
# Press: Ctrl + Z

# List current background jobs
jobs

# Bring background job back to active foreground focus
fg %1
```

---

## Automated Task Scheduling (`cron` & `crontab`)

The `cron` daemon handles automated execution of scheduled background tasks based on defined system schedules known as `crontabs`.

### Crontab Syntax Structure

A crontab entry consists of 6 fields separated by spaces:

```text
*  *  *  *  *  <command_to_execute>
|  |  |  |  |
|  |  |  |  +----> Day of Week (0 - 6) (Sunday=0)
|  |  |  +-------> Month of Year (1 - 12)
|  |  +----------> Day of Month (1 - 31)
|  +-------------> Hour (0 - 23)
+----------------> Minute (0 - 59)
```

### Management Commands
```bash
# Edit current user's crontab schedule
crontab -e

# View scheduled crontab entries
crontab -l
```

### Example Configurations

* **Execute a directory backup every 12 hours:**
  ```text
  0 */12 * * * cp -R /home/ubuntu/Documents /var/backups/
  ```
* **Execute a cleanup script every Sunday at midnight:**
  ```text
  0 0 * * 0 /usr/local/bin/cleanup.sh
  ```

> **Note:** The asterisk (`*`) acts as a wildcard representing every possible unit value for that specific position.