# Auto-incorrect — ImaginaryCTF Writeup

**Challenge:** Auto-incorrect  
**Category:** Steg  
**Difficulty:** Medium  
**Points:** 50 pts  
**Flag:** `ictf{50m3h0w_n07_7h3_w0r57_4u70c0rr3c7_1v3_u53d}`

---

## Description

> I'm trying to preserve this important piece of internet history but my autocorrect keeps messing with it 😠

**Attachment:** Text file riddled with spelling mistakes + solve script

---

## Background Knowledge (Read This First!)

### What is Steganography in Text?

**Text steganography** hides data inside ordinary-looking text. Unlike image steganography, the carrier is plain text. Common methods include:
- Hiding bits in whitespace (spaces vs tabs)
- Using synonyms to encode bits
- Introducing deliberate spelling mistakes at specific positions

### What is the Pattern Here?

The spelling mistakes in the text are **not random** — they occur at **multiples of position 23**. This is the hidden channel.

At each position that is a multiple of 23, check if the word is misspelled:
- Misspelled → bit = `1`
- Correct → bit = `0`

Collecting all these bits gives a binary string → convert to ASCII → flag.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the text file and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O text.txt
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → Takes LSB of every 23rd position's misspelling indicator
# → ictf{50m3h0w_n07_7h3_w0r57_4u70c0rr3c7_1v3_u53d}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Extract LSB of every 23rd word's spelling status | ⭐⭐ Medium |
| Spell checker | Identify which positions have spelling mistakes | ⭐⭐ Medium |

---

## Key Takeaways

- Text steganography hides bits in structured mistakes — position 23 is the hidden period
- Always look for patterns in "random" errors — the period (23 here) is the key
- LSB extraction at fixed intervals is a classic steganography technique
- The description says "autocorrect keeps messing with it" — the mistakes are intentional!
