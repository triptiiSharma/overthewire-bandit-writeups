# 🚩 Bandit - Level 1 → 2

## Objective

Find the password stored in a file called `-` located in the home directory and use it to log into the next level (bandit2). This level introduces the challenge of working with special filename characters in Linux.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit1`|
|Password|`263JGJPfgU6LtdEvgfWU1XP5yac29mFx` _(from Level 0 → 1)_|

## Solution

#### Listing Directory Contents

After logging in as `bandit1`, list the files in the current directory:

```bash
ls
```

This shows a file named `-`.

#### Understanding the Challenge

The dash (`-`) character is special in Linux because most commands interpret it as a flag or option prefix. If you try to run `cat -`, the shell treats the dash as a signal for command options or standard input, not as a filename.

#### Reading the Special Filename

To read the file, you need to explicitly specify its path:

```bash
cat ./-
```

**Command explanation:**

- `cat` - Command to display file contents
- `./` - Refers to the current directory (explicit path)
- `-` - The filename
- By prefixing with `./`, the shell treats `-` as a filename in the current directory instead of interpreting it as a command option or standard input

**Alternative methods:**

```bash
cat < -           # Redirect input from the file
cat -- -          # Use -- to signal end of options (not all commands support this)
cat /home/bandit1/-   # Use absolute path
```

The password for the next level is revealed:

```bash
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit2`:

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

## Key Takeaways

- Filenames beginning with `-` are interpreted by the shell as command options or flags
- Prefixing the filename with `./` forces the shell to treat it as a path to a regular file
- Special characters in filenames often require explicit path references or escape sequences
- The `./` notation refers to "current directory" and helps disambiguate special filenames
- Understanding how the shell parses arguments is crucial for handling edge cases

## References

- [Bandit Level 1 → 2](https://overthewire.org/wargames/bandit/bandit2.html)
- [cat Manual Page](https://linux.die.net/man/1/cat)
- [Bash Special Characters](https://tldp.org/LDP/abs/html/special-chars.html)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series