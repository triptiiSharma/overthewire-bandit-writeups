# 🚩 Level 9 → Level 10

## Level Goal

The password for the next level is stored in `data.txt` among several human-readable strings, preceded by several `=` characters.

## Commands Used

- `ls`
- `strings`
- `grep`
- `ssh`

## Walkthrough

Once logged in as `bandit9`, list the contents of the home directory:
```bash
ls
```

You will find a file named `data.txt`. Unlike previous levels, this file is binary — running `cat` on it will produce garbled, unreadable output.

Use the `strings` command to extract only the human-readable sequences from the binary file:
```bash
strings data.txt
```

This outputs a number of readable strings, but the list is still long. According to the level goal, the password is preceded by several `=` characters. Pipe the output into `grep` to filter for lines containing `=`:
```bash
strings data.txt | grep "="
```

This will narrow the output down significantly, revealing the password:
```
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Binary files:** Files that contain non-text data, such as compiled programs or encoded content. They cannot be read meaningfully with `cat` and require specialised tools to extract useful information.
- **`strings`:** Scans a file and extracts sequences of printable characters of a minimum length (default: 4 characters). Commonly used in reverse engineering and binary analysis to find human-readable content embedded in non-text files.
- **Chaining `strings` with `grep`:** A practical pattern for narrowing down readable content in binary files when you have a known marker — in this case, the `=` characters preceding the password.

[⬅ Back to Home](../README.md)