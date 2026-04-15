# Oguri Cap — ImaginaryCTF Writeup

**Challenge:** Oguri Cap  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{hmmmm_hamming_sandwich_code}`

---

## Description

> Due to recent economic downturn I can no longer afford to keep this horse fed, thats where you come in

**Attachment:** Binary with Hamming code implementation

---

## Background Knowledge (Read This First!)

### What is a Hamming Code?

A **Hamming code** is an error-correcting code that adds redundant parity bits to data so that single-bit errors can be detected and corrected. It is one of the most classic concepts in information theory.

### How Does it Work?

For every block of data bits, Hamming codes insert parity bits at positions that are powers of 2 (1, 2, 4, 8, 16, ...). Each parity bit covers a specific set of data bits. To decode:

1. Check each parity bit
2. If a parity error is found, the position of the error is the XOR of all failing parity positions
3. Flip that bit to correct the error
4. Extract the original data bits (remove parity bit positions)

### Why is Hamming Code Used in CTFs?

Hamming codes appear in Rev challenges because:
- Encoding is straightforward but decoding requires knowing the scheme
- Reversing the binary reveals the Hamming parameters
- Once you know the parameters, decoding is a standard algorithm

---

## Solution

### Step 1 — Reverse the binary to understand the Hamming parameters

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ strings oguri_cap
└─$ objdump -d oguri_cap | less
# → Identify block size, parity bit positions
```

### Step 2 — Implement Hamming decoder

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
encoded = [...]  # encoded data from binary output

def hamming_decode(block):
    n = len(block)
    # Calculate parity checks
    error_pos = 0
    for i in range(int(n).bit_length()):
        parity = 0
        for j in range(1, n+1):
            if j & (1 << i):
                parity ^= block[j-1]
        if parity:
            error_pos += (1 << i)
    # Correct error if any
    if error_pos:
        block[error_pos-1] ^= 1
    # Extract data bits (non-power-of-2 positions)
    data = [block[i] for i in range(n) if (i+1) & i != 0]
    return data

from Crypto.Util.number import long_to_bytes
bits = []
for block in chunks(encoded, block_size):
    bits.extend(hamming_decode(block))

print(long_to_bytes(int(''.join(map(str, bits)), 2)))
# → ictf{hmmmm_hamming_sandwich_code}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{hmmmm_hamming_sandwich_code}
```

*(Full source provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `objdump` / `strings` | Reverse the binary to find Hamming parameters | ⭐⭐ Medium |
| Python 3 | Implement Hamming decoder | ⭐⭐ Medium |

---

## Key Takeaways

- Hamming codes are a classic error-correcting scheme — learn the parity bit positions (powers of 2)
- Decoding: check parities → find error position → flip → extract data bits
- The "economic downturn" in the description = the horse needs to be "fed" (decoded/corrected)
