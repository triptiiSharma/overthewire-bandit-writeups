# 🚩 Bandit - Level 5 → 6

## Objective

Find the password stored in a file somewhere under the `inhere` directory that matches the following specific properties:

- Human-readable
- Exactly `1033` bytes in size
- Not executable

This level teaches you how to use the powerful `find` command to search for files based on multiple criteria simultaneously.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit5`|
|Password|`4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw` _(from Level 4 → 5)_|

## Solution

#### Exploring the Directory Structure

After logging in as `bandit5`, list the files in the home directory:

```bash
ls
```

This shows the `inhere` directory. Inspect its contents:

```bash
ls inhere
```

The directory contains multiple subdirectories (`maybehere00` through `maybehere19`), each potentially containing multiple files. Manually checking each directory and file would be extremely inefficient.

#### Understanding the Challenge

With 20 subdirectories and potentially hundreds of files, we need an automated approach. The challenge provides specific file properties, which makes the `find` command the perfect tool for this task.

#### Using Find with Multiple Criteria

Since we know the exact properties of the target file, construct a `find` command with multiple filters:

```bash
find inhere -type f -size 1033c ! -executable
```

**Command explanation:**

- `find` - Search for files in a directory hierarchy
- `inhere` - Starting directory for the search
- `-type f` - Match only regular files (not directories, symlinks, etc.)
- `-size 1033c` - Match files that are exactly 1033 bytes in size
    - The `c` suffix specifies bytes (characters)
    - Other options: `k` for kilobytes, `M` for megabytes, `G` for gigabytes
    - Use `+1033c` for "greater than" or `-1033c` for "less than"
- `! -executable` - Exclude executable files
    - The `!` (NOT operator) inverts the condition
    - `-executable` matches files with execute permission for the current user

**Note on "human-readable":** Since we're searching with `-type f` (regular files) and excluding executables, we implicitly filter out most binary files. The `file` command could verify this, but the size constraint (1033 bytes) combined with non-executable status makes it highly likely to be text.

This command returns a single match:

```bash
inhere/maybehere07/.file2
```

The target file is `.file2` (a hidden file) in the `maybehere07` subdirectory.

#### Reading the Password File

Read the contents of the identified file:

```bash
cat inhere/maybehere07/.file2
```

**Alternative verification method:**

```bash
file inhere/maybehere07/.file2    # Verify it's ASCII text
ls -lh inhere/maybehere07/.file2   # Confirm size and permissions
```

The password for the next level is revealed:

```bash
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit6`:

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit5@bandit:~$ ls
inhere
bandit5@bandit:~$ find inhere -type f -size 1033c ! -executable
inhere/maybehere07/.file2
bandit5@bandit:~$ cat inhere/maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

## Key Takeaways

- `find` is a powerful tool for locating files based on multiple criteria simultaneously
- `-type f` limits searches to regular files, excluding directories and special files
- `-size 1033c` searches for files of an exact size (the `c` suffix means bytes)
- `! -executable` uses the NOT operator to exclude files with execute permissions
- Combining multiple conditions in `find` eliminates the need for manual directory traversal
- The `find` command recursively searches through subdirectories automatically
- Hidden files (starting with `.`) are included in `find` results by default
- File size can be specified with different units: `c` (bytes), `k` (KB), `M` (MB), `G` (GB)

## References

- [Bandit Level 5 → 6](https://overthewire.org/wargames/bandit/bandit6.html)
- [find Manual Page](https://linux.die.net/man/1/find)
- [Find Command Examples](https://www.tecmint.com/35-practical-examples-of-linux-find-command/)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series