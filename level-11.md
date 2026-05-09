# 🚩 Bandit - Level 10 → 11

## Objective

Find the password for the next level stored in the file `data.txt`, which contains Base64 encoded data. This level introduces encoding schemes and how to decode them using built-in Linux utilities.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit10`|
|Password|`FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey` (from Level 9 → 10)|

---

## Solution

#### Exploring the Home Directory

After logging in as `bandit10`, I listed the files in the home directory:

```bash
ls
```

Output:

```bash
bandit10@bandit:~$ ls
data.txt
```

A file named `data.txt` exists in the home directory.

#### Inspecting the File

To view the contents of the file, I used `cat`:

```bash
cat data.txt
```

Output:

```bash
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```

The content is clearly not plain text — it's a string of seemingly random alphanumeric characters ending with `==`. This is a recognizable indicator of **Base64 encoding**.

#### Understanding Base64

Base64 is a binary-to-text encoding scheme that represents binary data using a set of 64 printable ASCII characters (`A–Z`, `a–z`, `0–9`, `+`, `/`). It is commonly used to safely transmit binary data over text-based systems like email or HTTP.

Key identifiers of Base64 encoded data:

- Contains only alphanumeric characters, `+`, and `/`
- Often ends with `=` or `==` padding characters (used to make the length a multiple of 4)
- The length is always a multiple of 4

#### Decoding the Base64 Data

To decode the Base64 string, use the `base64` command with the `-d` flag:

```bash
cat data.txt | base64 -d
```

Command explanation:

- `cat data.txt` — reads and outputs the contents of `data.txt`
- `|` — pipe operator: passes the output of `cat` as input to the next command
- `base64 -d` — decodes the Base64 encoded input (`-d` stands for decode)

Alternative methods:

```bash
base64 -d data.txt              # Pass the file directly instead of using cat
base64 --decode data.txt        # Long-form flag for clarity
```

Output:

```bash
bandit10@bandit:~$ cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

The password for the next level is revealed:

```bash
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit11`:

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

---

## Terminal Output

```bash
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
bandit10@bandit:~$ cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

---

## Key Takeaways

- Base64 encodes binary or text data into printable ASCII characters for safe transmission
- Trailing `=` or `==` padding is a strong indicator that data is Base64 encoded
- `base64 -d` decodes Base64 encoded input back to its original form
- You can pass file contents to `base64 -d` either via `cat` and a pipe, or directly as a file argument
- Base64 is an encoding scheme, not encryption — it provides no security and can be trivially reversed
- Base64 is widely used in web tokens (JWT), email attachments (MIME), and data URIs

---

## References

- [Bandit Level 10 → 11](https://overthewire.org/wargames/bandit/bandit11.html)
- [base64 Manual Page](https://linux.die.net/man/1/base64)
- [Base64 Encoding Explained](https://en.wikipedia.org/wiki/Base64)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series