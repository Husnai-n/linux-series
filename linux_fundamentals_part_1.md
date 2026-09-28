# Linux Fundamentals - Part 1

## Overview

This guide covers core Linux command-line tools, file searching techniques, and basic shell operator logic. Mastering these fundamental commands is essential for navigating Linux environments during security investigations, system administration tasks, and routine terminal operations.

---

## Basic Navigation & Information Gathering

Understanding your environment and navigating the filesystem efficiently are the first steps when working in a Linux terminal.

### Identifying the Current User
To verify which user account you are currently operating under, use the `whoami` command:

```bash
whoami
```

### Checking Current Working Directory
To display the absolute path of the directory you are currently in, use `pwd`:

```bash
pwd
```

### Listing Directory Contents
The `ls` command lists the files and directories inside a specified folder.

* List contents of the current directory:
  ```bash
  ls
  ```
* List contents of a specific directory without navigating into it:
  ```bash
  ls Pictures
  ```

### Navigating the Filesystem
Use `cd` (change directory) to move between folders:

```bash
cd Documents
```

To move back to the previous parent directory:
```bash
cd ..
```

---

## Interacting with Files & Output

### Displaying Text with `echo`
The `echo` command prints specified text or variable values back to the terminal interface. It is extensively used in script automation and debugging.

```bash
echo "Initiating system check..."
```

### Reading File Contents with `cat`
The `cat` (concatenate) command reads and displays the entire contents of a text file in your terminal output.

* Read a file in the current working directory:
  ```bash
  cat notes.txt
  ```
* Read a file located in a different directory path directly:
  ```bash
  cat /home/ubuntu/Documents/todo.txt
  ```

---

## Searching Files & Logs

Locating relevant data, configuration files, or suspicious log entries quickly is crucial in Linux operations.

### Finding Files with `find`
The `find` command searches through directory structures based on criteria such as filenames, file types, sizes, or timestamps.

* Search for a file by exact name in the current directory and subdirectories:
  ```bash
  find -name passwords.txt
  ```
* Search using a wildcard (`*`) to locate all files with a specific extension:
  ```bash
  find -name "*.txt"
  ```

### Searching Inside Files with `grep`
While `find` searches for filenames, `grep` searches for specific strings or patterns inside the contents of files. This is commonly used during log analysis to filter out relevant entries.

* Search for a specific IP address within an access log file:
  ```bash
  grep "81.143.211.90" access.log
  ```

---

## Terminal Shell Operators

Shell operators allow you to combine commands, control execution flow, and manage output redirection.

### Background Execution Operator (`&`)
Placing a single ampersand at the end of a command executes the process in the background, keeping your current terminal session available for other commands.

```bash
sleep 30 &
```

### Conditional Execution Operator (`&&`)
The double ampersand allows you to run multiple commands sequentially on a single line. Note that the second command will only execute if the first command succeeds without returning an error (exit status 0).

```bash
cd Documents && cat todo.txt
```

### Output Redirection - Overwrite (`>`)
The single redirection operator takes the standard output of a command and writes it into a specified file. If the target file already exists, its contents will be completely overwritten.

```bash
echo "Hello World" > newfile.txt
```

### Output Redirection - Append (`>>`)
The double redirection operator appends the output of a command to the end of a file without destroying or overwriting any existing content.

```bash
echo "New log entry added" >> newfile.txt
```