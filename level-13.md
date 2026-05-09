# 🚩 Bandit - Level 12 → 13

## Objective

Find the password for the next level stored in the file `data.txt`, which is a hexdump of a file that has been repeatedly compressed. This level teaches you how to reverse a hexdump and systematically peel back multiple layers of compression.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit12`|
|Password|`7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4` (from Level 11 → 12)|

---

## Solution

#### Exploring the Home Directory

After logging in as `bandit12`, I listed the files in the home directory:

```bash
ls
```

Output:

```bash
bandit12@bandit:~$ ls
data.txt
```

#### Inspecting the File

To understand what we're working with, I used `cat` to view the file:

```bash
cat data.txt
```

Output:

```bash
bandit12@bandit:~$ cat data.txt
00000000: 1f8b 0808 10da cf69 0203 6461 7461 322e
...
```

The file contains a **hexdump** — a human-readable representation of binary data where each byte is shown as two hexadecimal digits. Before we can do anything useful with it, we need to convert it back to binary and then decompress it — potentially many times.

#### Setting Up a Working Directory

Since we'll be extracting multiple files, it's best to work in a dedicated temporary directory to keep things clean:

```bash
cd /tmp
mktemp -d
```

Output:

```bash
/tmp/tmp.gFUIsIOWRy
```

`mktemp -d` creates a uniquely named temporary directory, avoiding conflicts with other users on the same system. Copy `data.txt` into it and navigate there:

```bash
cp ~/data.txt /tmp/tmp.gFUIsIOWRy
cd /tmp/tmp.gFUIsIOWRy
```

---

## Decompression — Layer by Layer

The general strategy for this level is: **reverse the hexdump → identify the file type → rename with the correct extension → decompress → repeat.**

---

#### Step 1 — Reverse the Hexdump

Convert the hexdump back to its original binary form using `xxd -r`:

```bash
xxd -r data.txt > data.bin
```

Command explanation:

- `xxd -r` — reverses a hexdump back into binary data (`-r` stands for reverse)
- `data.txt` — the input hexdump file
- `> data.bin` — redirects the binary output into a new file

Verify the resulting file type:

```bash
file data.bin
```

Output:

```bash
data.bin: gzip compressed data
```

---

#### Layer 1 — gzip

```bash
mv data.bin data.gz
gunzip data.gz
file data
```

Output:

```bash
data: bzip2 compressed data
```

---

#### Layer 2 — bzip2

```bash
mv data data.bz2
bunzip2 data.bz2
file data
```

Output:

```bash
data: gzip compressed data
```

---

#### Layer 3 — gzip

```bash
mv data data.gz
gunzip data.gz
file data
```

Output:

```bash
data: POSIX tar archive (GNU)
```

---

#### Layer 4 — tar archive

```bash
mv data data.tar
tar -xf data.tar
file data5.bin
```

Output:

```bash
data5.bin: POSIX tar archive (GNU)
```

---

#### Layer 5 — tar archive

```bash
mv data5.bin data5.tar
tar -xf data5.tar
file data6.bin
```

Output:

```bash
data6.bin: bzip2 compressed data
```

---

#### Layer 6 — bzip2

```bash
mv data6.bin data.bz2
bunzip2 data.bz2
file data
```

Output:

```bash
data: POSIX tar archive (GNU)
```

---

#### Layer 7 — tar archive

```bash
mv data data.tar
tar -xf data.tar
file data8.bin
```

Output:

```bash
data8.bin: gzip compressed data
```

---

#### Final Layer — gzip

```bash
mv data8.bin data8.gz
gunzip data8.gz
cat data8
```

Output:

```bash
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

The password for the next level is revealed:

```bash
FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit13`:

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

---

## Terminal Output

```bash
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ cd /tmp
bandit12@bandit:/tmp$ mktemp -d
/tmp/tmp.gFUIsIOWRy
bandit12@bandit:/tmp$ cp ~/data.txt /tmp/tmp.gFUIsIOWRy
bandit12@bandit:/tmp$ cd /tmp/tmp.gFUIsIOWRy
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ xxd -r data.txt > data.bin
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data.bin
data.bin: gzip compressed data
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data.bin data.gz && gunzip data.gz
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data
data: bzip2 compressed data
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data data.bz2 && bunzip2 data.bz2
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data
data: gzip compressed data
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data data.gz && gunzip data.gz
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data data.tar && tar -xf data.tar
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data5.bin data5.tar && tar -xf data5.tar
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data6.bin
data6.bin: bzip2 compressed data
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data6.bin data.bz2 && bunzip2 data.bz2
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data data.tar && tar -xf data.tar
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ file data8.bin
data8.bin: gzip compressed data
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ mv data8.bin data8.gz && gunzip data8.gz
bandit12@bandit:/tmp/tmp.gFUIsIOWRy$ cat data8
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

---

## Key Takeaways

- `xxd -r` reverses a hexdump back into its original binary form
- `file` identifies the true type of a file by inspecting its contents — always use it after each extraction step
- Files must be renamed with the correct extension before most decompression tools will process them
- `gunzip` decompresses `.gz` (gzip) files; `bunzip2` decompresses `.bz2` (bzip2) files; `tar -xf` extracts tar archives
- `mktemp -d` creates a safe, uniquely named temporary directory — essential when working with many intermediate files
- Repeated compression is a common CTF technique; the key is patience and methodically checking the file type at every step
- Working in `/tmp` keeps your home directory clean and avoids permission issues when creating new files

---

## References

- [Bandit Level 12 → 13](https://overthewire.org/wargames/bandit/bandit13.html)
- [xxd Manual Page](https://linux.die.net/man/1/xxd)
- [file Manual Page](https://linux.die.net/man/1/file)
- [tar Manual Page](https://linux.die.net/man/1/tar)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series