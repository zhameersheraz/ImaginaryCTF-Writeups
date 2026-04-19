# db diving — ImaginaryCTF Writeup

**Challenge:** db diving  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{f4c70r1n6_15_h4rd_bu7_6006l1n6_15_345y}`

---

## Description

> I was rummaging through ictf dumpster B, as one does, and found this old RSA key. Help me decrypt it and we can share the flag 😃

**Attachment:** RSA parameters directly in the challenge:

```
n = 0x137dcc9dfd83f0da70c45056e3a6fd68a540b4e8a01c130bcd09da355e39d226436017ba39ee3018c543743da4f29e4ad08cb39bb1673eec1dae7cb61c71c71c71c71c71c71c71c71c71c71c71c71c71c71c71c71c71
e = 0x10001
c = 0x333e8e718d231ba19c9b34c3a62bf3123cbc943234f64b2ac199fdca0aea730268e8fbc00701c82f31083c6d9f3d7daf0ff7a8968058e0cd5410241a2838456547d832fd701f1114cf2749072155c4c8cfddd4f982e
```

---

## Background Knowledge (Read This First!)

### What is FactorDB?

**FactorDB** (`factordb.com`) is a public database of factored numbers. Researchers and enthusiasts have factored billions of numbers over the years and stored the results. If your RSA modulus `n` has already been factored by someone else, it is in the database!

### Why is this n Factorable?

Looking at the hex representation of `n`, you can see a repeating pattern: `...c71c71c71c71c71c71c71c71...`. This repetition suggests the number has special mathematical structure that makes it easier to factor.

---

## Solution

### Option A — FactorDB (fastest)

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://factordb.com/index.php?query=0x137dcc9d..."
# → FactorDB shows p and q directly
```

### Option B — dcode.fr RSA tool

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://www.dcode.fr/rsa-cipher"
# → Enter n, e, c → tool finds p, q automatically → decrypts flag
```

### Step 2 — Decrypt once you have p and q

```python
from Crypto.Util.number import long_to_bytes

n = 0x137dcc9dfd83f0da70c45056e3a6fd68a540b4e8a01c130bcd09da355e39d226436017ba39ee3018c543743da4f29e4ad08cb39bb1673eec1dae7cb61c71c71c71c71c71c71c71c71c71c71c71c71c71c71c71c71c71
e = 0x10001
c = 0x333e8e718d231ba19c9b34c3a62bf3123cbc943234f64b2ac199fdca0aea730268e8fbc00701c82f31083c6d9f3d7daf0ff7a8968058e0cd5410241a2838456547d832fd701f1114cf2749072155c4c8cfddd4f982e
p = ...  # from FactorDB
q = n // p

d    = pow(e, -1, (p-1)*(q-1))
flag = long_to_bytes(pow(c, d, n))
print(flag)
# → ictf{f4c70r1n6_15_h4rd_bu7_6006l1n6_15_345y}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| factordb.com | Look up the pre-factored modulus | ⭐ Easy |
| dcode.fr/rsa-cipher | All-in-one RSA decryption | ⭐ Easy |

---

## Key Takeaways

- Always check FactorDB first for RSA challenges — someone may have already factored it!
- Repeating hex patterns in `n` hint at mathematical structure that aids factoring
- "Factoring is hard but Googling is easy" — the flag says it all
