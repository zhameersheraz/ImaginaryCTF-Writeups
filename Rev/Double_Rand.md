# Double Rand — ImaginaryCTF Writeup

**Challenge:** Double Rand  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{1dk_h4v3_4_10_m1nu73_r4nd_cr4ck_pr0bl3m}`

---

## Background Knowledge (Read This First!)

### What is Python's random Module?

Python's `random` module uses the **Mersenne Twister** PRNG. If you observe enough outputs (624 32-bit values), you can **predict all future and past outputs** using a tool called `randcrack`.

### What is randcrack?

**randcrack** is a Python library that recovers the internal state of Python's Mersenne Twister from 624 observed outputs. Once you have the state, you can predict any past or future random values.

### What is the Twist?

The first 1000 random values have been **XOR-ed with C's `rand()` function** output. This adds a layer of obfuscation — but since C's `rand()` was seeded with the current time (`srand(time(NULL))`), and you know the file's modification timestamp, you can determine the C seed and remove the XOR layer.

---

## Solution

### Step 1 — Download the zip (all files together)

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O double_rand.zip
└─$ unzip double_rand.zip
└─$ ls -la encrypted.txt
# → Note the file modification timestamp — this is the C rand() seed (±1 second)
```

### Step 2 — Remove the C rand() XOR layer

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
import ctypes
import time

# Try the modification time of encrypted.txt ± a few seconds
mod_time = int(os.path.getmtime('encrypted.txt'))

for seed in range(mod_time - 2, mod_time + 3):
    libc = ctypes.CDLL('libc.so.6')
    libc.srand(seed)
    c_rand_vals = [libc.rand() & 0xFFFFFFFF for _ in range(1000)]
    
    # XOR the first 1000 outputs back to get pure Python random outputs
    # Then feed into randcrack to recover state
    # Then decrypt the flag
```

### Step 3 — Download and run the full solve

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{1dk_h4v3_4_10_m1nu73_r4nd_cr4ck_pr0bl3m}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| randcrack | Recover Python Mersenne Twister state | ⭐⭐⭐ Hard |
| Python ctypes + libc | Reproduce C rand() outputs for XOR removal | ⭐⭐ Medium |

---

## Key Takeaways

- Python's `random` module is fully predictable once you observe 624 outputs
- `srand(time(NULL))` seeds C's rand with the current timestamp — recoverable from file metadata
- Download the full zip together — file modification timestamps matter for this challenge!
