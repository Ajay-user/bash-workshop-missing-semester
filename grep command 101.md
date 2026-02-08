To become a `grep` pro, you have to think of it as a high-speed **filter**. Its name stands for **G**lobal **R**egular **E**xpression **P**rint. Basically, it looks through text and pulls out only the lines that match your "search pattern."

Let’s use the `sample.txt` file you created (the one with the numbers) to learn.

---

## Step 1: The Basic Search

The simplest way to use `grep` is to find a specific string in a file.

**Command:**

```bash
grep "2" sample.txt

```

**What happens:** It scans the file and prints every line that contains the number `2`.

---

## Step 2: Useful "Quality of Life" Flags

In the real world, you usually need more than just the line itself. Here are the three most common flags:

* **`-n` (Line Numbers):** Shows you which line the match is on.
* **`-c` (Count):** Instead of showing the lines, it just tells you *how many* times the pattern appeared.
* **`-v` (Invert):** This is the "everything but" flag. It shows lines that **do NOT** match.

**Try this:**

```bash
grep -n "4" sample.txt
# Output might look like: 4:4 and 6:4 (Line 4 and Line 6)

grep -v "2" sample.txt
# This shows every number in your file EXCEPT the 2s.

```

---

## Step 3: Case Insensitivity and Multiple Files

If you start searching through text files with words, you’ll run into the "Capitalization Problem." By default, `grep` is case-sensitive.

* **`-i` (Ignore Case):** Finds "Apple", "apple", and "APPLE".
* **`*` (Wildcard):** Searches through every file in your current folder.

**Command:**

```bash
grep -i "error" *

```

*This is a "pro" move for troubleshooting logs—it finds the word "error" regardless of case in every file you have.*

---

## Step 4: Using "Context" (The Secret Weapon)

Sometimes you need to see what happened *around* the line you found. This is vital for debugging code or reading logs.

| Flag | Meaning | Result |
| --- | --- | --- |
| **`-B`** | **B**efore | Shows  lines before the match. |
| **`-A`** | **A**fter | Shows  lines after the match. |
| **`-C`** | **C**ontext | Shows  lines before AND after. |

**Command:**

```bash
grep -A 2 "7" sample.txt

```

*This finds "7", then shows you the 2 lines that come immediately after it.*

---

## Step 5: Regular Expressions (The "Pro" Level)

This is where `grep` gets its real power. You can use symbols to represent patterns rather than exact words.

* **`^` (Caret):** Matches the start of a line.
* **`$` (Dollar):** Matches the end of a line.
* **`.` (Dot):** Matches any single character.

**Example:** Let's say you want to find lines that **start** with the number 1.

```bash
grep "^1" sample.txt

```

---

## Summary Table for Your Cheat Sheet

| Command | Purpose |
| --- | --- |
| `grep "word" file` | Basic search |
| `grep -i "word" file` | Search ignoring uppercase/lowercase |
| `grep -r "word" .` | **Recursive** search (search every file in every sub-folder) |
| `grep -w "word" file` | Search for the **whole word** only (won't match "wordy") |
| `grep -E "pattern1 | pattern2" file` |

---

### Challenge for you:

Try to find how many lines in your `sample.txt` **do not** contain the number "2" and show their line numbers.
Ans : grep -v 2 -n sample.txt 

