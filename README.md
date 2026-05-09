# 🚩 OverTheWire: Bandit — Writeups & Walkthrough

> A structured, level-by-level documentation of the [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/) wargame — built as a personal reference and a practical demonstration of foundational Linux and command-line security concepts explored through hands-on challenges.


## About OverTheWire: Bandit

[OverTheWire](https://overthewire.org/wargames/) is a platform that offers wargames designed to help individuals learn and practice security concepts in a legal, controlled environment. **Bandit** is the entry-level wargame and the recommended starting point for beginners.

Each level introduces progressively complex challenges centered around Linux file navigation, permissions, encoding, networking, and scripting — all of which form the foundation of real-world cybersecurity work.


## Repository Structure

```
OverTheWire-Bandit/
├── README.md
└── 
    ├── level00.md
    ├── level01.md
    ├── level02.md
    ├── level03.md
    └── ...
```

Each file in the `` directory corresponds to a single level transition and follows a consistent structure:

- **Objective** — the goal as stated by the challenge
- **Commands Used** — all commands involved in solving the level
- **Walkthrough** — step-by-step breakdown of the solution with command explanations
- **Key Takeaways** — Linux and security concepts introduced or reinforced


## How to Use This Repository

1. Visit [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/) and start at Level 0
2. Attempt each level on your own before referring to any writeup
3. Navigate to the corresponding file in the `` directory if you need guidance
4. Read the Key Takeaways section to reinforce understanding beyond just the solution

> Note: This repository is intended as a **learning aid, not a shortcut**. Working through challenges yourself before consulting the writeups will yield the most value.


## Concepts Covered

**File System & Navigation**
`ls`, `cat`, `cd`, `file`, `find`, `strings`, hidden files, special filenames, path handling

**Text Processing**
`grep`, `sort`, `uniq`, `tr`, `diff`, pipes (`|`), redirection (`>`, `<<<`, `2>/dev/null`)

**Encoding & Compression**
Base64, ROT13, gzip, bzip2, tar archives, hexdump reversal (`xxd -r`)

**Networking & Cryptography**
SSH, SSH key-based authentication, `telnet`, `netcat`, `openssl s_client`, `nmap`, SSL/TLS

**Linux Fundamentals**
File permissions, user/group ownership, `/etc`, `/tmp`, `mktemp`, process management


## Progress Tracker

| # | Level | Core Concept | Status |
|---|-------|-------------|--------|
| 01 | [Level 0](level00.md) | SSH Login | 🏁 Completed |
| 02 | [Level 0 → 1](level01.md) | Reading Files (`cat`, `ls`) | 🏁 Completed |
| 03 | [Level 1 → 2](level02.md) | Dashed Filename (`./`) | 🏁 Completed |
| 04 | [Level 2 → 3](level03.md) | Spaces in Filenames | 🏁 Completed |
| 05 | [Level 3 → 4](level04.md) | Hidden Files (`ls -a`) | 🏁 Completed |
| 06 | [Level 4 → 5](level05.md) | File Type Detection (`file`) | 🏁 Completed |
| 07 | [Level 5 → 6](level06.md) | Finding Files by Properties | 🏁 Completed |
| 08 | [Level 6 → 7](level07.md) | System-Wide File Search | 🏁 Completed |
| 09 | [Level 7 → 8](level08.md) | Text Search (`grep`) | 🏁 Completed |
| 10 | [Level 8 → 9](level09.md) | Unique Lines (`sort` + `uniq`) | 🏁 Completed |
| 11 | [Level 9 → 10](level10.md) | Strings in Binary Files | 🏁 Completed |
| 12 | [Level 10 → 11](level11.md) | Base64 Decoding | 🏁 Completed |
| 13 | [Level 11 → 12](level12.md) | ROT13 Cipher (`tr`) | 🏁 Completed |
| 14 | [Level 12 → 13](level13.md) | Hexdump + Nested Compression | 🏁 Completed |
| 15 | [Level 13 → 14](level14.md) | SSH Key Authentication | 🏁 Completed |
| 16 | [Level 14 → 15](level15.md) | Network Services (`telnet`) | 🏁 Completed |
| 17 | [Level 15 → 16](level16.md) | SSL/TLS Connections (`openssl`) | 🏁 Completed |
| 18 | [Level 16 → 17](level17.md) | Port Scanning (`nmap`) | 🏁 Completed |
| 19 | [Level 17 → 18](level18.md) | File Comparison (`diff`) | 🏁 Completed |
| 20 | [Level 18 → 19](level19.md) | — | 👀 In Progress |
| 21 | [Level 19 → 20](level20.md) | — | ⏳ Pending |
| 22 | [Level 20 → 21](level21.md) | — | ⏳ Pending |
| 23 | [Level 21 → 22](level22.md) | — | ⏳ Pending |
| 24 | [Level 22 → 23](level23.md) | — | ⏳ Pending |
| 25 | [Level 23 → 24](level24.md) | — | ⏳ Pending |
| 26 | [Level 24 → 25](level25.md) | — | ⏳ Pending |
| 27 | [Level 25 → 26](level26.md) | — | ⏳ Pending |
| 28 | [Level 26 → 27](level27.md) | — | ⏳ Pending |
| 29 | [Level 27 → 28](level28.md) | — | ⏳ Pending |
| 30 | [Level 28 → 29](level29.md) | — | ⏳ Pending |
| 31 | [Level 29 → 30](level30.md) | — | ⏳ Pending |
| 32 | [Level 30 → 31](level31.md) | — | ⏳ Pending |
| 33 | [Level 31 → 32](level32.md) | — | ⏳ Pending |
| 34 | [Level 32 → 33](level33.md) | — | ⏳ Pending |
| 35 | [Level 33 → 34](level34.md) | — | ⏳ Pending |


## Author

**Tripti Sharma**
GitHub: [@triptiiSharma](https://github.com/triptiiSharma)

---

*Writeups are documented for educational purposes. All challenges are part of the OverTheWire platform and remain the property of their respective creators.*