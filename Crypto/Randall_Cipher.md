# Randall Cipher — ImaginaryCTF Writeup

**Challenge:** Randall Cipher  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 55 pts  
**Flag:** `ictf{THIS_CIPHER_IS_VERY_LOUD_AND_INSECURE}`

---

## Description

> Are you all caught up on all their comics... note: the correct flag contents are in all uppercase

**Attachment:** Encoded string:
```
ictf{ĀA̰ẢÃ_A̧ẢA̯A̰ÁȂ_ẢÃ_ÀÁȂA̦_ĂÅÄA̱_AÂA̱_ẢÂÃÁA̧ÄȂÁ}
```

---

## Background Knowledge (Read This First!)

### What is the Scream Cipher?

The **Scream Cipher** is a cipher described in **XKCD comic #3054**. It encodes uppercase letters using various Unicode diacritics on the letter `A` — making it look like someone is screaming in increasingly distressed text.

Each letter maps to a specific Unicode modification of `A`:
- `A` = plain A
- `B` = À (A with grave)
- `C` = Á (A with acute)
- And so on for each letter of the alphabet

### Why "Are you all caught up on all their comics"?

The description is a direct hint to read XKCD — the cipher is defined there!

---

## Solution

### Step 1 — Read XKCD #3054

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://xkcd.com/3054/"
# → Find the Scream Cipher encoding table
```

### Step 2 — Decode each character

Map each Unicode-modified A back to its corresponding letter using the XKCD cipher table:

```python
# Each diacritic variant of A maps to a letter
# ĀA̰ẢÃ → THIS
# A̧ẢA̯A̰ÁȂ → CIPHER
# etc.
```

### Step 3 — Read the flag

```
ictf{THIS_CIPHER_IS_VERY_LOUD_AND_INSECURE}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| XKCD #3054 | Find the Scream Cipher decoding table | ⭐ Easy |
| Python 3 (optional) | Automate decoding all characters | ⭐ Easy |

---

## Key Takeaways

- The description "are you all caught up on their comics" = go read XKCD
- The Scream Cipher encodes letters as Unicode diacritics on the letter A
- Always Google cipher names when you see unfamiliar encodings
