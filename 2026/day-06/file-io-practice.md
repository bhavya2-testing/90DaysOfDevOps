# 📁 Day 06 — Read & Write Text Files in Linux

> **Challenge:** 90DaysOfDevOps 2026 | **Author:** Vaibhav Godse
> **Topic:** File I/O Operations — creating, writing, appending, reading files in Linux
---
## Table of Contents
- [Commands](#commands)
- [Hands-On](#hands-on)
---
## 🛠 Commands
### 1. Create a file — `echo` with `>`

```bash
echo "My name is John Doe." > file-practice.txt
```
| Column | Meaning |
|------|---------|
| `echo` | Prints the given text to the terminal |
| `"..."` | The text/string to write |
| `>` | **Redirect** — writes output to a file. ⚠️ Overwrites if file already exists |
| `notes.txt` | The target file (created automatically if it doesn't exist) |

---
### 2. Append to a file — `>>`

```bash
echo "I am learning DevOps." >> file-practice.txt
echo "Starting with linux commands" >> file-practice.txt
```

| Part | Meaning |
|------|---------|
| `>>` | **Append redirect** — adds to end of file without overwriting existing content |

> 💡 **`>` vs `>>`** — `>` destroys existing content, `>>` preserves it and adds below.

---
### 3. Read a file — `cat`

```bash
cat file-practice.txt
```

Prints the **entire file** content to the terminal at once.

---

### 4. Read first N lines — `head`

```bash
head -n 2 file-practice.txt
```

| Flag | Meaning |
|------|---------|
| `-n 2` | Show only the first 2 lines of the file |

**Output:**
```
My name is John Doe
I am learning DevOps
```

---

### 5. Read last N lines — `tail`

```bash
tail -n 2 file-practice.txt
```

| Flag | Meaning |
|------|---------|
| `-n 2` | Show only the last 2 lines of the file |

**Output:**
```
I am learning DevOps.
"Starting with linux commands
```

> 💡 **DevOps use case:** `tail -f /var/log/syslog` — follow a log file live during deployments.

---

### 6. Write using `tee`

```bash
echo "Testing the 'tee' command" | tee -a file-practice.txt
```

| Part | Meaning |
|------|---------|
| `\|` | Pipe — sends output of `echo` into `tee` |
| `tee` | Writes to a file **AND** prints to the terminal simultaneously |
| `-a` | Append mode — same as `>>`, doesn't overwrite |

> 💡 **Why `tee` over `>>`?**
> `>>` only writes to file silently. `tee` writes to file **and** shows output on screen at the same time — useful in scripts where you want to log AND see what's happening.

---
