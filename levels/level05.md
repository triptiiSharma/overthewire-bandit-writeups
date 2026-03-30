# 🚩 Level 4 → Level 5

## Level Goal

The password for the next level is stored in the only human-readable file inside the `inhere` directory, which contains multiple files.

## Commands Used

- `ls`
- `cd`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit4`, list the contents of the home directory:
```bash
ls
```

You will see a directory named `inhere`. Navigate into it:
```bash
cd inhere
```

List the contents of the directory:
```bash
ls
```

You will find 10 files named `-file00` through `-file09`. Most of these contain binary or non-readable data. The goal is to identify the one human-readable file.

**Method 1 — Manual approach (check each file one by one):**
```bash
cat ./-file00
cat ./-file01
cat ./-file02
# ... and so on
```

The human-readable output is found in `-file07`:
```bash
cat ./-file07
```

**Method 2 — Efficient approach using `file` command (recommended):**
```bash
file ./*
```

This will display the file type of each file in the directory, making it easy to identify the ASCII text file without opening each one individually.

Either method will lead you to the password:
```
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Human-readable files:** A human-readable file contains plain ASCII or Unicode text, as opposed to binary data which appears as garbled characters in the terminal.
- **`file` command:** Determines and displays the type of a file based on its content rather than its extension. Useful for quickly identifying readable files among a large set.
- **`reset` command:** If the terminal display gets corrupted after printing binary content, `reset` restores it to a clean, usable state.
- **`./` prefix:** Used here again since all filenames begin with `-`, which the shell would otherwise interpret as command-line flags.

[⬅ Back to Home](../README.md)