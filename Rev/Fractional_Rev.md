# Fractional Rev — ImaginaryCTF Writeup

**Challenge:** Fractional Rev  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{The_Very_Basics_of_FRACTRAN}`

---

## Description

> Everyone knows that Life is Turing complete, but what about fractions...

**Attachment:** FRACTRAN program

---

## Background Knowledge (Read This First!)

### What is FRACTRAN?

**FRACTRAN** is an esoteric programming language invented by John Conway. A FRACTRAN program is just a list of fractions. Execution works like this:

1. Start with a number `n`
2. Go through the fractions in order
3. Multiply `n` by the first fraction that gives a whole number result
4. Repeat from the beginning with the new `n`
5. Stop when no fraction gives a whole number

Despite this simplicity, FRACTRAN is **Turing complete** — it can compute anything!

### How is the Flag Encoded?

Running this FRACTRAN program outputs a large power of 613:

```
613^12203154003490548175721013914001941979229278133070782051717611318191896301489789
```

The **exponent** of 613 encodes the flag. Decode the exponent to get the plaintext.

---

## Solution

### Step 1 — Download and run the FRACTRAN program

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O program.frac
└─$ cat program.frac
# → A list of fractions
```

### Step 2 — Run it (or read the exponent from the description)

The output exponent is:
```
12203154003490548175721013914001941979229278133070782051717611318191896301489789
```

### Step 3 — Decode the exponent to ASCII

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c "
exp = 12203154003490548175721013914001941979229278133070782051717611318191896301489789
from Crypto.Util.number import long_to_bytes
print(long_to_bytes(exp))
"
# → ictf{The_Very_Basics_of_FRACTRAN}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Decode the exponent from the FRACTRAN output | ⭐ Easy |
| FRACTRAN interpreter (optional) | Actually run the program | ⭐⭐ Medium |

---

## Key Takeaways

- FRACTRAN is John Conway's esoteric Turing-complete language based purely on fractions
- The encoding trick: flag is stored as the exponent of a prime in the output number
- `long_to_bytes` converts a big integer directly to its ASCII representation
- "Life is Turing complete" references Conway's Game of Life — both are Conway creations
