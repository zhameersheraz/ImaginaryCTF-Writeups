# Vampire Cipher — ImaginaryCTF Writeup

**Challenge:** Vampire Cipher  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{POLYGRAMICCIPHERWITHVERYTINYKEYSPACECANBEBRUTEFORCEDALSOMORBIUSCIPHERLOL}`

---

## Description

> You'll never find my SECRETKEY... note: flag contents are all uppercase

**Attachment:** Ciphertext number string:
```
74218765313827428312124874216651265711212874571725564288741634254287126586654212416754712178817643564211612561713834356471752574217531775565613827
```

---

## Background Knowledge (Read This First!)

### What is the Morbit Cipher?

The **Morbit cipher** is a polygrammic substitution cipher. It encodes pairs of letters using a keyword that generates a substitution table. Despite sounding complex, the keyspace is tiny — there are only a small number of valid keys, making brute force trivial.

### What is "SECRETKEY"?

The description taunts you with `SECRETKEY` — but as the flag itself says, the cipher has such a tiny keyspace that you do NOT even need the key. You can brute force all possible keys in milliseconds.

### What is Polygrammic?

**Polygrammic** means the cipher encodes multiple characters at once (pairs, in this case). Unlike monoalphabetic ciphers (one letter → one letter), polygrammic ciphers encode letter pairs → encoded pairs.

---

## Solution

### Step 1 — Use dcode.fr to decode

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://www.dcode.fr/morbit-cipher"
# → Paste the ciphertext
# → Try keyword SECRETKEY first, or use automatic brute force
```

### Step 2 — Read the flag

```
ictf{POLYGRAMICCIPHERWITHVERYTINYKEYSPACECANBEBRUTEFORCEDALSOMORBIUSCIPHERLOL}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| dcode.fr/morbit-cipher | Decode the Morbit cipher | ⭐ Easy |

---

## Key Takeaways

- The Morbit cipher has a tiny keyspace — always brute force when keyspace is small
- Polygrammic ciphers encode letter pairs, not single letters — don't confuse with monoalphabetic
- "You'll never find my SECRETKEY" is a misdirection — you don't even need it!
