In Bash, conditional statements are how your script makes decisions. Think of it as giving your script a "brain."

To use `if` and `while` effectively, you first need to understand the **Test Bracket `[ ]**`. This is where the magic happens.

---

## 1. The `if` Statement: Making Decisions

The `if` statement checks a condition. If it's true, it runs the code; if not, it skips it (or goes to `else`).

### Basic Syntax:

```bash
if [ condition ]; then
    # code to run if true
fi

```

> **Critical Rule:** You **must** have a space after `[` and before `]`. Without spaces, Bash will throw an error!

### Real-World Example: Checking a Number

```bash
#!/bash/bin
read -p "Enter your score: " score

if [ "$score" -ge 80 ]; then
    echo "You passed with flying colors!"
elif [ "$score" -ge 50 ]; then
    echo "You passed, but keep practicing."
else
    echo "Better luck next time."
fi

```

### Comparison Cheat Sheet for `[ ]`:

| Flag | Meaning | Type |
| --- | --- | --- |
| `-eq` | Equal to | Number |
| `-ne` | Not equal to | Number |
| `-gt` | Greater than | Number |
| `-lt` | Less than | Number |
| `==` | Exactly the same | String |
| `-z` | String is empty | String |
| `-f` | File exists | File |

---

## 2. The `while` Loop: Repeating Tasks

The `while` loop keeps running as long as a condition remains **true**.

### Basic Syntax:

```bash
while [ condition ]; do
    # code to repeat
done

```

### Example: A Simple Countdown

```bash
#!/bash/bin
count=5

while [ $count -gt 0 ]; do
    echo "Counting down: $count"
    count=$((count - 1))  # Decrement the number
    sleep 1               # Pause for 1 second
done

echo "Blast off!"

```

---

## 3. The Pro Level: Reading Files with `while`

One of the most common "pro" uses of a `while` loop is reading a file line-by-line. We combine `while`, `read`, and the redirection operator `<` you learned earlier.

**Example: Processing a list of users**

```bash
while read line; do
    echo "Processing user: $line"
    # Imagine adding code here to create a folder for each user
done < users.txt

```

---

## 4. Combining `if` and `while`

Let’s combine everything we've learned—`grep`, `if`, and `while`—into a script that monitors a log file for errors.

**The "Watchdog" Script:**

```bash
#!/bash/bin
LOG_FILE="server.log"

# This loop runs forever
while true; do
    # Check if the word "ERROR" exists in the log
    if grep -q "ERROR" "$LOG_FILE"; then
        echo "Alert: Error found in logs!"
        # In a real script, you might send an email here
        break # Stop the loop once found
    fi
    echo "System healthy... checking again in 5s"
    sleep 5
done

```

* **`-q` in grep:** Stands for "quiet." It doesn't print the error; it just tells the `if` statement whether it found it or not.

---

## Summary Checklist

* **`if`**: Good for one-time checks.
* **`while`**: Good for repetitive tasks or waiting for something to happen.
* **`[ ]`**: The "test" area. Always use spaces inside.
* **`$(( ... ))`**: How you do math inside a script.

### Your Bash Challenge:

Write a script that:

1. Asks the user to guess a secret number (e.g., 7).
2. Uses a `while` loop to keep asking until they get it right.
3. Uses an `if` statement inside the loop to tell them if their guess was "Too High" or "Too Low."

```
#!/bin/bash

read -p "guess : " guess

while [ "$guess" -ne 7 ]; do
    if [ "$guess" -gt 7 ]; then
        echo "Too high!"
    else
        echo "Too low!"
    fi
    read -p "guess again: " guess
done

echo "Correct! ok bye"
```

---



## The "For" Loop: Processing Lists

Now that you've mastered `while` (which runs until a condition changes), you should learn the **`for`** loop. This is used when you have a **defined list** of items (like a list of files or numbers) and you want to do something to each one.

### Basic Syntax

```bash
for item in list; do
    # action
done

```

### Real-World Example: Renaming Files

Let's say you want to add the current date to every `.txt` file in your folder:

```bash
today=$(date +%F)

for file in *.txt; do
    mv "$file" "${today}_$file"
    echo "Renamed $file"
done

```

### Why use `for` instead of `while`?

* **`while`** is better for "I don't know how many times this will run" (like waiting for a user to guess a number).
* **`for`** is better for "I have 10 files, do this 10 times."

---

### Summary Checklist for Bash Scripts:

1. **Shebang:** Always start with `#!/bin/bash`.
2. **Spaces:** `[ $a -eq $b ]` needs those spaces inside the brackets!
3. **Quotes:** Wrap your variables in `"$quotes"`.
4. **Math:** Use `$(( ... ))` for calculations.

