If `grep` is a filter and `sed` is a surgeon, **`awk`** is a **data analyst**.

`awk` is more than a command; it is a programming language designed for processing columns and rows. It treats every line as a record and every word as a field.

---

## 1. The Core Logic: Columns and Fields

`awk` automatically breaks every line into fields based on whitespace (spaces or tabs).

* **`$0`**: Represents the **entire line**.
* **`$1`**: Represents the **first column**.
* **`$2`**: Represents the **second column**, and so on.

**Example:**
Imagine you run `ls -l`. It outputs many columns (permissions, owner, size, date, filename). To see **only** the filenames:

```bash
ls -l | awk '{print $9}'

```

---

## 2. Setting a Custom Delimiter (`-F`)

By default, `awk` uses spaces. If you are dealing with a CSV file (commas) or the `/etc/passwd` file (colons), you must tell `awk` what the separator is.

**The Command:**

```bash
# Get the usernames (field 1) from the password file
awk -F ":" '{print $1}' /etc/passwd

```

---

## 3. Increasing Complexity (Step-by-Step)

### Level 1: Filtering by Value

You can tell `awk` to only perform an action if a column matches a certain condition.

```bash
# Print the names of files that are larger than 5000 bytes
ls -l | awk '$5 > 5000 {print $9}'

```

### Level 2: Built-in Variables (`NR` and `NF`)

* **`NR` (Number of Records):** The current line number.
* **`NF` (Number of Fields):** How many columns are in the current line.

**Examples:**

```bash
# Print line numbers followed by the content
awk '{print NR, $0}' sample.txt

# Print only the last column of any file (useful if column counts vary)
awk '{print $NF}' sample.txt

```

### Level 3: Simple Math

`awk` can do calculations on the fly. Let's say you have a list of prices in column 2 and you want to see them with a 10% tax added:

```bash
awk '{print $1, $2 * 1.1}' prices.txt

```

---

## 4. The "Pro" Workflow: BEGIN and END

`awk` has two special blocks:

1. **`BEGIN { ... }`**: Runs **before** the file is read (perfect for headers).
2. **`END { ... }`**: Runs **after** the file is finished (perfect for totals).

**Example: Totaling a Column**
Let’s calculate the total file size of all files in your current directory:

```bash
ls -l | awk 'BEGIN {sum=0} {sum+=$5} END {print "Total Storage Used:", sum, "bytes"}'

```

---

## 5. Summary Cheat Sheet

| Task | Command |
| --- | --- |
| **Print specific columns** | `awk '{print $1, $3}' file` |
| **Filter by text** | `awk '/pattern/ {print $0}' file` |
| **Sum a column** | `awk '{sum+=$1} END {print sum}' file` |
| **Find lines with > 5 columns** | `awk 'NF > 5' file` |
| **Extract IP from logs** | `awk '{print $1}' access.log` |

---

## 6. Comparison: When to use which?

* **Use `grep**`: When you just want to **find** a line.
* **Use `sed**`: When you want to **modify** characters or words.
* **Use `awk**`: When you want to **rearrange columns** or do **math**.

### Your Challenge:

You have a file called `data.txt` where column 1 is a name and column 2 is a score.

1. Find all lines where the score is greater than 80.
2. Print the name and the word "PASSED".
3. At the very end, print "Processing Complete".

**Hint:** `awk '$2 > 80 {print $1, "PASSED"} END {print "Processing Complete"}' data.txt`

**Would you like me to show you how to combine `find`, `grep`, and `awk` into one "Mega-Command" to generate a report of every large file on your system?**
