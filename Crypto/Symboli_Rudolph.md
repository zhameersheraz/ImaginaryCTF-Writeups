# Symboli Rudolph — ImaginaryCTF Writeup

**Challenge:** Symboli Rudolph  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{yukio_okabe_wrote_smut_about_riding_him}`

---

## Description

> boomer cryptography for the boomer horse (affectionate, I actually really like this horse)

**Attachment:** Merkle-Hellman knapsack ciphertext + public key

---

## Background Knowledge (Read This First!)

### What is the Merkle-Hellman Knapsack Cryptosystem?

**Merkle-Hellman** is a public-key cryptosystem from 1978 based on the knapsack problem. It was broken by Shamir in 1982. The system:

1. Creates a **superincreasing sequence** (private key)
2. Transforms it into a **general sequence** (public key) using a modulus `q` and multiplier `r`
3. Encrypts by computing a dot product of the plaintext bits with the public key

### What is the Vulnerability Here?

The implementation is very bad — you do not even need lattice reduction. Simply **divide each public key component by the previous one** to recover the private superincreasing sequence:

```
ratio[i] = public_key[i] / public_key[i-1]  →  reveals private key structure
```

Once you have the private key, decryption is just a greedy algorithm.

---

## Solution

### Step 1 — Read the public key

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat rudolph.txt
# → public_key = [...], ciphertext = ...
```

### Step 2 — Recover private key by dividing consecutive components

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
# Divide each public key element by the previous to get private key ratios
pub = [...]  # paste public key
ratios = [pub[i] / pub[i-1] for i in range(1, len(pub))]

# Reconstruct the superincreasing private sequence
private_key = recover_from_ratios(ratios, pub[0])

# Greedy decryption
ciphertext = ...
bits = []
remaining = ciphertext
for k in reversed(private_key):
    if remaining >= k:
        bits.append(1)
        remaining -= k
    else:
        bits.append(0)

from Crypto.Util.number import long_to_bytes
flag = long_to_bytes(int(''.join(map(str, reversed(bits))), 2))
print(flag)
# → ictf{yukio_okabe_wrote_smut_about_riding_him}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{yukio_okabe_wrote_smut_about_riding_him}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Ratio attack to recover private key + greedy decrypt | ⭐⭐ Medium |

---

## Key Takeaways

- Merkle-Hellman was broken in 1982 — never use it for real cryptography
- A bad implementation makes it even easier — no lattice reduction needed here
- Dividing consecutive public key components reveals the private key structure directly
