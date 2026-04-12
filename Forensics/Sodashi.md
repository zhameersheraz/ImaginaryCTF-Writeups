# Sodashi — ImaginaryCTF Writeup

**Challenge:** Sodashi  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{this_horse_is_kinda_majestic_frfr}`

---

## Description

> The owner of this horse is quite protective of Sodashi's likeness, to respect their wishes we've refrained from even showing her. Hint: this challenge requires some OSINT to solve.

**Attachment:** A blurred image with text overlaid

---

## Background Knowledge (Read This First!)

### What is OSINT?

**OSINT (Open Source Intelligence)** is the practice of finding information from publicly available sources — websites, social media, image search, etc. In CTF challenges, OSINT means you need to find something on the internet to solve the challenge.

### What is image blur and why is it reversible here?

The challenge applies a **known blur filter** to an image. Because you know:
1. The original image source (found via OSINT)
2. The exact blur parameters used
3. The font used in the flag overlay

...you can **reverse-engineer the flag character by character** by comparing the blurred flag against a blurred version of each possible character.

### What is a character-by-character oracle attack?

For each position in the flag:
1. Render each possible character (`a-z`, `0-9`, `_`, `{`, `}`) using the known font
2. Apply the same blur
3. Compare with the actual blurred flag at that position
4. The character with the **minimum difference** is the correct one

This is called a **similarity oracle** — the correct character minimizes the pixel difference.

---

## Solution

### Step 1 — Find the original image via OSINT

Search for "Sodashi horse" on Google Images. The original unblurred image source is:

```
https://idolhorse.com/wp-content/uploads/2024/07/SodashiWW.jpg
```

Download it:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget https://idolhorse.com/wp-content/uploads/2024/07/SodashiWW.jpg
```

### Step 2 — Identify the blur parameters and font

Inspect the attachment carefully. The blur type and parameters are deterministic — the same blur applied to the same image produces the same result. Identify the font from the flag overlay (e.g., using `exiftool` or by visual inspection).

### Step 3 — Create the character-by-character decode script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from PIL import Image, ImageFilter, ImageDraw, ImageFont
import numpy as np

blurred_flag = Image.open('challenge.png').convert('RGB')
charset = 'abcdefghijklmnopqrstuvwxyz0123456789_{}'

flag = 'ictf{'
# For each unknown character position:
for pos in range(known_prefix_len, max_flag_len):
    best_char  = ''
    best_score = float('inf')
    for c in charset:
        # Render candidate character at this position
        candidate = render_char(flag + c, font, blur_params)
        # Compare with blurred flag at this region
        diff = np.abs(np.array(candidate) - np.array(blurred_flag_crop)).mean()
        if diff < best_score:
            best_score = best_char
            best_char  = c
    flag += best_char
    print(f"Found so far: {flag}")
    if flag.endswith('}'):
        break

print("Flag:", flag)
# → ictf{this_horse_is_kinda_majestic_frfr}
```

### Step 4 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{this_horse_is_kinda_majestic_frfr}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Google Images / OSINT | Find the original unblurred image source | ⭐⭐ Medium |
| Python 3 (`Pillow`, `numpy`) | Character-by-character similarity comparison | ⭐⭐⭐ Hard |
| `wget` | Download the original image | ⭐ Easy |

---

## Key Takeaways

- **OSINT first** — if a challenge mentions finding something, search for it before touching any code
- Known blur parameters + known font = the blur is reversible character by character
- A **similarity oracle** (minimize pixel difference) is a powerful technique for recovering obscured text
- When the original image is known, blurring is NOT a secure way to hide overlay text
