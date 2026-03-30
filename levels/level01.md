# 🚩 Level 0 → Level 1

## Level Goal

The password for the next level is stored in a file called `readme`, located in the home directory. Use this password to log into `bandit1` via SSH.

## Commands Used

- `ls`
- `cat`
- `ssh`

## Walkthrough

Once logged in as `bandit0`, list the contents of the home directory:
```bash
ls
```

Read the contents of the `readme` file:
```bash
cat readme
```

This will output the password for the next level:
```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

Exit the current session:
```bash
exit
```

Alternatively, you can press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **`ls`:** Lists the contents of the current directory, helping you identify what files are present.
- **`cat`:** Reads and outputs the contents of a file to the terminal.
- **Home directory:** The default directory a user lands in upon login, typically `/home/<username>` on Linux systems.

[⬅ Back to Home](../README.md)