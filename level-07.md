# 🚩 Bandit - Level 6 → 7

## Objective

Find the password for the next level stored somewhere on the server with the following properties:

- Owned by user `bandit7`
- Owned by group `bandit6`
- Exactly 33 bytes in size

This level expands the scope of your search from a specific directory to the entire filesystem, teaching you system-wide file searches and error handling.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit6`|
|Password|`HWasnPhtq9AVKe0dmk45nxy20cvUa6EG` _(from Level 5 → 6)_|

## Solution

#### Checking the Home Directory

After logging in as `bandit6`, I first checked the home directory to see if there was anything useful:

```bash
ls -a
```

Output:

```bash
.  ..  .bash_logout  .bashrc  .profile
```

Nothing useful here - just the standard configuration files that exist in every user's home directory.

#### Understanding the Challenge

The level description states the file is stored **"somewhere on the server"** - not just in the home directory or a specific location. This means we need to search the entire filesystem starting from the root directory (`/`).

We have three specific criteria to narrow down our search:

- Owner: `bandit7`
- Group: `bandit6`
- Size: `33 bytes`

#### Searching the Entire Filesystem

Use the `find` command to search from the root directory with all three filters:

```bash
find / -user bandit7 -group bandit6 -size 33c
```

**Command explanation:**

- `find` - Search for files in a directory hierarchy
- `/` - Start searching from the root directory (the entire filesystem)
- `-user bandit7` - Match files owned by the user `bandit7`
- `-group bandit6` - Match files owned by the group `bandit6`
- `-size 33c` - Match files that are exactly 33 bytes in size (`c` = bytes)

#### Handling Permission Errors

This command produces a lot of "Permission denied" errors:

```bash
find: '/tmp': Permission denied
find: '/etc/ssl/private': Permission denied
find: '/root': Permission denied
...
/var/lib/dpkg/info/bandit7.password
```

**Why the errors occur:** When searching the entire filesystem, `find` tries to access directories that require root or special permissions. As a regular user (`bandit6`), we don't have access to many system directories, so `find` outputs error messages for each restricted location.

**Important:** Even with errors, `find` continues searching and returns results from accessible directories.

#### Cleaning Up the Output (Optional)

To suppress error messages and only see the matching files, redirect error output to `/dev/null`:

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

**Command explanation:**

- `2>/dev/null` - Redirects standard error (stderr, file descriptor 2) to `/dev/null` (a special file that discards all data)
- This keeps only the actual results visible, making output cleaner

Output:

```bash
/var/lib/dpkg/info/bandit7.password
```

The matching file is located at `/var/lib/dpkg/info/bandit7.password`.

#### Reading the Password File

Now that we have the file path, read its contents:

```bash
cat /var/lib/dpkg/info/bandit7.password
```

Output:

```bash
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

This reveals the password for the next level:

```bash
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit7`:

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

## Terminal Output

```bash
bandit6@bandit:~$ ls -a
.  ..  .bash_logout  .bashrc  .profile
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c
find: '/tmp': Permission denied
find: '/etc/ssl/private': Permission denied
find: '/root': Permission denied
...
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

## Key Takeaways

- `find` can search the entire filesystem by starting from the root directory (`/`)
- `-user` filters files by their owner (user)
- `-group` filters files by their group ownership
- `-size 33c` searches for files that are exactly 33 bytes (the `c` suffix specifies bytes)
- Permission denied errors are normal when searching system-wide as a non-privileged user
- `2>/dev/null` redirects error messages to suppress them, keeping output clean
- Even with permission errors, `find` continues searching and returns results from accessible locations
- File ownership (user and group) can be powerful filters when combined with other criteria
- `/dev/null` is a special file that discards all data written to it - useful for suppressing unwanted output

## References

- [Bandit Level 6 → 7](https://overthewire.org/wargames/bandit/bandit7.html)
- [find Manual Page](https://linux.die.net/man/1/find)
- [Linux File Permissions](https://www.guru99.com/file-permissions.html)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series