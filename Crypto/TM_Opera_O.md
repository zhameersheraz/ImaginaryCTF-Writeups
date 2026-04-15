# TM Opera O — ImaginaryCTF Writeup

**Challenge:** TM Opera O  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{I_gave_up_but_I_am_ashamed_that_he_didnt}`

---

## Description

> The centurial overlord sure had a lot of determination

**Attachment:** Matrix + ciphertext

---

## Background Knowledge (Read This First!)

### What is a Matrix Determinant?

The **determinant** of a square matrix is a single number that encodes important properties of the matrix. For a 2×2 matrix:

```
det([[a, b], [c, d]]) = a*d - b*c
```

### How is the Determinant Used Here?

The encryption uses a matrix operation. The key to decryption is realizing that the **determinant** of the encryption matrix directly gives you what you need to invert it.

For a matrix with determinant `d`, the modular inverse of `d` mod `n` lets you compute the inverse matrix, which decrypts the ciphertext.

---

## Solution

### Step 1 — Read the matrix and ciphertext

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat opera_o.txt
# → Matrix M, ciphertext ct, modulus n
```

### Step 2 — Compute the determinant and decrypt

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from Crypto.Util.number import inverse, long_to_bytes
import numpy as np

M  = [...]  # paste matrix
ct = ...    # ciphertext
n  = ...    # modulus

# Compute determinant
det = int(round(np.linalg.det(M))) % n

# Modular inverse of determinant
det_inv = inverse(det, n)

# Compute inverse matrix mod n
M_inv = [[M[1][1], -M[0][1]], [-M[1][0], M[0][0]]]
M_inv = [[(x * det_inv) % n for x in row] for row in M_inv]

# Decrypt
pt = sum(M_inv[0][i] * ct[i] for i in range(len(ct))) % n
print(long_to_bytes(pt))
# → ictf{I_gave_up_but_I_am_ashamed_that_he_didnt}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{I_gave_up_but_I_am_ashamed_that_he_didnt}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + numpy | Determinant + modular matrix inverse | ⭐⭐ Medium |

---

## Key Takeaways

- Matrix-based ciphers are invertible using the matrix determinant
- The writeup says "just use the determinant bro" — sometimes the solution really is that simple
- The flag references TM Opera O's determination — he won 8 graded races in 2000 alone
