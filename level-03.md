# 🚩 Bandit - Level 2 → 3

## Objective

Find the password stored in a file called `spaces in this filename` located in the home directory and use it to log into the next level (bandit3). This level introduces the challenge of handling filenames containing spaces.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit2`|
|Password|`263JGJPfgU6LtdEvgfWU1XP5yac29mFx` _(from Level 1 → 2)_|

## Solution

#### Listing Directory Contents

After logging in as `bandit2`, list the files in the current directory:

```bash
ls
```

This shows a file named `spaces in this filename`.

#### Understanding the Challenge

Spaces in filenames create problems because the shell uses spaces to separate command arguments. Without proper handling, `cat spaces in this filename` would be interpreted as trying to read four separate files: `spaces`, `in`, `this`, and `filename`.

#### Reading the File with Spaces

To read the file correctly, enclose the filename in quotes:

```bash
cat "spaces in this filename"
```

**Command explanation:**

- `cat` - Command to display file contents
- `"spaces in this filename"` - The entire filename enclosed in quotes
- Quotes tell the shell to treat everything inside as a single argument, preserving the spaces

**Alternative methods:**

```bash
cat 'spaces in this filename'        # Single quotes work too
cat spaces\ in\ this\ filename        # Escape each space with backslash
cat ./spaces\ in\ this\ filename      # Combine path prefix with escaping
cat "$(ls)"                           # Command substitution (if only one file exists)
```

**Pro tip:** Use tab completion to automatically escape special characters - type `cat sp` then press Tab, and the shell will complete it as `cat spaces\ in\ this\ filename`.

The password for the next level is revealed:

```bash
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit3`:

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat "spaces in this filename"
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

## Key Takeaways

- Spaces in filenames must be quoted or escaped because the shell uses spaces to separate arguments
- Double quotes (`"..."`) and single quotes (`'...'`) both preserve spaces in filenames
- Backslash (`\`) can escape individual space characters: `spaces\ in\ this\ filename`
- Tab completion automatically handles special characters and is the fastest method in practice
- Best practice: Avoid using spaces in filenames when creating files - use underscores or hyphens instead

## References

- [Bandit Level 2 → 3](https://overthewire.org/wargames/bandit/bandit3.html)
- [Bash Quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)
- [cat Manual Page](https://linux.die.net/man/1/cat)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series