# Voyager — ImaginaryCTF Writeup

**Challenge:** Voyager  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{error_correcting_ham_sandwich}`

---

## Description

> Communication over stellar distances is difficult but I'm sure you'll be able to figure it out...

**Connection:** `nc 155.248.210.243 42471`

---

## Background Knowledge (Read This First!)

### Why are error correcting codes needed for space communication?

When signals travel billions of kilometers (like from Voyager spacecraft), they get corrupted by noise. **Error correcting codes** add redundant data that lets the receiver detect and fix errors automatically.

### What is a (4,3) Hamming Code?

A **(n, k) Hamming code** encodes `k` data bits into `n` total bits (adding `n-k` parity bits). A **(4,3) Hamming code** encodes 3 data bits into 4 bits:

- Bit positions 1, 2, 4 are parity bits
- Bit position 3 carries data

To decode:
1. Check each parity bit
2. The XOR of failing parity positions gives the error position
3. Flip that bit to correct the error
4. Extract the 3 data bits

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42471
# → Server sends corrupted data, expects correct decoded output
```

### Step 2 — Implement the (4,3) Hamming decoder

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from pwn import *

def hamming_decode_4_3(bits):
    # bits is a list of 4 bits [b1, b2, b3, b4]
    # Parity checks
    p1 = bits[0] ^ bits[1] ^ bits[3]  # covers positions 1,2,4
    p2 = bits[0] ^ bits[2] ^ bits[3]  # covers positions 1,3,4
    # Error position
    error = p1 * 1 + p2 * 2
    if error:
        bits[error - 1] ^= 1  # flip the error bit
    # Extract data bits (position 3 = index 2)
    return bits[2]

r = remote('155.248.210.243', 42471)
# Receive corrupted codewords, decode, send back
while True:
    data = r.recvline()
    # Parse and decode using hamming_decode_4_3
    # Send corrected output
r.interactive()
```

### Step 3 — Run and get the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{error_correcting_ham_sandwich}
```

*(A golf-optimized implementation is provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Implement (4,3) Hamming decoder and interact with server | ⭐⭐ Medium |

---

## Key Takeaways

- (4,3) Hamming codes correct single-bit errors in 4-bit codewords
- The error position = XOR of all failing parity check indices
- Space communication (Voyager) uses error correcting codes — that is the theme here
- This also works as a code golf challenge — try to implement it as short as possible!
