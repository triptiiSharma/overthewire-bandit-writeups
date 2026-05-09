# 🚩 Bandit - Level 13 → 14

## Objective

Find the password for the next level stored in `/etc/bandit_pass/bandit14`, which can only be read by user **bandit14**. Instead of a password, this level provides an SSH private key that must be used to authenticate as bandit14. This level introduces SSH key-based authentication and how it differs from password-based login.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit13`|
|Password|`FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn` (from Level 12 → 13)|

---

## Solution

#### Exploring the Home Directory

After logging in as `bandit13`, I checked the files in the home directory:

```bash
ls
```

Output:

```bash
bandit13@bandit:~$ ls
HINT  sshkey.private
```

Two files are present — `HINT` and `sshkey.private`. The `sshkey.private` file contains the SSH private key needed to authenticate as **bandit14**.

To inspect its contents:

```bash
cat sshkey.private
```

Output:

```bash
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

This is a standard RSA private key in PEM format. SSH supports two methods of authentication: password-based and key-based. Here, we're using key-based authentication instead of a password.

#### Understanding SSH Key-Based Authentication

In SSH key authentication, a **key pair** is used:

- **Private key** — kept secret on your machine, never shared
- **Public key** — placed on the remote server in `~/.ssh/authorized_keys`

When you connect, SSH proves you own the private key without ever transmitting it. The server verifies the match cryptographically. This is more secure than passwords and cannot be brute-forced.

#### Attempting Direct Access — Permission Denied

Before using the key, I first tried reading the password file directly as `bandit13`:

```bash
cat /etc/bandit_pass/bandit14
```

Output:

```bash
cat: /etc/bandit_pass/bandit14: Permission denied
```

As expected — the file is protected by Linux file permissions and only readable by `bandit14`.

#### Attempting SSH from Inside the Server — Blocked

I then tried to use the private key to SSH into `bandit14` from within the Bandit server itself:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

Output:

```bash
!!! Connecting from localhost is blocked to conserve resources.
```

The server blocks SSH connections originating from `localhost` to prevent resource abuse. This means we need to connect from our **local machine** instead.

#### Copying the Key and Connecting from a Local Machine

I copied the contents of `sshkey.private` from the terminal and saved them locally as `bandit14_key.txt`. Then from PowerShell on my local machine, I used the key to authenticate:

```powershell
ssh -i bandit14_key.txt bandit14@bandit.labs.overthewire.org -p 2220
```

Command explanation:

- `ssh` — initiates the SSH connection
- `-i bandit14_key.txt` — specifies the private key file to use for authentication (`-i` stands for identity file)
- `bandit14@bandit.labs.overthewire.org` — the username and host to connect to
- `-p 2220` — connects on port 2220 instead of the default port 22

This successfully authenticated me as **bandit14**.

#### Retrieving the Password

Once logged in as `bandit14`, I accessed the password file:

```bash
cat /etc/bandit_pass/bandit14
```

Output:

```bash
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

The password for the next level is revealed:

```bash
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit14`:

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

---

## Terminal Output

```bash
bandit13@bandit:~$ ls
HINT  sshkey.private
bandit13@bandit:~$ cat /etc/bandit_pass/bandit14
cat: /etc/bandit_pass/bandit14: Permission denied
bandit13@bandit:~$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
!!! Connecting from localhost is blocked to conserve resources.
PS C:\Users\pandi> ssh -i bandit14_key.txt bandit14@bandit.labs.overthewire.org -p 2220
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

---

## Key Takeaways

- SSH supports key-based authentication as a more secure alternative to passwords
- The `-i` flag in `ssh` specifies the private key (identity file) to use for authentication
- SSH private keys are in PEM format, identifiable by the `-----BEGIN RSA PRIVATE KEY-----` header
- Linux file permissions enforce access control — files can be restricted to specific users regardless of how you're authenticated
- Some servers block SSH connections from `localhost` to prevent resource abuse — in such cases, connect from your local machine
- Private keys must be kept secret; anyone with a copy of the private key can authenticate as that user
- Key-based authentication is widely used in production environments, CI/CD pipelines, and CTF challenges

---

## References

- [Bandit Level 13 → 14](https://overthewire.org/wargames/bandit/bandit14.html)
- [SSH Manual Page](https://linux.die.net/man/1/ssh)
- [SSH Key-Based Authentication](https://www.ssh.com/academy/ssh/public-key-authentication)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series