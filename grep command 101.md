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



Regular Expressions (Regex) turn `grep` from a simple word-finder into a surgical tool. In Linux, there are two main types: **Basic (BRE)** and **Extended (ERE)**. I recommend using **`grep -E`** (Extended), as it's more intuitive and powerful.

Here are solid, real-world examples to help you master the syntax.

---

## 1. Anchors: Where is the pattern?

Anchors don't match characters; they match **positions**.

* **`^` (Caret):** Matches the start of a line.
* **`$` (Dollar):** Matches the end of a line.

**Examples:**

```bash
# Find lines that START with "1"
grep "^1" sample.txt

# Find lines that consist ONLY of the number "2" (start, 2, then end)
grep "^2$" sample.txt

# Find lines that end with a period (note: \ is used to escape the dot)
grep "\.$" file.txt

```

---

## 2. Character Sets: "Pick One from the List"

Use square brackets `[]` to match any **one** character inside them.

* **`[0-9]`**: Any single digit.
* **`[a-z]`**: Any lowercase letter.
* **`[^0-9]`**: Any character that is **NOT** a digit (the `^` inside brackets means "not").

**Examples:**

```bash
# Find lines containing 2, 3, or 4
grep "[234]" sample.txt

# Find lines that do NOT start with a number
grep "^[^0-9]" file.txt

```

---

## 3. Quantifiers: "How many times?"

These tell `grep` how many times the preceding character should appear. (Note: These work best with `grep -E`).

* **`?`**: 0 or 1 time (Optional).
* **`*`**: 0 or more times.
* **`+`**: 1 or more times.
* **`{n}`**: Exactly  times.

**Examples:**

```bash
# Find "color" or "colour" (the 'u' is optional)
grep -E "colou?r" file.txt

# Find lines with exactly three digits in a row
grep -E "[0-9]{3}" file.txt

```

---

## 4. The Wildcard and Alternation

* **`.` (Dot):** Matches **any** single character except a newline.
* **`|` (Pipe):** The "OR" operator.

**Examples:**

```bash
# Find "c", then ANY character, then "t" (matches cat, cot, c9t, etc.)
grep "c.t" file.txt

# Find lines containing "Error" OR "Warning" OR "Critical"
grep -E "Error|Warning|Critical" system.log

```

---

## 5. Pro-Level Combinations (The "Solid" Examples)

### A. Finding Valid IP Addresses (Simplified)

An IP address is four sets of numbers separated by dots.

```bash
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" logs.txt

```

### B. Finding Email Addresses

Looking for `text@text.text`:

```bash
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" contact_list.txt

```

### C. Cleaning Up a File (Removing Comments and Blank Lines)

This is a classic sysadmin trick. It finds lines that start with `#` OR are completely empty, and the `-v` flag hides them.

```bash
grep -Ev "^#|^$" config_file.conf

```

---

## Summary Table

| Symbol | Meaning | Example |
| --- | --- | --- |
| **`.`** | Any single character | `a.c` (abc, a2c) |
| **`^`** | Start of line | `^Root` |
| **`$`** | End of line | `false$` |
| **`*`** | 0 or more of previous | `ab*c` (ac, abc, abbc) |
| **`+`** | 1 or more of previous | `ab+c` (abc, abbc) |
| **`[ ]`** | One from the set | `[aeiou]` (any vowel) |
| pipe | Logical OR |


