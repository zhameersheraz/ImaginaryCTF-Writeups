# Tree Encoder — ImaginaryCTF Writeup

**Challenge:** Tree Encoder  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 80 pts  
**Flag:** `ictf{S7r1nG5_a5_Tr3e5_4s_5tr1Ng5}`

---

## Description

> I wrote this really trashy encoder in rust. See if you can figure out what it does and decode the flag.

**Attachment:** Rust binary + encoded flag

---

## Background Knowledge (Read This First!)

### What is a Tree Encoding?

The program encodes a string as a **binary tree** structure. It takes successive bit pairs from the input and:
- Either encodes the remaining bits recursively across one side of the tree
- Or splits them across both sides

The result is a tree where the leaves contain the encoded characters.

### How do you decode it?

Once you understand the encoding by reading the Rust source or disassembling the binary:
1. Parse the tree structure
2. Walk the tree to collect the leaves in order
3. Reconstruct the bit pairs
4. Convert back to characters

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download and analyze the binary

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ file tree_encoder
# → ELF 64-bit, Rust binary
└─$ strings tree_encoder | head -30
# → Look for clues about the encoding structure
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{S7r1nG5_a5_Tr3e5_4s_5tr1Ng5}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `strings` / Ghidra | Understand the tree encoding from the Rust binary | ⭐⭐ Medium |
| Python 3 | Parse and decode the tree structure | ⭐⭐ Medium |

---

## Key Takeaways

- Rust binaries can be reversed — `strings` and Ghidra both work well on them
- Tree-based encodings are uncommon but follow a simple recursive structure once understood
- The flag title says it all: strings as trees as strings — encoding and decoding are inverses
