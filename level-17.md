# 🚩 Bandit - Level 16 → 17

## Objective

Retrieve the credentials for the next level by submitting the current level's password to a port on `localhost` in the range **31000–32000**. First, identify which ports have a server listening on them, then determine which of those speak SSL/TLS. Only one server will return the next credentials — the others will simply echo back whatever you send.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit16`|
|Password|`kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx` (from Level 15 → 16)|

---

## Solution

#### Finding the Local IP Address

After logging in as `bandit16`, I used `ifconfig` to get the machine's IP address:

```bash
ifconfig
```

This showed the local IP as `10.0.1.162`, which I used for subsequent connections.

#### Scanning the Port Range

The challenge says the target is somewhere between ports 31000 and 32000. Instead of testing each one manually, I used `nmap` to scan the entire range:

```bash
nmap -p 31000-32000 10.0.1.162
```

Command explanation:

- `nmap` — Network Mapper, a tool for scanning open ports and discovering services
- `-p 31000-32000` — specifies the port range to scan
- `10.0.1.162` — the target host

Output:

```
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

Five ports are open. Now I needed to identify which ones speak SSL/TLS and which don't.

#### Identifying SSL/TLS Services

Using nmap's service version detection flag `-sV` gives more detail about what each port is running:

```bash
nmap -sV -p 31000-32000 10.0.1.162
```

Command explanation:

- `-sV` — enables service/version detection; nmap probes each open port to identify the running service

Output:

```
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

Key observations from the scan:

- `31046`, `31691`, `31960` — plain `echo` services (no SSL, will just reflect input back)
- `31518` — `ssl/echo` (SSL-enabled, but still echoes back — not the target)
- `31790` — `ssl/unknown` (SSL-enabled with an unrecognized service — this is the one to try)

#### Testing Port 31518 — SSL Echo

I first tried port `31518` to confirm it was just an echo server:

```bash
openssl s_client -connect 10.0.1.162:31518
```

After the handshake, the connection just reflected input back — confirmed as an echo server, not the target.

#### Connecting to Port 31790 — The Target

```bash
openssl s_client -connect 10.0.1.162:31790 -quiet
```

The `-quiet` flag suppresses the verbose certificate and session details, showing only the essential exchange.

After the SSL handshake, I submitted the current level's password:

```
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

The server responded with:

```
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
...
-----END RSA PRIVATE KEY-----
```

Instead of a password, the server returned an **RSA private key** — this is the credential for the next level.

#### Saving the Private Key and Logging In

I copied the private key from the terminal output and saved it locally as `bandit16_key.txt`. Then from PowerShell on my local machine:

```powershell
ssh -i bandit16_key.txt bandit17@bandit.labs.overthewire.org -p 2220
```

This successfully authenticated me as **bandit17**.

---

## Terminal Output

```bash
bandit16@bandit:~$ nmap -sV -p 31000-32000 10.0.1.162
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo

bandit16@bandit:~$ openssl s_client -connect 10.0.1.162:31790 -quiet
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
...
-----END RSA PRIVATE KEY-----
```

---

## Key Takeaways

- `nmap -p` scans a range of ports to identify which are open and listening
- `nmap -sV` performs service version detection, revealing protocol details like SSL/TLS
- An `echo` service simply reflects input back — useful for testing, not for retrieving credentials
- `ssl/unknown` in nmap output indicates an SSL-wrapped service with unrecognized behavior — often the interesting target
- The `-quiet` flag in `openssl s_client` suppresses certificate and session noise, showing only the data exchange
- Some levels return an SSH private key instead of a plaintext password — treat it the same way with `ssh -i`
- `nmap` is an essential recon tool in CTFs for discovering which services are running on a system

---

## References

- [Bandit Level 16 → 17](https://overthewire.org/wargames/bandit/bandit17.html)
- [nmap Manual Page](https://linux.die.net/man/1/nmap)
- [OpenSSL Manual Page](https://linux.die.net/man/1/openssl)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series

