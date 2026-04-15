# Sakura Star O — ImaginaryCTF Writeup

**Challenge:** Sakura Star O  
**Category:** Rev  
**Difficulty:** Easy-Medium  
**Points:** 55 pts  
**Flag:** `ictf{sakura_star_O_might_be_the_single_greatest_tragedy_in_jra_history..._anyways_where_is_he_cygames}`

---

## Description

> no clever pun this time sadly I just think this horse deserves more recognition than he gets

**Attachment:** Binary with Huffman code implementation

---

## Background Knowledge (Read This First!)

### What is Huffman Coding?

**Huffman coding** is a lossless data compression algorithm. It assigns shorter bit sequences to more frequent characters and longer sequences to less frequent ones. It is used in ZIP files, JPEG images, and many other formats.

### How Does Huffman Decoding Work?

1. Build (or recover) the **Huffman tree** from the frequency table
2. Read the encoded bits one by one
3. Traverse the tree: `0` = go left, `1` = go right
4. When you reach a leaf node → that is the decoded character
5. Return to root and repeat

### How do you get the Huffman tree?

By reversing the binary, you can find the frequency table or the encoded tree structure stored in the binary. Once you have the tree, decoding is straightforward.

---

## Solution

### Step 1 — Reverse the binary to get the Huffman tree/frequencies

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ strings sakura_star_o
└─$ objdump -d sakura_star_o | less
# → Find the frequency table or pre-built tree
```

### Step 2 — Implement the Huffman decoder

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
import heapq

# Reconstruct Huffman tree from recovered frequencies
freq = {'a': 5, 'b': 2, ...}  # recovered from binary

heap = [[weight, [char, ""]] for char, weight in freq.items()]
heapq.heapify(heap)

while len(heap) > 1:
    lo = heapq.heappop(heap)
    hi = heapq.heappop(heap)
    for pair in lo[1:]:
        pair[1] = '0' + pair[1]
    for pair in hi[1:]:
        pair[1] = '1' + pair[1]
    heapq.heappush(heap, [lo[0] + hi[0]] + lo[1:] + hi[1:])

codes = dict(sorted(heapq.heappop(heap)[1:], key=lambda p: (len(p[-1]), p)))
decode_map = {v: k for k, v in codes.items()}

# Decode the bitstream
encoded_bits = "..."  # from binary output
decoded = ""
current = ""
for bit in encoded_bits:
    current += bit
    if current in decode_map:
        decoded += decode_map[current]
        current = ""

print(decoded)
# → ictf{sakura_star_O_might_be_the_single_greatest_tragedy_in_jra_history..._anyways_where_is_he_cygames}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{sakura_star_O_might_be_the_single_greatest_tragedy_in_jra_history..._anyways_where_is_he_cygames}
```

*(Full source provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `objdump` / `strings` | Recover Huffman tree/frequencies from binary | ⭐⭐ Medium |
| Python 3 | Rebuild Huffman tree and decode bitstream | ⭐⭐ Medium |

---

## Key Takeaways

- Huffman coding is one of the most common compression schemes — learn how to decode it
- The frequency table is always stored somewhere — find it in the binary via strings or disassembly
- Decoding is just tree traversal: `0` = left, `1` = right, leaf = character
- The description is genuinely sad — Sakura Star O is a legendary horse that deserves more love
