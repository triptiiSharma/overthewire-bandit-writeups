# 🚩 Bandit - Level 0 → 1

## Objective

Find the password stored in the file `readme` located in the home directory and use it to log into the next level (bandit1).

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit0`|
|Password|`bandit0`|

## Solution

#### Listing Directory Contents

After logging in as `bandit0`, start by listing the files in the current directory:

```bash
ls
```

**Command explanation:**

- `ls` - Lists files and directories in the current working directory
- By default, it shows non-hidden files in alphabetical order

This reveals a file named `readme` in the home directory.

#### Reading the Password File

To view the contents of the `readme` file, use the `cat` command:

```bash
cat readme
```

**Command explanation:**

- `cat` - Concatenates and displays file contents to standard output
- `readme` - The filename to read
- `cat` is ideal for quickly viewing small text files

The file contains the password for the next level:

```bash
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit1`:

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

**Note:** Only the username changes between levels - the host (`bandit.labs.overthewire.org`) and port (`2220`) remain constant throughout the Bandit series.

## Terminal Output

```bash
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
The password you are looking for is:
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

## Key Takeaways

- `ls` lists files and directories in the current working directory
- `cat` displays the contents of text files to the terminal
- Bandit passwords are typically stored in files within the home directory
- The SSH host and port remain consistent across all Bandit levels - only the username changes
- The tilde (`~`) in the prompt represents the user's home directory

## References

- [Bandit Level 0 → 1](https://overthewire.org/wargames/bandit/bandit1.html)
- [ls Manual Page](https://linux.die.net/man/1/ls)
- [cat Manual Page](https://linux.die.net/man/1/cat)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series