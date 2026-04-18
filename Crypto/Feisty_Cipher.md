# Feisty Cipher — ImaginaryCTF Writeup

**Challenge:** Feisty Cipher  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{using_1_k3y_1n_f31573l_lol}`

---

## Description

> My key generator is broken but AES is so secure it probably doesn't matter.... right?

**Connection:** `nc 155.248.210.243 42191`

---

## Background Knowledge (Read This First!)

### What is a Feistel Cipher?

A **Feistel cipher** splits the plaintext into two halves (L, R) and applies multiple rounds of a function using subkeys. AES and DES are both Feistel-based.

### What is the Vulnerability Here?

Normally, each round uses a **different subkey**. In this implementation, the key scheduling is broken — **every round uses the same key**. This creates a critical symmetry.

### How Does Using One Key Break It?

When all subkeys are identical, you can turn the **encryption oracle into a decryption oracle** by simply swapping the two halves of the output:

```
encrypt(A || B) = C || D
→ encrypt(D || C) = B || A  ← this is the decryption of the original input!
```

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42191
```

### Step 2 — Use this script to turn encrypt into decrypt

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from Crypto.Util.number import long_to_bytes, bytes_to_long

# Step 1: Ask server to encrypt the flag → get flag_result
A = long_to_bytes(int(input("print flag result? ")))

# Step 2: Swap the two 16-byte halves
A = A[16:] + A[:16]
print("Enter this into encrypt message:", bytes_to_long(A))

# Step 3: Encrypt the swapped value → server decrypts the original flag
B = long_to_bytes(int(input("encrypt message result? ")))

# Step 4: Swap again to get original plaintext
print(B[16:] + B[:16])
# → ictf{using_1_k3y_1n_f31573l_lol}
```

### Step 3 — Follow the prompts

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → Follow instructions → enter values from server → get flag
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Swap halves to turn encrypt into decrypt | ⭐ Easy |
| `nc` | Connect to the challenge server | ⭐ Easy |

---

## Key Takeaways

- Feistel ciphers with identical round keys lose their one-wayness completely
- Swapping the output halves and re-encrypting effectively decrypts — a trivial chosen-ciphertext attack
- "AES is so secure it probably doesn't matter" — it matters a LOT when the key schedule is broken
