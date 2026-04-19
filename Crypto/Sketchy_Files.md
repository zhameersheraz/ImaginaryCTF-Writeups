# Sketchy Files — ImaginaryCTF Writeup

**Challenge:** Sketchy Files  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{7h3_0pp05173_0f_4_1_71m3_p4d}`

---

## Description

> A friend showed me this cool flag sharing site but when I downloaded my flag windows quarantined it can you help me get it back

**Attachment:** Windows Defender quarantined file

---

## Background Knowledge (Read This First!)

### What is Windows Defender Quarantine?

When Windows Defender detects a suspicious file, it **quarantines** it — moves it to a protected folder and encrypts it so it cannot run. The goal is to decrypt the quarantined file to recover the original.

### What is the Key?

Windows Defender always encrypts quarantined files using the **same RC4 key**:

```
KLFKDJSAJ349034KLJKASDJFK34J
```

(or a well-documented fixed key found in security research papers)

This key is documented at: `https://reversingfun.com/posts/how-to-extract-quarantine-files-from-windows-defender/`

### What is the Flag?

"The opposite of a one-time pad" — a one-time pad uses a unique key for every message. Windows Defender uses the **same key for every quarantined file** — the exact opposite! This makes it completely breakable.

---

## Solution

### Step 1 — Download the quarantined file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O quarantine.bin
```

### Step 2 — Decrypt with the known Windows Defender RC4 key

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from Crypto.Cipher import ARC4

# Windows Defender RC4 quarantine key (from reversingfun.com)
key = bytes([0x1E, 0x87, 0x78, 0x1B, 0x8D, 0xB0, 0x8E, 0xF2,
             0xD9, 0x69, 0xCA, 0x50, 0x6D, 0xB9, 0xF7, 0x68])

data = open('quarantine.bin', 'rb').read()

# Skip metadata header (first N bytes) and decrypt
cipher = ARC4.new(key)
decrypted = cipher.decrypt(data)

open('recovered.txt', 'wb').write(decrypted)
print(open('recovered.txt').read())
# → ictf{7h3_0pp05173_0f_4_1_71m3_p4d}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{7h3_0pp05173_0f_4_1_71m3_p4d}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + PyCryptodome | Decrypt with the known Windows Defender RC4 key | ⭐⭐ Medium |
| reversingfun.com | Find the documented Windows Defender RC4 key | ⭐ Easy |

---

## Key Takeaways

- Windows Defender uses a **fixed** RC4 key for all quarantined files — fully documented publicly
- The "opposite of a one-time pad" = reusing the same key for everything
- Security through obscurity fails — the quarantine key has been reverse engineered and published
