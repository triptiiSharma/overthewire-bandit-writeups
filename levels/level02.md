# 🚩 Level 1 → Level 2

## Level Goal

The password for the next level is stored in a file called `-`, located in the home directory.

## Commands Used

- `ls`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit1`, list the contents of the home directory:
```bash
ls
```

You will see a file named `-`. Reading it directly with `cat -` will not work, as the shell interprets the leading dash as a command-line flag rather than part of the filename. To explicitly reference it as a file, prefix the name with `./`:
```bash
cat ./-
```

This will output the password for the next level:
```
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Dashed filenames:** A filename beginning with `-` is misinterpreted by the shell as a command-line option or flag. To treat it as a literal filename, prefix it with `./` (current directory path) or use `--` before the filename to signal end of options.
- **`./` prefix:** Explicitly tells the shell to look for the file in the current directory, bypassing any option parsing.

[⬅ Back to Home](../README.md)