# GoSar — ImaginaryCTF Writeup

**Challenge:** GoSar  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{23_days_till_Christmas!}`

---

## Description

> You are lucky I did not compile the code...

**Attachment:** Source code file (Go)

---

## Background Knowledge (Read This First!)

### What does "You are lucky I did not compile the code" mean?

The challenge gives you the **source code** (in Go) instead of a compiled binary. This means you can read exactly how the encryption works — no reverse engineering needed!

### What is XOR encryption?

**XOR** is a bitwise operation. A common pattern in weak encryption:

```
encrypt: ciphertext = (plaintext - key) XOR key
decrypt: plaintext  = (ciphertext XOR key) + key
```

The key here is a **single number between 0 and 23**. Because the key space is tiny (only 24 possible values), you can just **try all of them** — this is called a brute force attack.

### What is brute forcing a small key?

When there are only a small number of possible keys, you simply try every single one and check which output makes sense (readable English = correct key).

---

## Solution

### Step 1 — Download and read the source code

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat gosar.go
# → Read the encryption logic: ciphertext = (plaintext - key) XOR key
# → Key is between 0 and 23
```

### Step 2 — Create the brute force script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

Paste this:

```python
# Read the ciphertext bytes from the attachment
ciphertext = [...]  # paste the encrypted bytes here

for key in range(24):  # try all keys 0 to 23
    plaintext = bytes([(b ^ key) + key for b in ciphertext])
    try:
        decoded = plaintext.decode('ascii')
        if 'ictf{' in decoded:
            print(f"Key {key}: {decoded}")
    except:
        pass
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → Key 23: ictf{23_days_till_Christmas!}
```

The correct key is **23** — and the flag even tells you! ✅

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Brute force all 24 possible keys | ⭐ Easy |

---

## Key Takeaways

- Always read the source code first — it tells you exactly how to reverse the encryption
- A key space of 0–23 is only **24 possibilities** — always brute force small key spaces
- The decryption order matters: **XOR first, then add** (reverse of encrypt which subtracts then XORs)
- The flag literally contains the key: `23_days_till_Christmas` — a fun hint!
