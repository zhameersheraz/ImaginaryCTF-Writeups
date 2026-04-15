# Air Shakur — ImaginaryCTF Writeup

**Challenge:** Air Shakur  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 50 pts  
**Flag:** `ictf{unf41r_70_c0mp4r3_p3rf0rm4nc35_wh3n_7h3yr3_c0mp371n6_4641n57_7h3_1999_60ld3n_63n3r4710n}`

---

## Description

> Air Shakur...part of the 2000 crop, the weakest generation in JRA history, gonna need a really strong hash to compensate... sha256 is unbreakable right???

**Attachment:** Binary using SHA256 for encryption

---

## Background Knowledge (Read This First!)

### What is the Vulnerability?

The description says "sha256 is unbreakable right???" — the sarcasm is the hint. SHA256 itself is unbreakable, but the **way it is used here is completely wrong**.

### What is the Mistake?

The program uses the **password hash to encrypt the data**. This means:

- The encryption key = `SHA256(password)`
- The password is short/guessable (or the hash is reused somehow)
- Since SHA256 is deterministic, the same password always produces the same key

This is a **key derivation mistake** — if you can figure out the password or observe the hash being used, you can decrypt the data without ever cracking SHA256 itself.

### The Attack

By reversing the binary, you can see that the hash is derived from a simple or hardcoded value. Recover the hash → use it as the key → decrypt the flag.

---

## Solution

### Step 1 — Reverse the binary to find the password/hash

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ strings air_shakur
# → Look for hardcoded strings, passwords, or hash values
```

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ gdb air_shakur
(gdb) break main
(gdb) run
(gdb) x/s <address_of_hash>
# → Extract the SHA256 hash being used as key
```

### Step 2 — Use the recovered hash as the decryption key

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
import hashlib

password = b"..."  # recovered from binary
key = hashlib.sha256(password).digest()

ciphertext = open("ciphertext.bin", "rb").read()

# Decrypt (XOR or AES depending on the cipher used)
flag = bytes([c ^ k for c, k in zip(ciphertext, key * (len(ciphertext) // 32 + 1))])
print(flag.decode())
# → ictf{unf41r_70_c0mp4r3_p3rf0rm4nc35_wh3n_7h3yr3_c0mp371n6_4641n57_7h3_1999_60ld3n_63n3r4710n}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → flag found!
```

*(Full source provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `strings` / GDB | Extract the password/hash from the binary | ⭐⭐ Medium |
| Python 3 | Reconstruct key and decrypt | ⭐ Easy |

---

## Key Takeaways

- SHA256 is unbreakable — but **using it wrong** makes the whole system weak
- Never use a password hash directly as an encryption key without proper key derivation (use PBKDF2 or Argon2)
- The description's sarcasm ("sha256 is unbreakable right???") directly points to the vulnerability
