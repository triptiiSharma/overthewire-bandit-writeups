# 🚩 Bandit - Level 9 → 10

## Objective

Find the password for the next level stored in the file `data.txt`. The password is one of the few human-readable strings in the file, preceded by several `=` characters. This level introduces working with binary files and extracting readable text from non-text data.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit9`|
|Password|`4CKMh1JI91bUIZZPXDqGanal4xvAg0JM` _(from Level 8 → 9)_|

## Solution

#### Exploring the Home Directory

After logging in as `bandit9`, I first listed the files in the home directory:

```bash
ls
```

Output:

```bash
data.txt
```

A file named `data.txt` exists in the home directory.

#### Attempting to Read the File

I tried to display the file's contents using `cat`:

```bash
cat data.txt
```

Output:

```bash
MA׺�ƹ�7Z3�...
```

**Observation:** The output contains gibberish characters, control sequences, and unreadable symbols - clear indicators that this is a binary file, not plain text.

**Why binary files are unreadable:** Binary files contain raw bytes that may represent images, executables, compressed data, or other non-text formats. When you try to display them with `cat`, the terminal interprets these bytes as characters, producing garbage output and potentially messing up your terminal display.

#### Searching for the Pattern

Since the hint mentioned the password is preceded by several `=` characters, I tried using `grep` to search for that pattern:

```bash
grep "=" data.txt
```

Output:

```bash
grep: data.txt: binary file matches
```

**What this means:** `grep` detected the pattern but refuses to display results because it recognized the file as binary. By default, `grep` protects you from accidentally dumping binary data to your terminal.

#### Understanding the Solution

To extract human-readable strings from binary files, we need the `strings` command. This tool scans the file for sequences of printable characters (typically 4+ consecutive printable characters) and extracts them.

#### Extracting Readable Text

Use `strings` to extract printable text, then pipe it to `grep` to find lines with `=`:

```bash
strings data.txt | grep "="
```

**Command explanation:**

- `strings` - Extracts all human-readable text sequences from binary files
    - By default, it looks for sequences of at least 4 printable characters
    - Ignores non-printable bytes and control characters
- `|` - Pipe operator: passes the output of `strings` to `grep`
- `grep "="` - Filters the readable strings to show only lines containing the `=` character

Output:

```bash
========== the
I\=Ow
V?L=
%3=VZ
========== password
={M\
========== is
=Dvq
=n/N
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
zX]%=
]\{=
```

**Analyzing the output:** Multiple lines contain `=` characters, but the password stands out because it's preceded by many `=` signs (exactly as the hint described):

```bash
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

The pattern also reveals the full phrase: "the password is FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey"

#### Alternative Methods

**More specific grep pattern:**

```bash
strings data.txt | grep "===="          # Match multiple equals signs
strings data.txt | grep -E "=+ [A-Za-z0-9]+"  # Regex for equals followed by alphanumeric
```

**Using grep with binary file handling:**

```bash
grep -a "====" data.txt                 # Force grep to treat as text (-a flag)
```

**Adjusting strings minimum length:**

```bash
strings -n 8 data.txt | grep "="        # Only strings of 8+ characters
```

The password for the next level is revealed:

```bash
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit10`:

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit9@bandit:~$ ls
data.txt
bandit9@bandit:~$ strings data.txt | grep "="
========== the
I\=Ow
V?L=
%3=VZ
========== password
={M\
========== is
=Dvq
=n/N
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
zX]%=
]\{=
```

## Key Takeaways

- Binary files contain non-text data that appears as gibberish when displayed with `cat`
- `grep` detects patterns in binary files but refuses to display them by default to protect your terminal
- `strings` extracts human-readable text sequences from binary files (default: 4+ consecutive printable characters)
- Piping `strings` into `grep` is a powerful technique for finding readable secrets hidden in binary data
- The `-n` flag in `strings` allows you to adjust the minimum string length threshold
- `grep -a` forces `grep` to treat binary files as text, but `strings` is usually cleaner
- Combining simple commands with pipes (`|`) enables sophisticated text processing workflows
- Binary files are common in CTFs and real-world scenarios (memory dumps, network captures, executables)

## References

- [Bandit Level 9 → 10](https://overthewire.org/wargames/bandit/bandit10.html)
- [strings Manual Page](https://linux.die.net/man/1/strings)
- [grep Manual Page](https://linux.die.net/man/1/grep)
- [Binary File Formats](https://en.wikipedia.org/wiki/Binary_file)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series