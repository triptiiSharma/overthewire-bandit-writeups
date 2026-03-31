# 🚩 Level 7 → Level 8

## Level Goal

The password for the next level is stored in the file `data.txt`, on the same line as the word `millionth`.

## Commands Used

- `ls`
- `grep`
- `ssh`

## Walkthrough

Once logged in as `bandit7`, list the contents of the home directory:
```bash
ls
```

You will find a file named `data.txt`. Opening it directly with `cat` reveals an overwhelming number of word and password pairs — manually scanning through it is not feasible:
```bash
cat data.txt
```

Instead, use `grep` to search for the specific line containing the word `millionth`:
```bash
grep "millionth" data.txt
```

This will output only the matching line, revealing the password:
```
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **`grep`:** Searches through file contents line by line and returns lines that match a given pattern. It is one of the most frequently used tools in Linux for text searching.
- **Syntax — `grep "pattern" filename`:** The pattern is treated as a string by default. Wrapping it in quotes is good practice, especially when the pattern contains spaces or special characters.
- **Efficiency over brute force:** Using `cat` on large files and reading manually is impractical. Reaching for search tools like `grep` early is a core habit in command-line work and is especially important in security contexts where files can contain thousands of entries.

[⬅ Back to Home](../README.md)