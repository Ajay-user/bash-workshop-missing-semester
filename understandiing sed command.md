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

**Would you like to see how to combine `grep` and `sed` together using "pipes" to perform complex data surgery?**
