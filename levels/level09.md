# 🚩 Level 8 → Level 9

## Level Goal

The password for the next level is stored in `data.txt` and is the only line of text that appears exactly once across the entire file.

## Commands Used

- `ls`
- `sort`
- `uniq`
- `ssh`

## Walkthrough

Once logged in as `bandit8`, list the contents of the home directory:
```bash
ls
```

You will find a file named `data.txt`. The file contains many lines, most of which are duplicated. The goal is to isolate the one unique line.

`uniq -u` filters out duplicate lines, but it only detects duplicates that are **adjacent** to each other. Running it directly on an unsorted file will not work correctly:
```bash
uniq -u data.txt        # incorrect — duplicates must be adjacent for uniq to detect them
```

Similarly, running `sort` and `uniq -u` as two separate commands does not work either, since `uniq -u` in the second command still operates on the original unsorted file, not the sorted output:
```bash
sort data.txt           # sorts but output is not passed forward
uniq -u data.txt        # still reads original file, not the sorted result
```

The correct approach is to pipe the output of `sort` directly into `uniq -u`, so that `uniq` operates on the sorted data:
```bash
sort data.txt | uniq -u
```

This will output the only line that occurs exactly once:
```
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

Exit the current session:
```bash
exit
```

Alternatively, press `Ctrl + D` to close the session. Now log into the next level:
```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password retrieved above.

## Concepts Covered

- **`sort`:** Arranges lines of a file in alphabetical or numerical order. A critical prerequisite for `uniq`, since duplicate detection requires matching lines to be adjacent.
- **`uniq -u`:** Filters a file or input stream, outputting only lines that are not repeated. The `-u` flag stands for unique — it suppresses all lines that appear more than once.
- **Pipe operator (`|`):** Passes the standard output of one command directly as the standard input to the next. This allows commands to be chained together without writing intermediate results to a file.
- **Order of operations:** Running `sort` and `uniq -u` as separate commands does not chain their effects — each reads from the original file independently. Piping is the correct way to pass output between commands.

[⬅ Back to Home](../README.md)