# 🚩 Bandit - Level 11 → 12

## Objective

Find the password for the next level stored in the file `data.txt`, where all lowercase `(a-z)` and uppercase `(A-Z)` letters have been rotated by 13 positions. This level introduces classical cipher decoding using built-in Linux text transformation tools.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit11`|
|Password|`dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr` (from Level 10 → 11)|

---

## Solution

#### Exploring the Home Directory

After logging in as `bandit11`, I listed the files in the home directory:

```bash
ls
```

Output:

```bash
bandit11@bandit:~$ ls
data.txt
```

A file named `data.txt` exists in the home directory.

#### Inspecting the File

To view its contents, I used `cat`:

```bash
cat data.txt
```

Output:

```bash
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```

The text is readable but clearly scrambled — real words like "The password is" are hidden in there, just shifted. The level hint confirms this is **ROT13** encoding.

#### Understanding ROT13

ROT13 is a simple substitution cipher based on the Caesar cipher. It shifts every letter in the alphabet forward by 13 positions, wrapping around at the ends:

```
A → N,  B → O,  C → P  ...  M → Z
N → A,  O → B,  P → C  ...  Z → M
```

Since the alphabet has 26 letters, rotating by 13 twice returns you to the original character — meaning ROT13 is its own inverse. Encoding and decoding are the exact same operation.

Numbers, spaces, and special characters are left unchanged.

#### Decoding the ROT13 Text

To decode it, use the `tr` (translate) command to perform character substitution:

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Command explanation:

- `cat data.txt` — reads the encoded file contents
- `|` — pipe operator: passes the output to `tr`
- `tr 'A-Za-z' 'N-ZA-Mn-za-m'` — translates each letter 13 positions forward
    - `'A-Za-z'` — the input character set: all uppercase and lowercase letters
    - `'N-ZA-Mn-za-m'` — the output character set: the alphabet rotated by 13 positions
    - `tr` maps each character in the first set to the corresponding character in the second set positionally

You can also pass the string directly using a here-string instead of `cat`:

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m' <<< "Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4"
```

Here, `<<<` is a **here-string** — it passes a string literal directly as standard input to the command, without needing to pipe from `cat` or create a file.

Alternative methods:

```bash
echo "Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Output:

```bash
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

The password for the next level is revealed:

```bash
7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit12`:

```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

---

## Terminal Output

```bash
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

---

## Key Takeaways

- ROT13 is a Caesar cipher that shifts each letter by 13 positions in the alphabet
- Because the alphabet has 26 letters, applying ROT13 twice always returns the original text — it is self-inverse
- The `tr` command performs character-by-character substitution, making it ideal for simple ciphers like ROT13
- `tr 'A-Za-z' 'N-ZA-Mn-za-m'` is the standard one-liner for ROT13 decoding on Linux
- `<<<` (here-string) passes a string directly as stdin, avoiding the need for `echo` or `cat`
- ROT13 is not encryption — it offers no security and is trivially reversible
- Numbers and special characters are unaffected by ROT13, which is why the password portion of the string stays unchanged

---

## References

- [Bandit Level 11 → 12](https://overthewire.org/wargames/bandit/bandit12.html)
- [tr Manual Page](https://linux.die.net/man/1/tr)
- [ROT13 – Wikipedia](https://en.wikipedia.org/wiki/ROT13)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series