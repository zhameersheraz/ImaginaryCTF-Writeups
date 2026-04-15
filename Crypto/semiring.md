# semiring — ImaginaryCTF Writeup

**Challenge:** semiring  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{l3@Rn_s0m3_Tr0pic4l_s3M1R1n9}`

---

## Description

> Wanna define some operations on a fake ring?

**Attachment:** `output.txt` with matrix M

---

## Background Knowledge (Read This First!)

### What is a Semiring?

A **semiring** is an algebraic structure with two operations (like addition and multiplication) that satisfies most ring axioms but not all. The challenge defines custom operations on a matrix over a semiring.

### What is the Key Observation?

Looking at the matrix output, the values in `M[0][1:]` (the first row, skipping the first element) encode the flag bits. Each value `c` in this row satisfies:

```
bit = (c - 1) XOR 1
```

This maps the semiring values back to binary bits, which you then convert to ASCII.

---

## Solution

### Step 1 — Read the output

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat output.txt
# → A matrix M with specific values
```

### Step 2 — Create the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from Crypto.Util.number import long_to_bytes
from ast import literal_eval

M = literal_eval(open("output.txt").read())

# Extract flag bits from first row (skip first element)
flag_bits = "".join(str((c - 1) ^ 1) for c in M[0][1:])

flag = int(flag_bits, 2)
print(long_to_bytes(flag))
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{l3@Rn_s0m3_Tr0pic4l_s3M1R1n9}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Extract and decode bits from the matrix | ⭐⭐ Medium |

---

## Key Takeaways

- Custom algebraic structures often have simple patterns hiding the flag in specific positions
- Always look at the structure of the output matrix carefully — rows and columns often encode data
- `(c - 1) XOR 1` is a simple bit-flip trick to recover binary values from modified semiring outputs
