# 🚩 Bandit - Level 17 → 18

## Objective

Find the password for the next level. Two files exist in the home directory — `passwords.old` and `passwords.new`. The password for the next level is the **only line that has changed** between the two files. This level introduces file comparison using the `diff` command.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit17`|
|Password|`EReVavePLFHtFlFsjn3hyzMlvSuSAcRD` (from `/etc/bandit_pass/bandit17`)|

---

## Solution

#### Exploring the Home Directory

After logging in as `bandit17`, I listed the files in the home directory:

```bash
ls
```

Output:

```bash
passwords.new  passwords.old
```

Two files are present. Both contain a large number of password-like strings — the challenge is to identify the single line that differs between them.

#### Understanding the Challenge

Both files contain hundreds of lines. Manually comparing them line by line would be impractical. The `diff` command is purpose-built for exactly this — it compares two files and reports only what changed.

#### Comparing the Files with diff

```bash
diff passwords.new passwords.old
```

Command explanation:

- `diff` — compares two files line by line and outputs the differences
- `passwords.new` — the first file (treated as the "new" version)
- `passwords.old` — the second file (treated as the "old" version)

Output:

```
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> 390zFj2NETFVZkqYw8UEFdN6h40oGVtT
```

Understanding the `diff` output:

- `42c42` — line 42 in the first file was **changed** (`c`) relative to line 42 in the second file
- `<` — indicates the line from the first file (`passwords.new`)
- `>` — indicates the line from the second file (`passwords.old`)
- `---` — separator between the two differing versions

The line from `passwords.new` (marked with `<`) is:

```
x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

This is the password for the next level.

#### Logging into the Next Level

Use the retrieved password to log in as `bandit18`:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

**Note:** As soon as you log in as `bandit18`, the connection closes immediately with `Byebye!`. This is intentional and is the challenge for Level 18 → 19. To work around it, pass a command directly to SSH without opening an interactive shell:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'
```

Output:

```
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

---

## Terminal Output

```bash
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> 390zFj2NETFVZkqYw8UEFdN6h40oGVtT
PS C:\Users\pandi> ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

---

## Key Takeaways

- `diff` compares two files line by line and reports what changed, what was added, and what was removed
- In diff output, `<` denotes a line from the first file and `>` denotes a line from the second file
- `42c42` means line 42 was **changed** — other codes include `a` (added) and `d` (deleted)
- `diff` is far more efficient than manual comparison for large files with small differences
- SSH allows you to pass a command directly after the connection details to run it non-interactively: `ssh user@host 'command'`
- When a shell is configured to exit immediately on login (like bandit18's `.bashrc`), non-interactive SSH command execution is a reliable workaround

---

## References

- [Bandit Level 17 → 18](https://overthewire.org/wargames/bandit/bandit18.html)
- [diff Manual Page](https://linux.die.net/man/1/diff)
- [SSH Manual Page](https://linux.die.net/man/1/ssh)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series