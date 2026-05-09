# 🚩 Bandit - Level 14 → 15

## Objective

Retrieve the password for the next level by submitting the current level's password to port `30000` on `localhost`. This level introduces network communication and how to interact with services running on specific ports using command-line tools.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit14`|
|Password|`MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS` (from Level 13 → 14)|

---

## Solution

#### Understanding the Challenge

The goal is straightforward: send the current level's password to a service listening on port `30000` on `localhost`. To do this, we need a tool that lets us establish a direct TCP connection to a specific host and port, type input, and receive a response.

#### Finding the IP Address

Before using `telnet`, I needed the IP address of the local machine. I used `ifconfig` to retrieve it:

```bash
ifconfig
```

This lists all network interfaces and their associated IP addresses. The relevant local IP address here was:

```
10.0.1.162
```

#### Connecting with Telnet

`telnet` is a classic command-line tool that establishes a bidirectional, interactive, text-based communication session with a remote host over TCP. While it's considered insecure for general use (data is sent in plaintext), it's perfectly suited for testing and interacting with local services.

```bash
telnet 10.0.1.162 30000
```

Command explanation:

- `telnet` — initiates a TCP connection to a remote host
- `10.0.1.162` — the IP address of the host to connect to (the local machine in this case)
- `30000` — the port number of the service we want to interact with

Alternatively, since the service is running on the same machine, you can use `localhost` directly instead of looking up the IP:

```bash
telnet localhost 30000
```

Other tools that would work for this task:

```bash
nc localhost 30000                          # netcat - more common in CTFs
echo "MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS" | nc localhost 30000  # one-liner with netcat
```

#### Submitting the Password

Once the connection was established, I typed the current level's password and pressed Enter:

```
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

The service validated the input and responded with:

```
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

The password for the next level is revealed:

```
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit15`:

```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

---

## Terminal Output

```bash
bandit14@bandit:~$ telnet 10.0.1.162 30000
Trying 10.0.1.162...
Connected to 10.0.1.162.
Escape character is '^]'.
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

Connection closed by foreign host.
bandit14@bandit:~$
```

---

## Key Takeaways

- `telnet` establishes a raw TCP connection to a host and port, allowing interactive text-based communication with a service
- `localhost` (or `127.0.0.1`) always refers to the current machine — no need to look up the IP for local services
- `ifconfig` displays network interface information including IP addresses
- Port numbers identify specific services running on a host — multiple services can run on the same machine on different ports
- `netcat` (`nc`) is the modern, more versatile alternative to `telnet` for interacting with network services in CTFs
- Telnet sends all data in plaintext, making it unsuitable for production use — but useful for testing and debugging
- Many CTF challenges involve submitting credentials or flags to a listening service on a specific port

---

## References

- [Bandit Level 14 → 15](https://overthewire.org/wargames/bandit/bandit15.html)
- [telnet Manual Page](https://linux.die.net/man/1/telnet)
- [netcat Manual Page](https://linux.die.net/man/1/nc)
- [ifconfig Manual Page](https://linux.die.net/man/8/ifconfig)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series