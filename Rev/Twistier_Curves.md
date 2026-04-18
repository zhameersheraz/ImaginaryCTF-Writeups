# Twistier Curves — ImaginaryCTF Writeup

**Challenge:** Twistier Curves  
**Category:** Rev  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{Maze_in_O(V)_time}`

---

## Description

> Even the source code is a maze, they really should re-ban the author from C++

**Attachment:** C++ source + encoded image

---

## Background Knowledge (Read This First!)

### What is the Encoding?

The program:
1. Generates a **random maze** seeded from 4 32-bit integers
2. Encrypts the flag using **linear transforms** corresponding to the shortest path between two corners of the maze
3. Encodes both the **encrypted flag** and the **4 seed values** into the image using LSB (Least Significant Bit) steganography

### What is LSB Steganography?

**LSB steganography** hides data by replacing the least significant bit of each pixel value. The change is invisible to the human eye, but the hidden bits can be extracted by reading the LSBs in order.

### What is the Attack?

1. Extract the 4 seed values from the image LSBs
2. Re-generate the same maze using those seeds
3. Find the shortest path between the two corners
4. Apply the inverse linear transform to decrypt the flag

*(The full source is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Extract LSBs from the image

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c "
from PIL import Image
img = Image.open('output.png')
pixels = list(img.getdata())
bits = [p & 1 for p in pixels]
# First bits = seed values, remaining = encrypted flag
"
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{Maze_in_O(V)_time}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + PIL | Extract LSB-encoded seeds from image | ⭐⭐ Medium |
| C++ / Python | Re-generate maze and decrypt via inverse transform | ⭐⭐⭐ Hard |

---

## Key Takeaways

- LSB steganography hides data invisibly in pixel values — always check image LSBs in steg/rev challenges
- Maze generation from a known seed is deterministic and reproducible
- "Maze in O(V) time" — the shortest path algorithm runs in O(vertices) time
