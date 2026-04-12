# simOTP — ImaginaryCTF Writeup

**Challenge:** simOTP  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{https://www.youtube.com/watch?v=CQXPc-O82Dc}`

---

## Description

> Basically the same as SEETF 2022-Neutrality, what could go wrong?

**Connection:** Server provided in attachment

---

## Background Knowledge (Read This First!)

### What is a One-Time Pad (OTP)?

A **One-Time Pad** is theoretically the most secure encryption method. It XORs plaintext with a truly random key of the same length. If used correctly (key used only once, truly random), it is **unbreakable**.

### What is "simOTP"?

**simOTP** ("simulated OTP") is a weak OTP implementation. The name says it all — it *simulates* a one-time pad but makes critical mistakes that break its security.

### What is SEETF 2022 — Neutrality?

The description references a previous CTF challenge. The vulnerability is the same: the server reuses key material or leaks enough information about the keystream that you can recover the key through multiple queries.

### What is the attack?

By sending carefully crafted plaintexts to the oracle and observing the ciphertexts, you can recover the keystream. Once you have the keystream, you can decrypt any ciphertext — including the flag.

> ⚠️ **Note:** The author requested that the full writeup be posted next month. The core idea is keystream recovery through oracle queries.

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc <server> <port>
# → Server accepts plaintext, returns ciphertext
```

### Step 2 — Probe the oracle

Send known plaintexts (e.g., all zeros) to recover the keystream:

```python
# If plaintext = 0x00...00, then ciphertext = keystream XOR 0x00...00 = keystream
known_plain = b'
