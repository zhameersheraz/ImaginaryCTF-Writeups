# Wrong ssh — ImaginaryCTF Writeup

**Challenge:** Wrong ssh  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 80 pts  
**Flag:** `ictf{y37_4n07h3r_64mbl1n6_ch4ll?!?}`

---

## Description

> I've heard MD4 is insecure so I made this new super secure hash to replace it, guess my 1 in a 100 million password to win big 🎰🎰🎰

**Connection:** `nc 155.248.210.243 42191`

---

## Background Knowledge (Read This First!)

### What is a Timing Attack?

A **timing attack** exploits the fact that some operations take different amounts of time depending on the data. For example, if a string comparison stops at the first mismatch:

```
compare("abc", "axc") → stops at position 1 → faster
compare("abc", "abx") → stops at position 2 → slower
```

By measuring response times, you can determine the correct character at each position!

### How Does This Work Here?

The server compares your password guess against the real password. The comparison is **not constant-time** — it stops early on mismatch. By measuring how long the server takes to respond for each guess:

- Longer response = more characters matched → you found the correct prefix
- Shorter response = early mismatch → wrong character

Repeat byte by byte → recover the full password.

---

## Solution

### Step 1 — Create the timing attack script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from pwn import *
import time
import string

charset = string.ascii_letters + string.digits + string.punctuation
password = ''

for position in range(20):  # try up to 20 chars
    best_char  = ''
    best_time  = 0
    for c in charset:
        guess = password + c
        times = []
        for _ in range(5):  # average multiple attempts
            r = remote('155.248.210.243', 42191)
            r.recvuntil(b'password: ')
            start = time.time()
            r.sendline(guess.encode())
            r.recvall(timeout=1)
            elapsed = time.time() - start
            times.append(elapsed)
            r.close()
        avg = sum(times) / len(times)
        if avg > best_time:
            best_time = avg
            best_char = c
    password += best_char
    print(f"Found so far: {password}")
    if password.endswith('}'):
        break

print("Flag:", password)
```

### Step 2 — Run it (takes some time)

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{y37_4n07h3r_64mbl1n6_ch4ll?!?}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Timing attack — measure response time per character | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Non-constant-time comparisons leak which prefix is correct via timing differences
- Always use `hmac.compare_digest()` or similar for secure comparisons
- Averaging multiple timing measurements reduces noise from network jitter
- The flag jokes about it being "yet another gambling challenge" — but it is really a timing attack!
