# 🚩 Bandit - Level 15 → 16

## Objective

Retrieve the password for the next level by submitting the current level's password to port `30001` on `localhost` — but this time using **SSL/TLS encryption**. This level introduces secure encrypted network communication using OpenSSL.

|Field|Value|
|---|:--|
|Host|`bandit.labs.overthewire.org`|
|Port|`2220`|
|Username|`bandit15`|
|Password|`8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo` (from Level 14 → 15)|

---

## Solution

#### Understanding the Challenge

The previous level used `telnet` to send a password over a plain TCP connection. This level steps it up — the service on port `30001` requires an **SSL/TLS encrypted connection**. Plain `telnet` or `netcat` won't work here because they don't support encryption. We need a tool that can perform an SSL/TLS handshake before communicating.

#### Understanding SSL/TLS and OpenSSL

**SSL/TLS** (Secure Sockets Layer / Transport Layer Security) is the protocol that encrypts data in transit between a client and a server — the same technology that powers HTTPS. Before any data is exchanged, both sides perform a **handshake** to negotiate encryption settings and verify the server's identity using a certificate.

**OpenSSL** is an open-source toolkit that implements SSL/TLS and a wide range of cryptographic functions. It can be used to generate certificates, test connections, encrypt files, and more.

#### Connecting with OpenSSL s_client

To connect to the SSL/TLS service on port `30001`, use `openssl s_client`:

```bash
openssl s_client -connect 10.0.1.162:30001
```

Command explanation:

- `openssl` — the OpenSSL toolkit
- `s_client` — sub-command that implements an SSL/TLS client for testing and debugging encrypted connections
- `-connect 10.0.1.162:30001` — specifies the host and port to connect to in `host:port` format

You can also use `localhost` directly:

```bash
openssl s_client -connect localhost:30001
```

#### Reading the SSL Handshake Output

After connecting, OpenSSL prints detailed information about the SSL session before accepting input. Key parts of the output include:

- **Certificate chain** — shows the server's certificate details (issued to `CN = SnakeOil` here)
- **Verification error: self-signed certificate** — the server's certificate is self-signed (not issued by a trusted CA). This is common in CTF environments and doesn't prevent the connection from working
- **TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384** — confirms the protocol version and cipher suite being used
- **read R BLOCK** — indicates the SSL handshake is complete and the client is ready to receive input

Once you see `read R BLOCK`, the connection is live and waiting for your input.

#### Submitting the Password

After the handshake completes, type the current level's password and press Enter:

```
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

The service validates the input and responds:

```
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

The password for the next level is revealed:

```
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

#### Logging into the Next Level

Use the retrieved password to log in as `bandit16`:

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

---

## Terminal Output

```bash
bandit15@bandit:~$ openssl s_client -connect 10.0.1.162:30001
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
---
Certificate chain
 0 s:CN = SnakeOil
   i:CN = SnakeOil
...
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 4096 bit
Verify return code: 18 (self-signed certificate)
---
read R BLOCK
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

closed
bandit15@bandit:~$
```

---

## Key Takeaways

- SSL/TLS encrypts data in transit, preventing anyone intercepting the connection from reading the contents
- `openssl s_client` is the standard tool for establishing and testing SSL/TLS connections from the command line
- The `-connect host:port` flag specifies the destination in `host:port` format
- A **self-signed certificate** means the server issued its own certificate rather than using a trusted Certificate Authority (CA) — common in CTFs and internal tools
- `read R BLOCK` in the OpenSSL output signals the handshake is complete and the connection is ready for input
- Unlike `telnet`, `openssl s_client` encrypts all communication — the password is never sent in plaintext
- If you see `DONE`, `RENEGOTIATING`, or `KEYUPDATE` responses after submitting input, refer to the `CONNECTED COMMANDS` section in the OpenSSL manpage — pressing `R` can resume reading

---

## References

- [Bandit Level 15 → 16](https://overthewire.org/wargames/bandit/bandit16.html)
- [OpenSSL Manual Page](https://linux.die.net/man/1/openssl)
- [SSL/TLS Explained](https://en.wikipedia.org/wiki/Transport_Layer_Security)

---

Writeup by [Tripti Sharma](https://github.com/triptiiSharma) | Part of Overthewire - Bandit Series