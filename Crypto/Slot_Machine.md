# Slot Machine — ImaginaryCTF Writeup

**Challenge:** Slot Machine  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{r4nd0m_n0t_g4mbl3}`

---

## Description

> Gambling is ~~good~~ bad

**Connection:** `nc 155.248.210.243 42185`

---

## Background Knowledge (Read This First!)

### What is the Vulnerability?

Upon registration, the username is appended with a **random string** generated from an 8-bit seed (`srand` seeded with 8 bits = only 256 possible values!). 

By brute-forcing all 256 possible seeds and comparing against the registered username suffix, you can identify the exact seed used.

### Why Does This Work?

With only 256 possible seeds, you can generate the random string for all seeds locally and match them against what the server gave you. Once you know the seed, you can **predict all future random values** — including the slot machine outcomes!

---

## Solution

### Step 1 — Register and observe the username suffix

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42185
# → Register → server gives you "yourname_XXXXXXXX"
# → Note the random suffix XXXXXXXX
```

### Step 2 — Brute force all 256 seeds locally

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
import ctypes
import string

libc = ctypes.CDLL('libc.so.6')
observed_suffix = "XXXXXXXX"  # replace with actual suffix

for seed in range(256):
    libc.srand(seed)
    # Generate the random suffix the same way the server does
    candidate = ''.join(...)  # match the server's generation method
    if candidate == observed_suffix:
        print(f"Found seed: {seed}")
        # Now predict all slot machine outcomes
        break
```

### Step 3 — Predict slot outcomes and win

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → Predict winning spins → submit them → win prizes → get flag
# → ictf{r4nd0m_n0t_g4mbl3}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + ctypes | Brute force 8-bit seed to predict RNG | ⭐⭐ Medium |

---

## Key Takeaways

- An 8-bit seed has only 256 values — always brutable in milliseconds
- Once you have the seed, the entire RNG sequence is deterministic and predictable
- The username suffix leaks the seed → predict slot outcomes → win guaranteed
