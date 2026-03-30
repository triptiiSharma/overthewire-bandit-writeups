# 🚩 Level 6 → Level 7

## Level Goal

The password for the next level is stored somewhere on the server. The target file has all of the following properties:

- Owned by user `bandit7`
- Owned by group `bandit6`
- Exactly 33 bytes in size

## Commands Used

- `find`
- `cd`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit6`, running `ls` in the home directory returns nothing, as the file is not stored in the home directory but somewhere on the entire server. The search must begin from the root of the filesystem.

Use `find` starting from `/` with filters matching all three properties:
```bash
find / -user bandit7 -group bandit6 -size 33c
```

This will produce a large number of `Permission denied` errors for directories the current user cannot access, but the target file will appear in the output:
```
/var/lib/dpkg/info/bandit7.password
```

To suppress the permission denied errors and get a cleaner output, redirect stderr to `/dev/null`:
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Navigate to the located directory:
```bash
cd /var/lib/dpkg/info/
```

Read the file:
```bash
cat bandit7.password
```

This will output the password for the next level:
```
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **Searching from root (`/`):** Starting `find` from `/` instructs it to search the entire filesystem, not just the current directory or home directory.
- **`-user` and `-group` flags:** Allow filtering files by their owning user and group respectively, useful when file ownership metadata is the only known property.
- **`2>/dev/null`:** Redirects standard error (file descriptor `2`) to `/dev/null`, effectively suppressing error messages such as `Permission denied` and producing a cleaner output.
- **`/var/lib/dpkg/info/`:** A system directory used by the Debian package manager (`dpkg`) to store metadata and configuration files for installed packages.

[⬅ Back to Home](../README.md)