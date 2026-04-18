# eX0Rdinary challenge — ImaginaryCTF Writeup

**Challenge:** eX0Rdinary challenge  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{this_is_a_pretty_stereotyped_xor_chall_but_anyways_good_job_have_a_flag}`

---

## Description

> Most original 50 point crypto chall am I right chat???

**Attachment:** Base64 encoded ciphertext directly in the challenge:

```
FAoXEh0PHAEaLDYaLD4+LwIXEQANJiwHERcXChsNCRUBOycXHS08CwkNADM9FwErPg8XDhYYCiw4CAALOzUFDT03CRcTOj4+OQoNBho=
```

---

## Background Knowledge (Read This First!)

### What is XOR chaining?

**XOR chaining** (also called CBC-XOR or stream XOR) encrypts each byte using the previous ciphertext byte as part of the key:

```
ciphertext[i] = plaintext[i] XOR ciphertext[i-1]
```

This means if you know the first byte, you can recover all subsequent bytes. Since CTF flags start with `ictf{`, the first byte is always `i` — giving you the starting point.

### Why is it weak?

Because the first byte of the flag is known (`i` = 0x69 = `ord('i')`), you can set `ciphertext[0] = ord('i')` and then chain-decode all subsequent bytes.

---

## Solution

### Step 1 — Create the decode script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
import base64

encrypted = bytearray(base64.b64decode(
    'FAoXEh0PHAEaLDYaLD4+LwIXEQANJiwHERcXChsNCRUBOycXHS08CwkNADM9FwErPg8XDhYYCiw4CAALOzUFDT03CRcTOj4+OQoNBho='
))

# First byte is known: flag starts with 'i'
encrypted[0] = ord('i')

# XOR chain decode: each byte XORed with the previous
for i in range(1, len(encrypted)):
    encrypted[i] ^= encrypted[i - 1]

print(encrypted.decode())
```

### Step 2 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{this_is_a_pretty_stereotyped_xor_chall_but_anyways_good_job_have_a_flag}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Base64 decode + XOR chain decode | ⭐ Easy |

---

## Key Takeaways

- XOR chaining is trivially broken when the first byte is known (like `i` in `ictf{`)
- Known-plaintext attacks on XOR are always possible when you know part of the plaintext
- The title "eX0Rdinary" is literally telling you it is an XOR challenge
