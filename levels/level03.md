# 🚩 Level 2 → Level 3

## Level Goal

The password for the next level is stored in a file called `--spaces in this filename--`, located in the home directory.

## Commands Used

- `ls`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit2`, list the contents of the home directory:
```bash
ls
```

You will see a file named `--spaces in this filename--`. Reading it directly with `cat --spaces in this filename--` will not work, as the shell treats each space-separated word as a separate argument. There are two ways to handle this:

**Method 1 — Escape each space with a backslash:**
```bash
cat ./--spaces\ in\ this\ filename--
```

**Method 2 — Wrap the filename in double quotes:**
```bash
cat "./--spaces in this filename--"
```

Either method will output the password for the next level:
```
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Spaces in filenames:** The shell uses spaces as argument delimiters by default. A filename containing spaces must be handled explicitly to be treated as a single argument.
- **Backslash escaping:** Placing a `\` before a space tells the shell to treat the space as a literal character and not a delimiter.
- **Quoting filenames:** Wrapping a filename in double quotes `" "` preserves spaces and treats the entire string as a single argument.
- **`./` prefix:** Carried forward from the previous level — used to distinguish a filename from a command-line option when the name begins with a special character.

[⬅ Back to Home](../README.md)