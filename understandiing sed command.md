If `grep` is a filter, **`sed`** is a surgeon.

`sed` stands for **S**tream **Ed**itor. Unlike a normal text editor (like Notepad or Nano), you don't open a window to change text; you send commands to the "stream" of data as it passes through.

---

## 1. The Basic Syntax

The most common use of `sed` is for **Substitution**.

`sed 's/old_text/new_text/' filename`

* **`s`**: The command for **S**ubstitute.
* **`/ / /`**: These are delimiters (like fences) that separate your search and replace terms.

---

## 2. The "Must-Know" Flags

To be productive, you need to know how to control the behavior of the substitution.

| Flag | Name | Purpose |
| --- | --- | --- |
| **`-i`** | **In-place** | **Warning:** This actually changes the file. Without it, `sed` just prints the result to your screen. |
| **`g`** | **Global** | Replaces *every* instance on a line. Without it, `sed` only changes the first one it sees. |
| **`-n`** | **Silent** | Tells `sed` not to print anything unless we specifically ask (used with the `p` command). |
| **`-e`** | **Expression** | Allows you to run multiple commands at once. |

---

## 3. Increasing Complexity (Step-by-Step)

### Level 1: The Basic Swap

Let's say you want to change "2" to "Two" in your `sample.txt`.

```bash
sed 's/2/Two/' sample.txt

```

*Note: This only changes the screen output. Your file is still safe.*

### Level 2: Changing the File Forever

To actually save your changes, use the `-i` flag. **Pro tip:** Use `-i.bak` to create a backup just in case you mess up the command.

```bash
sed -i.bak 's/2/Two/g' sample.txt

```

### Level 3: Deleting Lines

`sed` isn't just for swapping; it's great for cleaning.

* **Delete line 3:** `sed '3d' sample.txt`
* **Delete lines 5 through 8:** `sed '5,8d' sample.txt`
* **Delete every line containing "4":** `sed '/4/d' sample.txt`

### Level 4: The Power of `&` (The "Add-on")

The `&` symbol represents "whatever was found in the search."
If you want to wrap every number in brackets:

```bash
sed 's/[0-9]/[&]/g' sample.txt
# Result: [1], [2], [3]...

```

---

## 4. Being a "Pro": Productivity Tricks

### A. Multiple Changes at Once

Don't run `sed` three times. Use `-e` to chain them:

```bash
sed -e 's/1/One/' -e 's/2/Two/' sample.txt

```

### B. Address-Based Editing

You can tell `sed` to only perform a substitution on specific lines.

* Only change "2" to "Two" **on line 10**:
`sed '10s/2/Two/' sample.txt`
* Only change "2" to "Two" **between lines 1 and 5**:
`sed '1,5s/2/Two/' sample.txt`

### C. Using Different Delimiters

If you are editing a file path (like `/usr/bin/`), using `/` as a delimiter is a nightmare: `sed 's/\/usr\/bin\//\/home\/user\//'`.
**Pro move:** Use a different character like `|` or `:`:

```bash
sed 's|/usr/bin/|/home/user/|' filename

```

---

## 5. Summary Cheat Sheet

| Task | Command |
| --- | --- |
| **Substitute first match** | `sed 's/old/new/' file` |
| **Substitute all matches** | `sed 's/old/new/g' file` |
| **Delete empty lines** | `sed '/^$/d' file` |
| **Print only specific lines** | `sed -n '5,10p' file` |
| **Insert text at the top** | `sed '1i "Header Text"' file` |
| **Append text at the bottom** | `sed '$a "Footer Text"' file` |

---

### Your Pro Challenge:

You have a list of numbers. Can you write a `sed` command that:

1. Deletes the first line.
2. Replaces the number "7" with the word "LUCKY".
3. Saves the changes to the file.


### Capture groups

Capture groups are the "memory" of the `sed` world. They allow you to snatch pieces of text and store them in numbered "pockets" to be reused or rearranged later.

In `sed`, you create a capture group by wrapping a part of your regex in **escaped parentheses**: `\( \)` (or just `( )` if using Extended Regex with `-E`).

---

## 1. The Anatomy of a Swap

To swap "John Doe" to "Doe, John", we need to identify two distinct groups of text separated by a space.

**The Command:**

```bash
echo "John Doe" | sed -E 's/([A-Z][a-z]+) ([A-Z][a-z]+)/\2, \1/'

```

### How it works:

1. **`([A-Z][a-z]+)`**: This is **Group 1**. It looks for a capital letter followed by lowercase letters (the first name).
2. **`     `**: A literal space separates the two names.
3. **`([A-Z][a-z]+)`**: This is **Group 2**. It looks for the second name.
4. **`\2, \1`**: This is the **Backreference**.
* `\2` pulls whatever was found in the second set of parentheses.
* `\1` pulls whatever was found in the first set.
* We added a literal comma and space in between.



---

## 2. Real-World Examples

### A. Swapping Dates (YYYY-MM-DD to DD/MM/YYYY)

If you have a log file with dates like `2023-10-25` and your boss wants them in a different format:

```bash
echo "2023-10-25" | sed -E 's/([0-9]{4})-([0-9]{2})-([0-9]{2})/\3\/\2\/\1/'

```

* `\1` = 2023
* `\2` = 10
* `\3` = 25
* **Result:** `25/10/2023`

### B. Cleaning Up Key-Value Pairs

If you have a config file like `Password = Secret123` and you want to format it as `Secret123:Password`:

```bash
echo "Password = Secret123" | sed -E 's/(.*) = (.*)/\2:\1/'

```

---

## 3. The "Pro" Rules for Capture Groups

* **Max Limit:** You can have up to **9** capture groups (`\1` through `\9`).
* **Nesting:** You can put groups inside groups! `sed` numbers them based on the order of the opening parenthesis `(`.
* **Extended vs. Basic:** * **Basic (`sed`):** You must escape parentheses: `\(\)`.
* **Extended (`sed -E`):** You use plain parentheses: `()`. **Always use `-E**` for capture groups; it’s much easier to read.



---

## 4. Why this makes you productive

Imagine you have a CSV file with 10,000 rows where the "Last Name, First Name" are in the wrong columns.
`sed -i -E 's/([^,]+), ([^,]+)/\2 \1/' data.csv`

In less than a second, you've reformatted the entire database without ever opening Excel.

---

### One Last Trick: Deleting Everything *Except* the Group

If you have a line like `user_id: 5521 (active)` and you ONLY want the number:

```bash
echo "user_id: 5521 (active)" | sed -E 's/.*: ([0-9]+).*/\1/'

```

By matching the *entire* line (`.*` at start and end) but capturing only the digits, the `\1` replacement effectively deletes everything else.

