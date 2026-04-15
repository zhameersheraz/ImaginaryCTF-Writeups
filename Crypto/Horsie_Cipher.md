# Horsie Cipher — ImaginaryCTF Writeup

**Challenge:** Horsie Cipher  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{heavy_duty_multiplication_for_heavy_duty_cryptography}`

---

## Description

> It might not be the fastest cipher but it tries its best...

**Attachment:** Custom cipher source + ciphertext

---

## Background Knowledge (Read This First!)

### What is the Horsie Cipher?

The challenge defines a custom function `haru_cipher(a, b, c)`. It looks complicated with many operations, but after careful analysis:

```
haru_cipher(a, b, c)  =  a * b
```

It is literally just **multiplication**! All the extra steps cancel out and reduce to a simple multiply.

### Why is this a vulnerability?

If `encrypt(plaintext) = plaintext * key`, then:

```
decrypt(ciphertext) = ciphertext / key = ciphertext * modular_inverse(key)
```

You just need to find the modular inverse of the key. If you can observe a known plaintext-ciphertext pair, you recover the key immediately:

```
key = ciphertext * modular_inverse(plaintext)
```

---

## Solution

### Step 1 — Analyze the cipher source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat horsie_cipher.py
# → Read haru_cipher(a, b, c) — simplify it mentally or on paper
# → Realize: it just computes a * b
```

### Step 2 — Recover the key using a known pair

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
# If encrypt(pt) = pt * key mod n
# Then key = ct * modular_inverse(pt) mod n

from Crypto.Util.number import inverse

pt  = ...   # known plaintext
ct  = ...   # corresponding ciphertext
n   = ...   # modulus

key  = ct * inverse(pt, n) % n

# Decrypt the flag
flag_ct  = ...
flag_pt  = flag_ct * inverse(key, n) % n

from Crypto.Util.number import long_to_bytes
print(long_to_bytes(flag_pt))
```

### Step 3 — Run and get the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{heavy_duty_multiplication_for_heavy_duty_cryptography}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Modular inverse + decryption | ⭐⭐ Medium |
| Mathematical analysis | Simplify haru_cipher to multiplication | ⭐⭐ Medium |

---

## Key Takeaways

- Always simplify custom cipher functions — they often reduce to something trivial
- Multiplication-based ciphers are trivially broken with modular inverse
- If `E(x) = x * k`, recovery is just `k = E(x) * x^{-1} mod n`
