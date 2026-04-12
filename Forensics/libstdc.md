# libstdc++5 i386 — ImaginaryCTF Writeup

**Challenge:** libstdc++5 i386  
**Category:** Forensics  
**Difficulty:** Easy-Medium  
**Points:** 65 pts  
**Flag:** `ictf{if_only_steghide_worked_without_depreciated_libs_from_a_decade_ago}`

---

## Description

> The problem with hiding passwords in steg, is that you need to remember a passphrase, if only there was a simpler way...

---

## Background Knowledge (Read This First!)

### What is Steganography?

**Steganography** is the art of hiding secret data *inside* an innocent-looking file (like an image). Unlike encryption (which scrambles data), steganography *hides* the fact that secret data even exists.

### What is Steghide?

**Steghide** is a command-line tool that hides files inside JPEG or BMP images using a passphrase. To extract the hidden file, you need:

1. The image file
2. The correct passphrase

### What is a polyglot file?

A **polyglot** file is valid in two formats at once. Here, the `.jpg` image also has a **ZIP file appended at the end**. Most programs see it as a normal JPEG, but `binwalk` will reveal the hidden ZIP inside.

### Why is the challenge named `libstdc++5 i386`?

`libstdc++5 i386` is a very old, deprecated Linux library. Steghide was compiled against it — meaning on modern systems you may need to install this ancient library to run it. The flag itself jokes about this!

---

## Solution

### Step 1 — Go to your files

```
┌──(zham㉿kali)-[~]
└─$ cd /media/sf_downloads
```

### Step 2 — Inspect the image for hidden data

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ binwalk image.jpg
# → Look for a ZIP signature (PK header) after the JPEG data
```

Extract everything:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ binwalk -e image.jpg
# → Creates a folder with extracted contents including the encrypted ZIP
```

### Step 3 — Find the passphrase visually

Open the image and look closely (zoom in, adjust brightness/contrast). You will see the phrase:

```
STEGIFIEDHORSE
```

written in **very low opacity** somewhere in the image. This is the steghide passphrase.

### Step 4 — Install the old library if needed

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sudo apt-get install libstdc++5:i386
```

### Step 5 — Extract with Steghide

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ steghide extract -sf image.jpg -p STEGIFIEDHORSE
# → Writes out the hidden file
```

### Step 6 — Read the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat extracted_file.txt
# → ictf{if_only_steghide_worked_without_depreciated_libs_from_a_decade_ago}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `binwalk` | Detect and extract the hidden ZIP inside the JPEG | ⭐⭐ Medium |
| Image editor | Spot the low-opacity passphrase text | ⭐ Easy |
| `steghide` | Extract the hidden file using the passphrase | ⭐⭐ Medium |

---

## Key Takeaways

- Always run `binwalk` on suspicious images — hidden ZIPs appended to JPEGs are a classic CTF trick
- Steganography passphrases are often hidden *inside* the same image
- Low-opacity text is nearly invisible at normal brightness — always adjust levels when inspecting
- The challenge name itself is a hint about which tool you need to install!
