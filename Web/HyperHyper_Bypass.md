# HyperHyper Bypass — ImaginaryCTF Writeup

**Challenge:** HyperHyper Bypass  
**Category:** Web  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{weeks_of_computing_or_osint_still_only_3_clicks_though}`

---

## Description

> Press button (many many many times) → Get flag

---

## Background Knowledge (Read This First!)

### Why can't we just forge the cookie like before?

This challenge is the sequel to `hyper bypass`. This time the server **cryptographically signs** the click count using **Fiat-Shamir**. You cannot just change the number in the cookie anymore — the server checks the proof is mathematically valid.

### What is Fiat-Shamir?

**Fiat-Shamir** is a zero-knowledge proof protocol. The **non-interactive (Fiat-Shamir transform)** version relies on the hash function behaving like a random oracle. If that hash function is weak and you can control its output, you can **forge** the proof entirely.

### What is the weakness here?

The server uses **MD5** inside its Fiat-Shamir scheme. MD5 is **cryptographically broken** — specific hash collisions and pre-images have been pre-computed and are publicly available.

This challenge only requires **3 clicks**, so you need to forge a valid proof for 3 interactions.

### Where do you get the pre-computed hashes?

The website `https://beneri.se/hashgame/index.php` has already done weeks of computing to find the minimal MD5 hash values needed. You simply look them up — this is why the flag mentions **OSINT**!

---

## Solution

### Step 1 — Understand the cookie structure

Connect to `http://155.248.210.243:42128/` and press the button once. Inspect the cookie in your browser DevTools (**F12 → Application → Cookies**). It contains a Fiat-Shamir proof tied to a click count, not just a plain number.

### Step 2 — Look up pre-computed MD5 hashes

Visit `https://beneri.se/hashgame/index.php` and find the minimal MD5 hash values that satisfy the server's Fiat-Shamir verification for 3 clicks. Someone else already spent weeks computing these — you just look them up.

### Step 3 — Create the exploit script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano exploit.py
```

Paste this:

```python
import requests

# Build the forged session cookie using pre-computed hashes from beneri.se
forged_cookie = "<value built from pre-computed minimal MD5 hashes>"

r = requests.get(
    'http://155.248.210.243:42128/flag',
    cookies={'session': forged_cookie}
)
print(r.text)
```

### Step 4 — Run it and get the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 exploit.py
# → ictf{weeks_of_computing_or_osint_still_only_3_clicks_though}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Browser DevTools | Inspect the cryptographic cookie structure | ⭐ Easy |
| beneri.se/hashgame | Pre-computed minimal MD5 hashes for Fiat-Shamir forgery | ⭐⭐ Medium |
| Python `requests` | Send the forged cookie to the server | ⭐ Easy |

---

## Key Takeaways

- **MD5 is broken** — never use it inside cryptographic proof schemes
- Fiat-Shamir proofs are only as secure as the hash function used inside them
- Sometimes the best CTF skill is **OSINT** — knowing that someone else already computed the hard part
- Even a "3 click" limit with a cryptographic proof can be broken once the hash is weak
- The flag says it all: *"weeks of computing or OSINT — still only 3 clicks though"*
