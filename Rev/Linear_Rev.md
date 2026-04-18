# Linear Rev — ImaginaryCTF Writeup

**Challenge:** Linear Rev  
**Category:** Rev  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{4_l177l3_h4rd3r_7h4n_v163n3r3}`

---

## Description

> Short and simple flag cracking, get to it

**Attachment:** Binary + encrypted flag

---

## Background Knowledge (Read This First!)

### What is a Stream Cipher Based on 2×2 Matrices?

Instead of encrypting one byte at a time, this cipher encrypts **pairs of bytes** using 2×2 matrix multiplication over **Z(256)** (integers modulo 256).

Each pair of plaintext bytes `(a, b)` is multiplied by a 2×2 key matrix `M`:

```
[c]   [M11 M12] [a]
[d] = [M21 M22] [b]  (mod 256)
```

### Why is it Linear?

Matrix multiplication over Z(256) is a **linear operation**. This means:
- If you know a plaintext-ciphertext pair, you can recover the matrix
- The matrix can then be inverted (if it has a modular inverse) to decrypt everything else

### How do you invert a 2×2 matrix mod 256?

```
M^(-1) = det(M)^(-1) * [[M22, -M12], [-M21, M11]]  (mod 256)
```

The determinant must be **coprime to 256** (odd) for the inverse to exist.

*(The full source is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the binary and encrypted flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url>
└─$ ghidra
# → Reverse the binary → extract the 2x2 key matrix
```

### Step 2 — Invert the matrix and decrypt

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
# Key matrix extracted from binary
M = [[a, b], [c, d]]  # fill from reversing

# Compute inverse mod 256
det = (M[0][0]*M[1][1] - M[0][1]*M[1][0]) % 256
det_inv = pow(det, -1, 256)  # modular inverse

M_inv = [
    [(M[1][1] * det_inv) % 256,  (-M[0][1] * det_inv) % 256],
    [(-M[1][0] * det_inv) % 256, (M[0][0] * det_inv) % 256]
]

# Decrypt pairs of bytes
ciphertext = open('encrypted.bin', 'rb').read()
flag = b''
for i in range(0, len(ciphertext), 2):
    c1, c2 = ciphertext[i], ciphertext[i+1]
    p1 = (M_inv[0][0]*c1 + M_inv[0][1]*c2) % 256
    p2 = (M_inv[1][0]*c1 + M_inv[1][1]*c2) % 256
    flag += bytes([p1, p2])

print(flag)
# → ictf{4_l177l3_h4rd3r_7h4n_v163n3r3}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{4_l177l3_h4rd3r_7h4n_v163n3r3}
```

*(Full source provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Ghidra | Extract the key matrix from the binary | ⭐⭐ Medium |
| Python 3 | Invert matrix mod 256 and decrypt | ⭐⭐ Medium |

---

## Key Takeaways

- 2×2 matrix ciphers over Z(256) are linear — invertible with modular matrix inverse
- The determinant must be odd (coprime to 256) for the inverse to exist
- "A little harder than Vigenère" — it operates on byte pairs instead of single bytes, but the linearity remains
