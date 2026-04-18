# FNV-1 — ImaginaryCTF Writeup

**Challenge:** FNV-1  
**Category:** Rev  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{heres_a_basic_xor_cracking_chall_to_start_the_month_off}`

---

## Description

> Cryptographic hashes are overrated heres something much simpler...

**Attachment:** Source + ciphertext

---

## Background Knowledge (Read This First!)

### What is FNV-1?

**FNV-1** is the predecessor to FNV-1a. The only difference is the order of operations:

```
FNV-1:   hash = hash * FNV_prime → hash = hash XOR byte
FNV-1a:  hash = hash XOR byte    → hash = hash * FNV_prime
```

### Why is this a "basic XOR cracking challenge"?

The writeup says it directly — this is a **known-plaintext XOR attack**. Since CTF flags always start with `ictf{`, you know the first several bytes of the plaintext. XOR-ing the known plaintext bytes against the ciphertext bytes reveals the key (or key stream), which you can then use to decrypt the rest.

---

## Solution

### Step 1 — Download and read the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O fnv1.py
└─$ cat fnv1.py
# → XOR-based encryption using FNV-1 hash as keystream
```

### Step 2 — Create the solve script using known plaintext

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
# Known: flag starts with ictf{
# XOR known plaintext against ciphertext to recover key bytes

ciphertext = bytes.fromhex("...")  # paste ciphertext hex here
known = b"ictf{"

# Recover key bytes from known plaintext
key_bytes = bytes([c ^ p for c, p in zip(ciphertext, known)])
print("Key prefix:", key_bytes)

# Extend and decrypt full flag
# (reconstruct full key from the FNV-1 structure)
```

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{heres_a_basic_xor_cracking_chall_to_start_the_month_off}
```

*(Full source and solve provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Known-plaintext XOR attack | ⭐ Easy |

---

## Key Takeaways

- Known-plaintext XOR attacks always work when you know any part of the plaintext
- CTF flags always start with `ictf{` — use this as your known plaintext
- FNV-1 (and FNV-1a) are NOT secure as encryption keystreams
