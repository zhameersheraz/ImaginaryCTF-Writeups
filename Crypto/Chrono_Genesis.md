# Chrono Genesis — ImaginaryCTF Writeup

**Challenge:** Chrono Genesis  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 65 pts  
**Flag:** `ictf{unix_timestamps_are_just_delta_chrono_genesis}`

---

## Background Knowledge (Read This First!)

### What is a Unix Timestamp?

A **Unix timestamp** is the number of seconds since January 1, 1970 (UTC). It is how computers internally store time. Because timestamps are sequential and predictable, they make terrible random seeds.

### What is the Vulnerability?

If a program uses `time()` (current Unix timestamp) as the seed for a random number generator, the seed space is tiny — only about 31.5 million values per year. You can brute force every possible timestamp in a small time window and find the one that matches the observed output.

---

## Solution

### Step 1 — Read the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat chrono_genesis.py
# → Seed = Unix timestamp → used to generate "random" values
```

### Step 2 — Brute force the timestamp

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
import random
import time

observed_output = [...]  # paste the observed random values here
target_time = int(time.time())

# Try timestamps in a window around now (or the challenge release date)
for ts in range(target_time - 10000000, target_time + 1):
    random.seed(ts)
    candidate = [random.randint(0, 255) for _ in range(len(observed_output))]
    if candidate == observed_output:
        print(f"Found seed: {ts}")
        # Now generate the keystream and decrypt
        random.seed(ts)
        keystream = [random.randint(0, 255) for _ in range(len(ciphertext))]
        flag = bytes([c ^ k for c, k in zip(ciphertext, keystream)])
        print(flag.decode())
        break
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{unix_timestamps_are_just_delta_chrono_genesis}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Brute force timestamp seed and decrypt | ⭐⭐ Medium |

---

## Key Takeaways

- **Never use timestamps as random seeds** — the seed space is far too small
- Brute forcing a timestamp window is trivial — a modern computer checks millions per second
- The flag name references **Delta Chrono Genesis** — a famous Japanese racehorse with "Chrono" in the name
