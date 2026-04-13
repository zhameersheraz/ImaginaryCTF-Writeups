# Sea Bird — ImaginaryCTF Writeup

**Challenge:** Sea Bird  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{french_horses_deserve_more_love_too}`

---

## Description

> can combining weak ciphers make a stronger one, well sometimes...

**Attachment:** Ciphertext file

---

## Background Knowledge (Read This First!)

### What is a Monoalphabetic Cipher?

A **monoalphabetic substitution cipher** replaces each letter of the alphabet with a different fixed letter. For example:

```
A → Q
B → X
C → M
...
```

Every `A` in the plaintext always becomes `Q` in the ciphertext. This is its fatal weakness — the **letter frequency** is preserved.

### Why is frequency analysis so powerful?

In English text, some letters appear far more often than others:

```
Most common: E, T, A, O, I, N, S, H, R
Least common: Q, X, Z, J, K
```

By counting which ciphertext letters appear most often and matching them to the most common English letters, you can crack the substitution without knowing the key.

### Does combining weak ciphers make it stronger?

The challenge asks this question. The answer here is **no** — combining monoalphabetic ciphers just produces another monoalphabetic cipher. The combined result is still crackable by frequency analysis.

---

## Solution

### Step 1 — Download and inspect the ciphertext

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat ciphertext.txt
# → Garbled text — a substitution cipher
```

### Step 2 — Use an online substitution cipher solver

Go to `https://www.dcode.fr/monoalphabetic-substitution` and paste the ciphertext. Click **Automatic Decrypt** — it uses frequency analysis to crack it automatically.

Or use `quipqiup.com` — another excellent monoalphabetic solver:

```
https://quipqiup.com
```

Paste the ciphertext → click Solve → read the flag.

### Step 3 — Read the flag

```
ictf{french_horses_deserve_more_love_too}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| dcode.fr or quipqiup.com | Automatic frequency analysis and monoalphabetic decryption | ⭐ Easy |

---

## Key Takeaways

- Combining weak monoalphabetic ciphers does **not** make a stronger cipher — the result is still monoalphabetic
- **Frequency analysis** breaks any monoalphabetic cipher given enough ciphertext
- dcode.fr and quipqiup are essential bookmarks for CTF crypto challenges
- The description "well sometimes..." hints that in this case, combining them did NOT help
