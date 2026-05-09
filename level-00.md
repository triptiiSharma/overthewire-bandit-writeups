# 🚩 Bandit - Level 0

## Objective

Log into the Bandit game server for the first time using SSH. This introductory level teaches the basics of establishing a secure remote connection to a Linux server.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit0`|
|Password|`bandit0`|

## Solution

#### Establishing the SSH Connection

Use SSH to connect to the Bandit server:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

**Command breakdown:**

- `ssh` - Secure Shell protocol for encrypted remote login
- `bandit0@bandit.labs.overthewire.org` - specifies the remote username (`bandit0`) and hostname
- `-p 2220` - tells SSH to connect on port `2220` instead of the default port `22`

#### Host Fingerprint Verification

On the first connection, SSH prompts you to verify the server's fingerprint:

```bash
The authenticity of host '[bandit.labs.overthewire.org]:2220' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type `yes` to continue:

```bash
yes
```

This stores the host fingerprint locally in `~/.ssh/known_hosts`, which helps protect against **[Man-in-the-Middle (MITM)](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)** attacks by ensuring future connections are to the same server.

#### Authentication

Enter the password when prompted:

```bash
bandit0
```

**Note:** Password input remains hidden in the terminal for security - you won't see any characters as you type.

#### Successful Login

If authentication succeeds, the shell prompt changes to:

```bash
bandit0@bandit:~$
```

This confirms you're now logged into the Bandit server as user `bandit0`.

## Terminal Output

```bash
PS C:\Users\pandi> ssh bandit0@bandit.labs.overthewire.org -p 2220
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([13.63.65.121]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[bandit.labs.overthewire.org]:2220' (ED25519) to the list of known hosts.
bandit0@bandit.labs.overthewire.org's password:
Welcome to OverTheWire!
bandit0@bandit:~$
```

## Key Takeaways

- SSH (Secure Shell) provides encrypted remote access to servers
- The `-p` flag specifies a custom SSH port when the server doesn't use the default port 22
- SSH stores trusted host fingerprints after first verification to detect potential MITM attacks
- Password input in terminals is hidden for security purposes
- The shell prompt format `user@hostname:~$` indicates successful login and current working directory

## References

- [Bandit Level 0](https://overthewire.org/wargames/bandit/bandit0.html)
- [SSH Protocol](https://en.wikipedia.org/wiki/Secure_Shell)
- [ssh Manual Page](https://linux.die.net/man/1/ssh)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series