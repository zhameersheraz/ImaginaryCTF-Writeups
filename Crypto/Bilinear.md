# Bilinear — ImaginaryCTF Writeup

**Challenge:** Bilinear  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{4dD1ng+m0re_op3raTIon5~1sNT_alwAyS_stR0ngEr}`

---

## Description

> Xor is linear, addition is too... does combining them make it bilinear???

**Attachment:** Ciphertext + encryption source

---

## Background Knowledge (Read This First!)

### What does "linear" mean in cryptography?

An operation is **linear** if it satisfies:

```
f(a XOR b) = f(a) XOR f(b)
```

Both XOR and addition over integers are linear operations. This is a fatal weakness — linear operations leak information about the key when you observe multiple ciphertexts.

### What does "bilinear" mean here?

The challenge combines XOR and addition hoping that their interaction creates a stronger (bilinear) cipher. But it does NOT — the combined cipher is still vulnerable to linear algebraic attacks because the linearity of each operation can be exploited independently.

### What is the attack?

With multiple plaintext-ciphertext pairs, you can set up a system of linear equations and solve for the key. The solve script is provided in the challenge attachments.

---

## Solution

### Step 1 — Download and inspect the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat bilinear.py
# → Read the encryption: combines XOR and addition with a key
```

### Step 2 — Use the provided solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
# → Paste the solve script from the attachment
```

The key insight: both XOR and addition are linear. Given enough ciphertext, solve the linear system to recover the key.

### Step 3 — Run and get the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{4dD1ng+m0re_op3raTIon5~1sNT_alwAyS_stR0ngEr}
```

*(Full solve script provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Linear algebraic attack to recover key | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Combining linear operations does NOT create a stronger cipher — linearity is inherited
- XOR and addition are both linear — their combination is still exploitable
- The flag says it all: adding more operations is NOT always stronger
