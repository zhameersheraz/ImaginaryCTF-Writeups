# Do Deuce — ImaginaryCTF Writeup

**Challenge:** Do Deuce  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{couldve_just_been_an_equinox_victim_but_bro_just_keeps_winning_now_that_his_rivals_retired}`

---

## Description

> The eclair slayer... shame he pulled out of this year's japan cup

**Attachment:** Encrypted file (block cipher)

---

## Background Knowledge (Read This First!)

### What is a Block Cipher?

A **block cipher** encrypts data in fixed-size chunks (blocks). Common block sizes are 16 or 32 bytes. The same key is used for each block, but the exact behavior depends on the mode.

### What is "brute force in reverse order"?

In this challenge, each block of the flag is encrypted independently with a key. The trick is:

1. Work **backwards** — start from the last block
2. Try all possible keys for that block
3. You know you have the **right key** when the decrypted block produces **readable English text**
4. Move to the previous block and repeat

Since the flag is readable English (and you know the format `ictf{...}`), a correctly decrypted block is obvious.

### Why go in reverse?

The encryption may chain blocks, so decrypting the last block first (where you have no dependencies) is the cleanest starting point.

---

## Solution

### Step 1 — Download and inspect the file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ xxd encrypted.bin | head -20
# → Inspect the structure, identify block size
```

### Step 2 — Create the brute force script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

Paste this:

```python
data = open('encrypted.bin', 'rb').read()

BLOCK_SIZE = 16  # adjust if needed
blocks = [data[i:i+BLOCK_SIZE] for i in range(0, len(data), BLOCK_SIZE)]

flag = ['?'] * len(blocks)

# Go through blocks in reverse order
for block_idx in range(len(blocks) - 1, -1, -1):
    block = blocks[block_idx]
    for key in range(256):  # brute force single-byte key
        decrypted = bytes([b ^ key for b in block])
        try:
            text = decrypted.decode('ascii')
            # Check if it looks like readable flag text
            if all(c.isprintable() for c in text):
                print(f"Block {block_idx}, key {key}: {text}")
        except:
            pass
```

### Step 3 — Run it and assemble the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → Block outputs printed — pick the readable English ones
# → Assemble in order → ictf{couldve_just_been_an_equinox_victim_...}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Brute force each block key in reverse order | ⭐⭐ Medium |

---

## Key Takeaways

- When a block cipher uses a small key per block, brute force is always viable
- Work **in reverse order** when blocks may have dependencies on previous blocks
- Readable English plaintext is a self-validating oracle — if it makes sense, the key is right
- The flag itself being plain English makes it easy to confirm correct decryption
