# 🚩 Level 12 → Level 13

## Level Goal

The password for the next level is stored in `data.txt`, which is a hexdump of a file that has been repeatedly compressed using multiple compression formats. The file must be reconstructed and decompressed iteratively to retrieve the password.

## Commands Used

- `mktemp`
- `cp`
- `xxd`
- `file`
- `mv`
- `gzip` / `gunzip`
- `bzip2` / `bunzip2`
- `tar`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit12`, the home directory is read-only, so a temporary working directory must be created first. Use `mktemp -d` to generate a uniquely named directory under `/tmp`:
```bash
mktemp -d
```

Output:
```
/tmp/tmp.XI61eOnqfK
```

Navigate into it and copy the data file:
```bash
cd /tmp/tmp.XI61eOnqfK
cp ~/data.txt .
```

---

### Step 1 — Reverse the hexdump

`data.txt` is a hexdump. Use `xxd -r` to convert it back into binary:
```bash
xxd -r data.txt > file
```

Check the file type:
```bash
file file
```

Output: `gzip compressed data`

---

### Step 2 — Decompress gzip

Rename the file with the correct extension and decompress:
```bash
mv file file.gz
gunzip file.gz
```

Check the file type again:
```bash
file file
```

Output: `bzip2 compressed data`

---

### Step 3 — Decompress bzip2
```bash
bunzip2 file
```

Output file: `file.out`. Check its type:
```bash
file file.out
```

Output: `gzip compressed data`

---

### Step 4 — Decompress gzip again

`gunzip` requires the `.gz` extension to recognise the file. Rename it first:
```bash
mv file.out file.gz
gunzip file.gz
```

Check the file type:
```bash
file file
```

Output: `POSIX tar archive (GNU)`

---

### Step 5 — Extract tar archive

Rename and extract:
```bash
mv file file.tar
tar -xf file.tar
```

This produces `data5.bin`. Check its type:
```bash
file data5.bin
```

Output: `POSIX tar archive (GNU)`

---

### Step 6 — Extract another tar archive
```bash
mv data5.bin file.tar
tar -xf file.tar
```

This produces `data6.bin`. Check its type:
```bash
file data6.bin
```

Output: `bzip2 compressed data`

---

### Step 7 — Extract nested tar archive
```bash
mv data6.bin file2.tar
tar -xf file2.tar
```

This produces `data8.bin`. Check its type:
```bash
file data8.bin
```

Output: `gzip compressed data`

---

### Step 8 — Final gzip decompression
```bash
mv data8.bin file.gz
gunzip file.gz
```

Check the file type one last time:
```bash
file file
```

Output: `ASCII text`

---

### Step 9 — Read the password
```bash
cat file
```

Output:
```
FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

---

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Hexdump (`xxd`):** A utility that displays file contents in hexadecimal format. The `-r` flag reverses this process, converting a hexdump back into its original binary form.
- **`mktemp -d`:** Creates a temporary directory with a randomly generated, hard-to-guess name under `/tmp`. Essential when the home directory is read-only.
- **`file` command:** Identifies the true type of a file based on its content rather than its extension. Critical in this level since files are renamed and repacked multiple times.
- **`gzip` / `gunzip`:** A widely used compression format. Files must carry the `.gz` extension for `gunzip` to process them — renaming with `mv` is required when the extension is missing.
- **`bzip2` / `bunzip2`:** An alternative compression format that generally achieves better compression ratios than gzip. Operates similarly but uses a different algorithm.
- **`tar`:** A tool for archiving multiple files into a single file. The `-xf` flags extract the contents of an archive. `tar` does not compress by itself — it only bundles files.
- **Iterative decompression:** This level simulates a real-world scenario where files may be compressed multiple times or with multiple formats. The correct approach is to check the file type at each step and apply the appropriate tool rather than assuming a fixed sequence.

[⬅ Back to Home](../README.md)