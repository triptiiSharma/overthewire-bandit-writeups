# 🚩 Level 11 → Level 12

## Level Goal

The password for the next level is stored in `data.txt`, where all letters have been rotated by 13 positions using the ROT13 cipher. Both uppercase and lowercase letters are affected.

## Commands Used

- `ls`
- `cat`
- `tr`
- `ssh`

## Walkthrough

Once logged in as `bandit11`, list the contents of the home directory:
```bash
ls
```

You will find a file named `data.txt`. Reading it with `cat` reveals ROT13 encoded text:
```bash
cat data.txt
```

Output:
```
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```

To decode this, the letters need to be shifted back by 13 positions. A first attempt using `tr` to swap case does not decode ROT13 — it only converts lowercase to uppercase and vice versa, which is a different operation entirely:
```bash
echo "Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4" | tr '[:lower:]' '[:upper:]'   # incorrect
```

The correct approach is to map each letter to the one 13 positions ahead of it in the alphabet, wrapping around at the end. This is done by providing explicit character ranges to `tr`:
```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
```

Breaking down the translation map:
- `A-Z` → `N-ZA-M` : uppercase letters are shifted by 13, wrapping from Z back to A
- `a-z` → `n-za-m` : lowercase letters are shifted by 13, wrapping from z back to a

This will output the password for the next level:
```
7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **ROT13:** A simple substitution cipher that replaces each letter with the letter 13 positions after it in the alphabet. Since the alphabet has 26 letters, applying ROT13 twice returns the original text — encoding and decoding use the exact same operation.
- **`tr` command:** Translates or replaces characters from one set to another. It operates on a character-by-character basis and is well suited for simple substitution tasks like ROT13.
- **Character ranges in `tr`:** Ranges like `A-Z` and `N-ZA-M` define explicit mapping tables. The first set is the input and the second set is the output, matched positionally.
- **Input redirection (`<`):** The `<` operator feeds the contents of a file into a command as standard input, as an alternative to piping with `cat`.

[⬅ Back to Home](../README.md)