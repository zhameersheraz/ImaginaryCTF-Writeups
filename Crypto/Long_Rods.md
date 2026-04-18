# Long Rods — ImaginaryCTF Writeup

**Challenge:** Long Rods  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 65 pts  
**Flag:** `ictf{f4c70r1z4710n_pr0bl3m_1n51d3_4n_4nc13n7_c1ph3r}`

---

## Description

> Don't you hate it when your favorite cipher needs padding to encrypt, me, I just make my messages longer... Anyways go decrypt this code for the flag, shouldn't be too hard, even Plutarch knew how to do it

**Attachment:** Ciphertext + cipher parameters

---

## Background Knowledge (Read This First!)

### What is the Scytale Cipher?

The **Scytale** is one of the oldest known ciphers, used by ancient Sparta. It works by wrapping a strip of leather around a rod:
- Write the message along the rod
- When unwrapped, the letters appear scrambled
- To decrypt, wrap around a rod of the same diameter

Mathematically, it is just a **columnar transposition** — the message is written in rows and read in columns (or vice versa).

### What are "Long Rods"?

The challenge says the message is NOT padded — meaning the number of wrappings **divides the length exactly**. The message length has exactly two prime factors: **1013** and **1009**. Try both as the number of columns in the transposition.

### Why Does Factorization Matter?

Without padding, the only valid column counts are the factors of the message length. Since the length = 1013 × 1009, you try both numbers — and one of them gives readable output.

---

## Solution

### Step 1 — Download and read the ciphertext

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat ciphertext.txt
# → A long scrambled string
└─$ python3 -c "print(len(open('ciphertext.txt').read()))"
# → Length = 1013 * 1009 = 1022117
```

### Step 2 — Try both factors as column counts

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
ciphertext = open('ciphertext.txt').read().strip()
n = len(ciphertext)

for cols in [1009, 1013]:
    rows = n // cols
    # Arrange in rows x cols grid, then read by columns
    grid = [ciphertext[i*cols:(i+1)*cols] for i in range(rows)]
    decoded = ''.join(grid[r][c] for c in range(cols) for r in range(rows))
    if 'ictf{' in decoded:
        print(f"cols={cols}: {decoded[:100]}")
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{f4c70r1z4710n_pr0bl3m_1n51d3_4n_4nc13n7_c1ph3r}
```

*(A solve script is also provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Factor length → try both column counts → read transposition | ⭐ Easy |
| dcode.fr/scytale-cipher (optional) | Online Scytale solver | ⭐ Easy |

---

## Key Takeaways

- The Scytale cipher is just columnar transposition — very easy to reverse
- Without padding, only exact divisors of the message length are valid column counts
- Factoring the message length narrows down the options immediately
- "Even Plutarch knew how to do it" — Plutarch documented the Scytale in ancient Greece!
