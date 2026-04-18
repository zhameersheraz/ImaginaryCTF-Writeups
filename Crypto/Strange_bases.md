# Strange bases — ImaginaryCTF Writeup

**Challenge:** Strange bases  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{less_common_90s_base}`

---

## Description

> base64 isn't the only text encoding scheme...

**Attachment:** Hex values directly in the challenge:

```
0x47,0x27,0x48,0x72,0x58,0x2d,0x63,0x28,0x40,0x5e,0x74,0x2e,0x6f,0x39,0x4d,0x46,
0x48,0x4d,0x76,0x5a,0x4d,0x76,0x57,0x52,0x34,0x7b,0x68,0x33,0x42,0x5d,0x31,0x4a,0x25,0x51
```

---

## Background Knowledge (Read This First!)

### What is Base92?

**Base92** is an encoding scheme that uses 92 printable ASCII characters (from `!` to `~`, excluding `"` and `\`) to represent binary data. It is more space-efficient than Base64 but far less commonly used — hence "strange bases" and "90s base" in the flag.

### What is the Decode Process?

1. Convert each hex value to its ASCII character
2. Treat the resulting string as Base92 encoded data
3. Decode it → get the flag

---

## Solution

### Step 1 — Convert hex to ASCII

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c "
vals = [0x47,0x27,0x48,0x72,0x58,0x2d,0x63,0x28,0x40,0x5e,0x74,0x2e,0x6f,0x39,
        0x4d,0x46,0x48,0x4d,0x76,0x5a,0x4d,0x76,0x57,0x52,0x34,0x7b,0x68,0x33,
        0x42,0x5d,0x31,0x4a,0x25,0x51]
print(''.join(chr(v) for v in vals))
"
# → G'HrX-c(@^t.o9MFHMvZMvWR4{h3B]1J%Q
```

### Step 2 — Decode as Base92

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
# Install base92 if needed: pip install base92
import base92

encoded = "G'HrX-c(@^t.o9MFHMvZMvWR4{h3B]1J%Q"
print(base92.decode(encoded))
# → ictf{less_common_90s_base}
```

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{less_common_90s_base}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Hex to ASCII conversion | ⭐ Easy |
| `base92` library | Decode Base92 encoded string | ⭐ Easy |

---

## Key Takeaways

- Base64 is the most common encoding but many others exist — Base92, Base85, Base58, etc.
- Always try different base encodings when you see a "strange" looking string
- The description "base64 isn't the only one" is a direct hint to try other bases
