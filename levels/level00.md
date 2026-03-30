# 🚩 Level 0

## Level Goal

Log into the game server using SSH. The objective is simply to establish a successful connection to the remote host using the provided credentials.

## Connection Details

| Field    | Value                            |
|----------|----------------------------------|
| Host     | bandit.labs.overthewire.org      |
| Port     | 2220                             |
| Username | bandit0                          |
| Password | bandit0                          |

## Command Used

- `ssh`

## Walkthrough

Connect to the remote server using the SSH command with the specified port flag:
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password:
```
bandit0
```

A successful login will drop you into a remote shell session as `bandit0`.

## Concepts Covered

- **SSH (Secure Shell):** A cryptographic network protocol used to securely connect to remote machines over an unsecured network.
- **Port specification:** By default, SSH operates on port 22. The `-p` flag is used to specify a non-default port, in this case `2220`.

[⬅ Back to Home](../README.md)