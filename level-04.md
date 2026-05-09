# 🚩 Bandit - Level 3 → 4

## Objective

Find the password stored in a hidden file within the `inhere` directory and use it to log into the next level (bandit4). This level teaches you how to discover and access hidden files in Linux.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit3`|
|Password|`MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx` _(from Level 2 → 3)_|

## Solution

#### Exploring the Directory Structure

After logging in as `bandit3`, list the files in the current directory:

```bash
ls
```

This shows a directory named `inhere`.

#### Checking for Visible Files

List the contents of the `inhere` directory:

```bash
ls inhere
```

No files appear in the output, which suggests the password may be stored in a hidden file.

#### Understanding Hidden Files in Linux

In Linux, files and directories whose names begin with a dot (`.`) are treated as hidden. The standard `ls` command doesn't display them by default to reduce clutter in directory listings.

#### Revealing Hidden Files

To display hidden files, use the `-a` flag:

```bash
ls -a inhere
```

**Command explanation:**

- `ls` - List directory contents
- `-a` - Show "all" files, including hidden ones (filenames starting with `.`)
- `inhere` - The directory to list

This reveals:

```bash
.  ..  ...Hiding-From-You
```

**Understanding the output:**

- `.` - Represents the current directory (`inhere`)
- `..` - Represents the parent directory (home directory)
- `...Hiding-From-You` - The hidden file containing the password

#### Reading the Hidden File

Read the hidden file using its relative path:

```bash
cat inhere/...Hiding-From-You
```

**Command explanation:**

- `cat` - Display file contents
- `inhere/...Hiding-From-You` - Relative path to the file (no need to `cd` into the directory first)

**Alternative methods:**

```bash
cd inhere && cat ...Hiding-From-You     # Change directory first
cat ./inhere/...Hiding-From-You         # Explicit current directory reference
cat ~/inhere/...Hiding-From-You         # Absolute path from home directory
```

The password for the next level is revealed:

```bash
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit4`:

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ ls inhere
bandit3@bandit:~$ ls -a inhere
.  ..  ...Hiding-From-You
bandit3@bandit:~$ cat inhere/...Hiding-From-You
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

## Key Takeaways

- Files and directories beginning with `.` are hidden by default in Linux systems
- `ls -a` displays all files, including hidden ones (the `-a` flag stands for "all")
- `.` always represents the current directory, and `..` represents the parent directory
- Hidden files can be accessed like normal files once you know their names
- You can read files using relative paths without changing directories with `cd`
- Hidden files are commonly used for configuration files (`.bashrc`, `.gitignore`, etc.)

## References

- [Bandit Level 3 → 4](https://overthewire.org/wargames/bandit/bandit4.html)
- [ls Manual Page](https://linux.die.net/man/1/ls)
- [Hidden Files and Directories](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/Linux-Filesystem-Hierarchy.html)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series