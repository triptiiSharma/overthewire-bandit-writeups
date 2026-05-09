# 🚩 Bandit - Level 4 → 5

## Objective

Find the password stored in the only human-readable file within the `inhere` directory and use it to log into the next level (bandit5). This level teaches you how to identify file types and filter through multiple files efficiently.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit4`|
|Password|`2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ` _(from Level 3 → 4)_|

## Solution

#### Navigating to the Target Directory

After logging in as `bandit4`, list the files in the current directory:

```bash
ls
```

This shows a directory named `inhere`. Navigate into it:

```bash
cd inhere
```

List the files in this directory:

```bash
ls
```

You'll see multiple files named `-file00` through `-file09` - ten files in total.

#### Understanding the Challenge

Opening each file manually would be time-consuming and inefficient. Instead, we need a systematic way to identify which file contains human-readable text versus binary data.

#### Identifying File Types

Use the `file` command to check the type of each file:

```bash
file ./*
```

**Command explanation:**

- `file` - Examines file contents and determines the file type (doesn't rely on extensions)
- `./*` - Wildcard pattern that expands to all files in the current directory
- `./` prefix ensures filenames starting with `-` are treated as paths, not options
- `*` is a glob pattern that matches all files in the current directory

The output reveals the type of each file:

```bash
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

**Understanding the output:**

- `data` - Binary or non-text files (not human-readable)
- `ASCII text` - Plain text file that can be read by humans

Only `./-file07` is marked as ASCII text, making it the target file.

#### Reading the Human-Readable File

Read the identified file:

```bash
cat ./-file07
```

**Alternative methods:**

```bash
cat < -file07                           # Input redirection
grep -r ".*" ./ 2>/dev/null             # Search for any content in readable files
find . -type f -exec file {} \; | grep "ASCII"  # Find and filter in one command
```

The password for the next level is revealed:

```bash
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit5`:

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

## Key Takeaways

- The `file` command identifies file types by examining their content, not their extensions or names
- `ASCII text` indicates a human-readable text file, while `data` typically indicates binary files
- Using `./*` as a wildcard safely handles filenames beginning with `-` by treating them as paths
- The `file` command is invaluable for quickly scanning multiple files without opening each one
- Glob patterns (`*`) expand to match all files in a directory, making batch operations efficient
- Binary files contain non-printable characters and appear as gibberish when viewed with `cat`

## References

- [Bandit Level 4 → 5](https://overthewire.org/wargames/bandit/bandit5.html)
- [file Manual Page](https://linux.die.net/man/1/file)
- [Bash Globbing Patterns](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series