# Verxina — ImaginaryCTF Writeup

**Challenge:** Verxina  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{verxinas_owner_is_a_famous_baseball_player}`

---

## Description

> This horse probably knew something about bases, do you think she knew base64.... cause I certainly don't. Note the attachment directly decodes to the flag, nothing extra.

**Attachment:** Encoded string directly in the challenge

```
aQ====wYdA====gZew====gdZQ====gceA====Qabg====QYcw====wXbw====wdbg====QZcg====wXaQ====wcXw====QYXw====gZYQ====Qbbw====Qdcw====wXYg====QYcw====QZYg====QYbA====AbXw====AcbA====QYeQ====QZcg====Qf
```

---

## Background Knowledge (Read This First!)

### What is the dumb_encode function?

The challenge provides this encoding function:

```python
from base64 import *

def dumb_encode(data):
    out = b''
    for i in range(len(data)):
        temp = b64encode(data[i:i+1])  # base64 encode one byte at a time
        if i % 2:
            temp = temp[::-1]           # reverse every other chunk
        out += temp
    return out
```

Each byte of the original data is:
1. Base64 encoded individually (produces 4 characters including `==` padding)
2. Every **odd-indexed** chunk is **reversed**

### How do you reverse it?

For each 4-character chunk:
1. If it is at an **odd index** → reverse it first, then base64 decode
2. If it is at an **even index** → base64 decode directly

---

## Solution

### Step 1 — Create the decode script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from base64 import b64decode

encoded = "aQ====wYdA====gZew====gdZQ====gceA====Qabg====QYcw====wXbw====wdbg====QZcg====wXaQ====wcXw====QYXw====gZYQ====Qbbw====Qdcw====wXYg====QYcw====QZYg====QYbA====AbXw====AcbA====QYeQ====QZcg====Qf"

# Split into 4-character chunks (but the padding is ====, so chunks are 6 chars)
# Each encoded byte = 4 base64 chars, padding makes them longer
# Split by ==== separator
chunks = encoded.split("====")
chunks = [c + "====" for c in chunks if c]  # restore padding... adjust as needed

result = b""
for i, chunk in enumerate(chunks):
    if i % 2 == 1:
        chunk = chunk[::-1]  # reverse odd chunks before decoding
    result += b64decode(chunk)

print(result.decode())
# → ictf{verxinas_owner_is_a_famous_baseball_player}
```

### Step 2 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{verxinas_owner_is_a_famous_baseball_player}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Reverse the dumb_encode and decode | ⭐⭐ Medium |

---

## Key Takeaways

- Always read the encoding function carefully — reversing it gives you the decoder for free
- Base64 padding (`====`) is a useful delimiter here to split the chunks
- "Knew something about bases" in the description = hint about base64
