# 🚩 Level 5 → Level 6

## Level Goal

The password for the next level is stored somewhere under the `inhere` directory. The target file has all of the following properties:

- Human-readable
- Exactly 1033 bytes in size
- Not executable

## Commands Used

- `ls`
- `cd`
- `find`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit5`, list the contents of the home directory:
```bash
ls
```

You will see a directory named `inhere`. Navigate into it:
```bash
cd inhere
```

List the contents:
```bash
ls
```

You will find 20 subdirectories. Searching through each one manually is not practical. Instead, use `find` with specific filters to match the file properties given in the level goal:
```bash
find . -type f -size 1033c
```

This will output:
```
./maybehere07/.file2
```

Navigate into the located directory:
```bash
cd maybehere07
```

Listing the directory contents reveals several files. The target `.file2` is a hidden file, as its name begins with `.` and it will not appear with a plain `ls`. Use `find` again to confirm its presence:
```bash
find
```

Read the file using `./` to prevent the leading `.` from being misinterpreted:
```bash
cat ./.file2
```

This will output the password for the next level:
```
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **`find` with `-type f`:** Restricts the search to regular files only, excluding directories and symbolic links.
- **`find` with `-size 1033c`:** Filters results to files of exactly 1033 bytes. The `c` suffix denotes bytes; other units include `k` for kilobytes and `M` for megabytes.
- **Combining `find` filters:** Multiple conditions can be chained in a single `find` command, making it a powerful tool for locating files based on precise properties.
- **Hidden files in subdirectories:** Hidden files (prefixed with `.`) can exist inside nested directories and will not appear with a plain `ls`. Using `find` without arguments from within the directory reveals them.

[⬅ Back to Home](../README.md)