# 🚩 Level 10 → Level 11

## Level Goal

The password for the next level is stored in `data.txt`, which contains Base64 encoded data that must be decoded to retrieve it.

## Commands Used

- `ls`
- `cat`
- `base64`
- `ssh`

## Walkthrough

Once logged in as `bandit10`, list the contents of the home directory:
```bash
ls
```

You will find a file named `data.txt`. Reading it with `cat` reveals encoded text:
```bash
cat data.txt
```

Output:
```
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```

This is Base64 encoded data. The trailing `==` is a characteristic padding signature of Base64 encoding. Use the `base64` command with the `-d` flag to decode it:
```bash
base64 -d data.txt
```

This will output the password for the next level:
```
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Base64 encoding:** A binary-to-text encoding scheme that represents binary data using a set of 64 printable ASCII characters. It is widely used for encoding data in contexts such as email attachments, JWTs, and HTTP Basic Authentication. Base64 is an encoding scheme, not an encryption method — it provides no security and can be trivially decoded.
- **`base64 -d`:** Decodes a Base64 encoded file or input stream back into its original form. The `-d` flag stands for decode.
- **Identifying Base64:** Encoded strings typically consist of alphanumeric characters along with `+` and `/`, and are often padded with one or two `=` characters at the end to align the output to a multiple of 4 characters.

[⬅ Back to Home](../README.md)