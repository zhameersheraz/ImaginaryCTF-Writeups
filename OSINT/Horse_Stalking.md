# Horse Stalking — ImaginaryCTF Writeup

**Challenge:** Horse Stalking  
**Category:** OSINT  
**Difficulty:** Easy-Medium  
**Points:** 50 pts  
**Flag:** `ictf{shishamo_20-10-2022_10-09-2025_salt_mill}`

---

## Description

> Can you find out who this horse is? Combine: horse name, date of first win, date of final race, winner of final race. All lowercase, words/components separated by underscore, day-month-year format.

**Attachment:** Photo of a horse with a feed bucket

---

## Background Knowledge (Read This First!)

### What is Katakana?

**Katakana** is one of the Japanese writing systems, commonly used to write foreign words and names. Horse names in Japan are often written in katakana on equipment like feed buckets and nameplates.

### What is JBIS?

**JBIS (Japan Bloodhorse Information System)** is the official Japanese horse racing database. You can search any JRA horse by name and find their complete racing record including all race dates and results.

---

## Solution

### Step 1 — Read the katakana on the feed bucket

Look closely at the feed bucket in the image. The katakana text reads:

```
シシャモ  →  Shishamo
```

### Step 2 — Look up Shishamo on JBIS

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://www.jbis.or.jp/horse/search/?word=shishamo"
```

Find the horse's full racing record:

- **First win:** 20-10-2022
- **Final race:** 10-09-2025
- **Winner of final race:** Salt Mill

### Step 3 — Format the flag

```
ictf{shishamo_20-10-2022_10-09-2025_salt_mill}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Image inspection | Read katakana on the feed bucket | ⭐⭐ Medium |
| JBIS | Look up the horse's full racing record | ⭐ Easy |

---

## Key Takeaways

- Horse equipment (feed buckets, nameplates) often has the horse's name written on it
- Katakana is used for horse names in Japan — learn to recognize it
- JBIS is the go-to database for Japanese racehorse records
