# Sweet and Sour Sauce — ImaginaryCTF Writeup

**Challenge:** Sweet and Sour Sauce  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{is_this_what_they_mean_by_an_unbalanced_oil_and_vinegar_scheme}`

---

## Description

> Idk anything about multivariate cryptography, that won't stop me from making a challenge tho... key length is 15

**Attachment:** Ciphertext

---

## Background Knowledge (Read This First!)

### What is a Vigenère Cipher?

A **Vigenère cipher** encrypts text using a repeating keyword. Each letter is shifted by the corresponding letter in the key. With a known key length, it is trivially crackable using frequency analysis.

### What is Oil-and-Vinegar?

**Unbalanced Oil and Vinegar (UOV)** is a real post-quantum signature scheme. The challenge title jokes about this — but the actual cipher is just Vigenère, not UOV at all!

### Why is Key Length 15 a Big Hint?

With a known key length of 15, you can split the ciphertext into 15 groups (one per key position) and crack each group independently as a simple Caesar cipher using frequency analysis.

---

## Solution

### Step 1 — Download and inspect the ciphertext

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat sweet_and_sour.txt
# → Vigenère ciphertext
```

### Step 2 — Crack it with dcode.fr (known key length = 15)

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://www.dcode.fr/vigenere-cipher"
```

Enter the ciphertext, set key length to **15**, click **Decrypt** → the correct key is:

```
BALSAMICVINEGAR
```

### Step 3 — Read the flag

```
ictf{is_this_what_they_mean_by_an_unbalanced_oil_and_vinegar_scheme}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| dcode.fr/vigenere-cipher | Vigenère decrypt with known key length 15 | ⭐ Easy |

---

## Key Takeaways

- Known key length reduces Vigenère to 15 independent Caesar ciphers — trivially crackable
- The key `BALSAMICVINEGAR` is a pun on "oil and vinegar" (the post-quantum scheme)
- The description trolling about "multivariate cryptography" is just flavor text — it is plain Vigenère
