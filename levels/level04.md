# 🚩 Level 3 → Level 4

## Level Goal

The password for the next level is stored in a hidden file inside the `inhere` directory.

## Commands Used

- `ls`
- `cd`
- `find`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit3`, list the contents of the home directory:
```bash
ls
```

You will see a directory named `inhere`. Navigate into it:
```bash
cd inhere
```

Running a plain `ls` here will show nothing, as the file is hidden. In Linux, hidden files are prefixed with a `.` and are not shown by default. Use `find` to reveal all files including hidden ones:
```bash
find
```

This will output:
```
./...Hiding-From-You
```

Read the file using `./` to ensure the filename is not misinterpreted as a command-line option:
```bash
cat ./...Hiding-From-You
```

This will output the password for the next level:
```
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Hidden files:** In Linux, any file or directory whose name begins with `.` is treated as hidden and is not displayed by `ls` by default. Use `ls -a` or `find` to reveal them.
- **`find` command:** When run without arguments, `find` recursively lists all files and directories from the current location, including hidden ones.
- **`./` prefix:** Prevents the shell from misinterpreting filenames that begin with special characters as command-line flags.

[⬅ Back to Home](../README.md)