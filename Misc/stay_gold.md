# stay gold — ImaginaryCTF Writeup

**Challenge:** stay gold  
**Category:** Misc  
**Difficulty:** Easy-Medium  
**Points:** 50 pts  
**Flag:** `ictf{horseramblingstego}`

---

## Description

> 2 fun facts, his name was often shortened to stego, and his first win was in hong kong... ps remember to wrap your flag in the ictf format, also it's in all lowercase

**Attachment:** File with encoded values

---

## Background Knowledge (Read This First!)

### What are Unicode Homoglyphs?

**Homoglyphs** are characters from different scripts (languages) that look visually identical or very similar to English letters. For example:

- Chinese character 彡 might look like `s`
- Japanese character ア might look like `A`

The challenge encodes English letters as Chinese/Japanese Unicode characters that visually resemble them. Decoding means mapping each character back to its English equivalent.

### What is Unicode?

**Unicode** is the universal character encoding standard that covers characters from virtually every language and writing system. Each character has a unique number (code point).

The challenge stores values as Unicode code points — decode them to find the homoglyph characters, then map those back to English letters.

---

## Solution

### Step 1 — Open the attachment and decode the Unicode values

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c "
data = [...]  # paste the encoded values here
print(''.join(chr(v) for v in data))
"
# → Displays the Chinese/Japanese homoglyph characters
```

### Step 2 — Map homoglyphs back to English

Each Chinese/Japanese character corresponds to an English letter visually:

```python
# The characters are homoglyphs for: h o r s e r a m b l i n g s t e g o
# → horseramblingstego
```

### Step 3 — Wrap in flag format

```
ictf{horseramblingstego}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Decode Unicode values to characters | ⭐ Easy |
| Visual inspection | Match homoglyphs to English letters | ⭐⭐ Medium |

---

## Key Takeaways

- Unicode homoglyphs are characters from other scripts that look like English letters
- The description hints: "stego" is a shortening of the horse's name — the flag contains it!
- Always check the description for hints about the flag content
