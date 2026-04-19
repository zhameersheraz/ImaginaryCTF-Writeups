# multiplication — ImaginaryCTF Writeup

**Challenge:** multiplication  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{sT4nD@rd_D3Via71oN}`

---

## Description

> Can you identify the impostor multiplication tables?

**Attachment:** `out.txt` with multiplication table data

---

## Background Knowledge (Read This First!)

### What is Standard Deviation?

**Standard deviation** measures how spread out values are from their mean. A high standard deviation = values are spread far apart. A low standard deviation = values are clustered together.

### What is the Trick?

Real multiplication tables (like 3×7=21, 5×8=40...) have **high standard deviation** — the products are spread across a wide range.

Fake/random data tends to be more **uniform** — lower standard deviation.

By computing the standard deviation of each row in the output, you can distinguish real multiplication tables (high σ) from impostors (low σ):

- `std > 0.00135` → bit = `0` (real multiplication table)
- `std ≤ 0.00135` → bit = `1` (impostor)

Collect all bits → convert from binary to ASCII → get the flag.

---

## Solution

### Step 1 — Download the output file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O out.txt
```

### Step 2 — Create and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from numpy import std
from Crypto.Util.number import long_to_bytes

with open('out.txt') as file:
    exec(file.read())

msg = ''
for a in ct:
    if std(a) > 0.00135:
        msg += '0'
    else:
        msg += '1'

print(long_to_bytes(int(msg, 2)))
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{sT4nD@rd_D3Via71oN}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + numpy | Compute standard deviation to classify each table | ⭐⭐ Medium |

---

## Key Takeaways

- Standard deviation distinguishes structured data (multiplication tables) from random data
- The threshold `0.00135` separates the two distributions — find it by plotting or binary search
- Statistical side channels are powerful — even subtle distribution differences carry information
