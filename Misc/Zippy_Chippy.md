# Zippy Chippy — ImaginaryCTF Writeup

**Challenge:** Zippy Chippy  
**Category:** Misc  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{5_minute_chal_that_takes_5_seconds_to_solve}`

---

## Description

> The evil and intimidating loser horse...

**Attachment:** A password-protected ZIP file

---

## Background Knowledge (Read This First!)

### What is a Password-Protected ZIP?

A ZIP file can be encrypted with a password. Without the password you cannot open it. However, the **password hash** is stored inside the ZIP file itself — and that hash can be cracked.

### What is Hash Cracking?

Tools like **hashcat** and **John the Ripper** can extract the hash from an encrypted ZIP and then try millions of common passwords against it until they find a match.

### What is a Wordlist?

A **wordlist** (like `rockyou.txt`) is a huge list of commonly used passwords. Since most people use weak passwords, cracking tools can find the password in seconds by trying every entry in the wordlist.

---

## Solution

### Step 1 — Extract the hash from the ZIP using zip2john

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ zip2john zippy_chippy.zip > hash.txt
```

### Step 2 — Crack the hash with John the Ripper

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
# → Found password in seconds
```

Or with hashcat:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ hashcat -m 17225 hash.txt /usr/share/wordlists/rockyou.txt
```

### Step 3 — Unzip with the cracked password

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ unzip -P <cracked_password> zippy_chippy.zip
└─$ cat flag.txt
# → ictf{5_minute_chal_that_takes_5_seconds_to_solve}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `zip2john` | Extract the hash from the encrypted ZIP | ⭐ Easy |
| John the Ripper | Crack the hash using rockyou.txt wordlist | ⭐ Easy |

---

## Key Takeaways

- Password-protected ZIPs store a crackable hash — they are NOT truly secure against offline attacks
- `zip2john` + John the Ripper is the standard workflow for encrypted ZIP challenges
- Weak passwords from `rockyou.txt` are cracked in seconds — always use strong random passwords
