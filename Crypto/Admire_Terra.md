# Admire Terra — ImaginaryCTF Writeup

**Challenge:** Admire Terra  
**Category:** Crypto  
**Difficulty:** Easy-Medium  
**Points:** 60 pts  
**Flag:** `ictf{if_terra_hadnt_blocked_croix_du_nord_he_wouldve_won_totally_not_cope}`

---

## Description

> Bro got disqualified but at least it was funny

**Attachment:** Ciphertext file

---

## Background Knowledge (Read This First!)

### What is a Vigenère Cipher?

A **Vigenère cipher** encrypts text using a repeating keyword. Each letter of the plaintext is shifted by the corresponding letter of the keyword. For example with key `CAT`:

```
Plaintext:  H  E  L  L  O
Key:        C  A  T  C  A
Ciphertext: J  E  E  N  O
```

The key repeats as many times as needed.

### The Key Insight — Combining Vigenère Ciphers

If you apply **two Vigenère ciphers one after another** with key lengths `a` and `b`, the result is equivalent to a **single Vigenère cipher** with key length `LCM(a, b)` — the Least Common Multiple.

For example:
- Key 1 length = 4
- Key 2 length = 6
- Combined key length = LCM(4, 6) = **12**

This means the combined cipher is still just a Vigenère cipher — just with a longer key. You can crack it using standard Vigenère analysis!

### How do you crack a Vigenère cipher?

1. **Find the key length** using the Kasiski test or Index of Coincidence
2. **Solve each Caesar shift** independently (one per key position)
3. **Reconstruct the key** and decrypt

---

## Solution

### Step 1 — Download and inspect the ciphertext

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat ciphertext.txt
# → Looks like garbled text — a Vigenère cipher
```

### Step 2 — Use an online Vigenère solver

Go to `https://www.dcode.fr/vigenere-cipher` and paste the ciphertext. Use **Automatic Decrypt** — it will find the key length using Index of Coincidence and solve it automatically.

Or use Python:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
# The combined Vigenère is just another Vigenère — crack it normally
# Use index of coincidence to find key length, then frequency analysis per position
from itertools import cycle

def vigenere_decrypt(ciphertext, key):
    result = []
    key_cycle = cycle(key.upper())
    for c in ciphertext:
        if c.isalpha():
            shift = ord(next(key_cycle)) - ord('A')
            dec = chr((ord(c.upper()) - shift - ord('A')) % 26 + ord('A'))
            result.append(dec)
        else:
            result.append(c)
    return ''.join(result)

# After finding key via dcode.fr or IC analysis:
print(vigenere_decrypt(ciphertext, "FOUND_KEY"))
```

### Step 3 — Read the flag

```
ictf{if_terra_hadnt_blocked_croix_du_nord_he_wouldve_won_totally_not_cope}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| dcode.fr/vigenere-cipher | Automatic Vigenère decryption | ⭐ Easy |
| Python 3 | Manual decryption once key is known | ⭐⭐ Medium |

---

## Key Takeaways

- Combining Vigenère ciphers of lengths `a` and `b` gives a Vigenère of length `LCM(a, b)` — not more secure!
- Vigenère ciphers are always crackable with enough ciphertext using frequency analysis
- The Index of Coincidence is the go-to method for finding Vigenère key length
- dcode.fr is an extremely useful tool for classical cipher challenges
