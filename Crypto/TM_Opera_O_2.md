# TM Opera O 2 — ImaginaryCTF Writeup

**Challenge:** TM Opera O 2  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 125 pts  
**Flag:** `ictf{Meishi_Doto_famously_hates_ryuji_wada_but_liked_TM_Opera_O}`

---

## Description

> In the year 2000 TM Opera O won a staggering 8 graded races in a row, in accordance with such a tremendous accomplishment we've increased the dimension to 8

**Attachment:** 8×8 matrix system + ciphertext

---

## Background Knowledge (Read This First!)

### What Changed from TM Opera O 1?

The dimension increased from 2×2 to **8×8**. The same determinant-based approach applies, but now you need to handle an 8×8 matrix inverse modulo `n`.

### What is the Adjugate Matrix?

For larger matrices, the modular inverse is computed using the **adjugate (classical adjoint)**:

```
M^(-1) mod n  =  adj(M) * det(M)^(-1)  mod n
```

The adjugate is the transpose of the cofactor matrix. SageMath handles this directly.

---

## Solution

### Step 1 — Read the matrix and ciphertext

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat opera_o2.txt
# → 8×8 matrix M, ciphertext vector ct, modulus n
```

### Step 2 — Compute the inverse and decrypt

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath handles modular matrix inverse directly
from Crypto.Util.number import long_to_bytes

n  = ...   # modulus
M  = Matrix(Zmod(n), [...])  # 8x8 matrix
ct = vector(Zmod(n), [...])  # ciphertext vector

# Modular matrix inverse
M_inv = M.inverse()

# Decrypt
pt = M_inv * ct
flag = long_to_bytes(int(pt[0]))
print(flag)
# → ictf{Meishi_Doto_famously_hates_ryuji_wada_but_liked_TM_Opera_O}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{Meishi_Doto_famously_hates_ryuji_wada_but_liked_TM_Opera_O}
```

*(Full solve script provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | 8×8 modular matrix inverse + decryption | ⭐⭐⭐ Hard |

---

## Key Takeaways

- SageMath's `Matrix(Zmod(n), ...)` handles modular matrix inverses automatically
- The approach scales from 2×2 to 8×8 — same idea, bigger matrix
- TM Opera O's 8 consecutive graded wins in 2000 is reflected in the 8×8 dimension
