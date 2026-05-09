# 🚩 Bandit - Level 7 → 8

## Objective

Find the password for the next level stored in the file `data.txt` next to the word `millionth`. This level introduces text searching and pattern matching in large files.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit7`|
|Password|`morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj` _(from Level 6 → 7)_|

## Solution

#### Exploring the Home Directory

After logging in as `bandit7`, I first listed the files in the home directory:

```bash
ls
```

Output:

```bash
data.txt
```

A file named `data.txt` exists in the home directory.

#### Inspecting the File

To get a sense of the file's structure, I used `cat` to view its contents:

```bash
cat data.txt
```

Output:

```bash
bodyguards      ioPDlA0BWGGuFUBd3178xaGlAxESJGA7
scalpers        XHVfzQafBYdvMbROF9HM90z5eHX6Gck8
valedictories   oz0ms7pZLC9uXcVnhlPqadvRF1IwV4qV
Pentecost       CAVmMMRw9O7E3SEej3F6tyr48qtw4uZy
...
```

**File structure:** The file contains thousands of lines, each with a word followed by a password-like string (separated by tabs or spaces).

**The problem:** Manually scrolling through thousands of lines to find "millionth" would be extremely inefficient and error-prone.

#### Understanding the Solution

Since the level hint tells us the password is next to the word **"millionth"**, we can use `grep` - a powerful text search tool - to find that specific line instantly.

#### Searching for the Target Word

Use `grep` to search for "millionth" in the file:

```bash
grep -w "millionth" data.txt
```

**Command explanation:**

- `grep` - Global Regular Expression Print - searches for patterns in text files
- `-w` - Match whole words only (prevents matching partial words like "millionth**s**" or "**a**millionth")
- `"millionth"` - The search pattern (the word we're looking for)
- `data.txt` - The file to search in

**Why use `-w`?** Without the `-w` flag, `grep` would match any line containing "millionth" as a substring. The `-w` flag ensures we only match complete words, which is more precise and avoids false positives.

**Alternative methods:**

```bash
grep "millionth" data.txt           # Works here, but less precise
awk '/millionth/ {print}' data.txt  # Using awk for pattern matching
sed -n '/millionth/p' data.txt      # Using sed to print matching lines
```

Output:

```bash
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

The password for the next level is revealed:

```bash
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

#### Extracting Just the Password (Optional)

If you want only the password without the word "millionth", you can use additional tools:

```bash
grep -w "millionth" data.txt | awk '{print $2}'
```

or

```bash
grep -w "millionth" data.txt | cut -f2
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit8`:

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ grep -w "millionth" data.txt
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

## Key Takeaways

- `grep` is essential for searching text patterns in files, especially large ones
- The `-w` flag matches whole words only, preventing partial matches
- `grep` searches line-by-line and returns all matching lines
- Text search tools are far more efficient than manual inspection for large files
- `grep` uses regular expressions for powerful pattern matching (though simple strings work too)
- Common `grep` flags: `-i` (case-insensitive), `-v` (invert match), `-n` (show line numbers), `-c` (count matches)
- Combining `grep` with pipes (`|`) and other tools like `awk` or `cut` enables powerful text processing

## References

- [Bandit Level 7 → 8](https://overthewire.org/wargames/bandit/bandit8.html)
- [grep Manual Page](https://linux.die.net/man/1/grep)
- [Regular Expressions Tutorial](https://www.regular-expressions.info/)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series