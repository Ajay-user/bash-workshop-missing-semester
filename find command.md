If `grep` searches for content *inside* files, **`find`** is the master of searching for the files themselves. It doesn't look at what you wrote; it looks at "metadata" (name, size, age, permissions).

To use `find` like a pro, you must remember its basic anatomy:
`find [where to look] [criteria] [what to do]`

---

## 1. Searching by Name (The Basics)

The most common use is finding a file when you've forgotten where it is.

* **`-name`**: Case-sensitive search.
* **`-iname`**: **I**gnore case (finds `.JPG` and `.jpg`).

**Examples:**

```bash
# Find a file named "sample.txt" in the current directory and all subfolders
find . -name "sample.txt"

# Find all .txt files in your home directory
find ~ -iname "*.txt"

```

---

## 2. Searching by "Physical" Traits

A pro knows that files have more than just names. You can search by **Size** or **Type**.

* **`-type f`**: Look for **f**iles only.
* **`-type d`**: Look for **d**irectories (folders) only.
* **`-size`**: Use `k` (kilobytes), `M` (megabytes), or `G` (gigabytes).

**Examples:**

```bash
# Find all folders named "config"
find /etc -type d -name "config"

# Find files larger than 100 Megabytes
find /var/log -type f -size +100M

```

---

## 3. Searching by "Time" (The "Emergency" Flag)

This is vital for troubleshooting. If a server crashed 10 minutes ago, you want to see which files changed recently.

* **`-mmin -10`**: Modified in the last 10 **min**utes.
* **`-mtime -1`**: Modified in the last 24 hours (**days**).

**Example:**

```bash
# Find any config file changed in the last 30 minutes
find /etc -name "*.conf" -mmin -30

```

---

## 4. Combining Logic (The "And/Or/Not")

You can stack your criteria to get extremely specific.

* **`-a`**: And (default).
* **`-o`**: Or.
* **`!`**: Not.

**Example:**

```bash
# Find files that are (.txt OR .log) AND are larger than 1MB
find . \( -name "*.txt" -o -name "*.log" \) -a -size +1M

```

---

## 5. Taking Action (The Pro Move)

Finding the file is only half the battle. Usually, you want to *do* something to it. You have two choices: `-delete` or `-exec`.

### A. The Quick Delete

```bash
# Find and instantly delete all .tmp files (BE CAREFUL!)
find . -name "*.tmp" -delete

```

### B. The `-exec` command

This is the most powerful part of `find`. It allows you to run **any** command on every file found.

**Syntax:** `-exec [command] {} \;`

* `{}` is the placeholder for the filename.
* `\;` tells `find` the command is finished.

**Example:** Find all `.sh` files and make them executable:

```bash
find . -name "*.sh" -exec chmod +x {} \;

```

---

## 6. Comparison: `find` vs `ls` vs `grep`

| Tool | Search Level | Best For... |
| --- | --- | --- |
| **`ls`** | Surface | Seeing what is in one folder. |
| **`grep`** | Deep (Inside) | Finding a specific word inside a file. |
| **`find`** | Structural | Finding files based on their "ID card" (name, size, date). |

---

### Pro Productivity Tip: `find` + `xargs`

While `-exec` is great, pros often use `xargs` instead because it is faster for large amounts of data.

Instead of: `find . -name "*.log" -exec rm {} \;` (runs `rm` 100 times for 100 files).
Use: `find . -name "*.log" | xargs rm` (runs `rm` **once** for all 100 files).

**Would you like to see how to use `find` to fix permissions across an entire web server directory? (This is a very common real-world task!)**
