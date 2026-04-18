# S-Box — ImaginaryCTF Writeup

**Challenge:** S-Box  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{why_us1ng_tr1pIe_x0r_1n_Crypt0syst3m}`

---

## Description

> RC4 with totally random S-Box! No one can decrypt it...

**Connection:** `nc 8.138.23.35 8000` (backup: `nc 155.248.210.243 42138`)

---

## Background Knowledge (Read This First!)

### What is RC4?

**RC4** is a stream cipher that uses a key-scheduling algorithm (KSA) to initialize an S-Box (a permutation of 0-255), then generates a keystream using the pseudo-random generation algorithm (PRGA). The keystream is XOR-ed with the plaintext to encrypt.

### What is a "totally random S-Box"?

The challenge claims the S-Box is random — meaning the key scheduling is replaced with a random permutation. This breaks the standard RC4 structure.

### What is the Attack?

The S-Box is random, but it is **fixed for the session**. By sending a very long known plaintext (like 200,000 `a` characters), the server encrypts it with the keystream. Since you know the plaintext, XOR-ing gives you the full keystream. Then you can decrypt the flag!

```bash
python -c 'print("a"*200000)' | nc [ip] [port]
```

---

## Solution

### Step 1 — Send a massive known plaintext to recover the keystream

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c 'print("a"*200000)' | nc 155.248.210.243 42138 > output.bin
```

### Step 2 — Extract the keystream

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
known_plain = b'a' * 200000
ciphertext  = open('output.bin', 'rb').read()

# XOR known plaintext against ciphertext to get keystream
keystream = bytes([c ^ p for c, p in zip(ciphertext, known_plain)])

# Use keystream to decrypt the flag (sent at the beginning or end)
# flag_cipher = first N bytes of ciphertext where flag is
flag = bytes([c ^ k for c, k in zip(flag_cipher, keystream)])
print(flag)
# → ictf{why_us1ng_tr1pIe_x0r_1n_Crypt0syst3m}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{why_us1ng_tr1pIe_x0r_1n_Crypt0syst3m}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Known-plaintext attack to recover keystream | ⭐⭐ Medium |
| `nc` | Send massive plaintext and receive keystream | ⭐ Easy |

---

## Key Takeaways

- A fixed S-Box (random or not) is vulnerable to chosen-plaintext attacks
- Sending a long known plaintext reveals the entire keystream via XOR
- Once you have the keystream, decrypting any ciphertext is trivial
