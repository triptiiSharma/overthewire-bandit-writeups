# 🚩 Bandit - Level 8 → 9

## Objective

Find the password for the next level stored in the file `data.txt`. The password is the only line of text that occurs exactly once - all other lines appear multiple times. This level teaches you how to identify unique entries in a dataset using command-line text processing tools.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit8`|
|Password|`dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc` _(from Level 7 → 8)_|

## Solution

#### Exploring the Home Directory

After logging in as `bandit8`, I first listed the files in the home directory:

```bash
ls
```

Output:

```bash
data.txt
```

A file named `data.txt` exists in the home directory.

#### Inspecting the File Structure

To understand what we're working with, I used `cat` to view the contents:

```bash
cat data.txt
```

Output:

```bash
xKsDNVi9P1QxAfpXjRkWnVNwNAROsWL4
TNbE7gPGCQmBnJV8tssvVrEP2sSnI70k
hS72gFEpvw6VepgzyYAUFPntDT1hnQnl
jBe9vdX53Lutc3Ns7y4TDsa2qM9bMEBF
...
```

**Observation:** The file contains hundreds of lines of seemingly random strings. Many lines appear multiple times, and the level hint tells us that only **one line appears exactly once** - that's our password.

**The challenge:** Manually comparing each line to identify the unique one would be impractical and error-prone with this much data.

#### Understanding the Solution Strategy

To find the unique line efficiently, we need to:

1. Group identical lines together
2. Identify which line appears only once

This is where the combination of `sort` and `uniq` becomes powerful.

#### Finding the Unique Line

Use `sort` and `uniq` together with a pipe:

```bash
sort data.txt | uniq -u
```

**Command explanation:**

- `sort data.txt` - Sorts all lines in the file alphabetically
    - This places duplicate lines adjacent to each other
    - Sorting is essential because `uniq` only detects adjacent duplicates
- `|` - Pipe operator: takes the output of the left command and feeds it as input to the right command
- `uniq -u` - Filters to show only unique lines
    - `-u` flag means "unique" - prints only lines that appear exactly once
    - Without `-u`, `uniq` would remove duplicates but keep one copy of repeated lines

**Why sorting is necessary:** The `uniq` command works by comparing consecutive lines. If duplicates aren't adjacent, `uniq` won't detect them. By sorting first, we guarantee all identical lines are grouped together, making `uniq` effective.

**Alternative methods:**

```bash
sort data.txt | uniq -c | grep "1 "          # Count occurrences, find lines with count = 1
awk '{count[$0]++} END {for(line in count) if(count[line]==1) print line}' data.txt
```

Output:

```bash
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

This reveals the password for the next level:

```bash
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

#### Understanding Other uniq Options

```bash
uniq -c data.txt     # Count occurrences of each line
uniq -d data.txt     # Show only duplicate lines (lines that appear more than once)
uniq -u data.txt     # Show only unique lines (lines that appear exactly once)
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit9`:

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit8@bandit:~$ ls
data.txt
bandit8@bandit:~$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

## Key Takeaways

- `sort` arranges lines alphabetically, grouping identical lines together
- `uniq` detects and filters duplicate lines, but only when they're adjacent
- `uniq -u` shows only lines that appear exactly once in the input
- The pipe operator (`|`) connects commands, passing output from one as input to the next
- `sort` and `uniq` are commonly used together for duplicate analysis and data deduplication
- Command chaining with pipes is a fundamental Unix philosophy: small tools that do one thing well, combined for complex tasks
- Other useful `uniq` flags: `-c` (count), `-d` (duplicates only), `-i` (case-insensitive)
- Always sort before using `uniq` unless you're certain the data is already sorted

## References

- [Bandit Level 8 → 9](https://overthewire.org/wargames/bandit/bandit9.html)
- [sort Manual Page](https://linux.die.net/man/1/sort)
- [uniq Manual Page](https://linux.die.net/man/1/uniq)
- [Unix Pipes Tutorial](https://www.geeksforgeeks.org/piping-in-unix-or-linux/)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series