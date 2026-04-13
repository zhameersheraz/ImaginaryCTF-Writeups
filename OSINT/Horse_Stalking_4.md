# Horse Stalking 4 — ImaginaryCTF Writeup

**Challenge:** Horse Stalking 4  
**Category:** OSINT  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{kosho_bokujo}`

---

## Description

> by popular demand the horse stalking will continue for this month. Can you find the farm that owns this mare, enter flag as all lowercase separate words by underscore, the correct flag has 2 words, the first letter of the second word is 'b', the last 2 letters of the second word are 'jo'.

**Attachment:** Image of a horse

---

## Background Knowledge (Read This First!)

### What is OSINT?

**OSINT (Open Source Intelligence)** means finding information using only publicly available sources — Google, social media, websites, image search, etc. No hacking needed, just smart searching.

### What is Reverse Image Search?

**Reverse image search** lets you upload a photo and find where else it appears on the internet. Instead of searching by text, you search by the image itself.

Tools you can use:
- Google Images → click the camera icon
- `https://images.google.com`
- `https://tineye.com`

### What is a Bokujo?

**牧場 (bokujo)** is the Japanese word for "ranch" or "farm." Many thoroughbred horse breeding farms in Japan use this word in their name.

---

## Solution

### Step 1 — Download the image and reverse image search it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O horse.jpg
```

Then go to `https://images.google.com`, click the camera icon, and upload `horse.jpg`.

### Step 2 — Find the Twitter/X account that posted it

The reverse image search reveals the image was posted by:

```
https://x.com/Koushou_F
```

### Step 3 — Visit their website

Go to the website linked in the Twitter profile. You will see the farm name listed as **Takaaki Ranch Ltd** — but this is the wrong translation.

### Step 4 — Translate the hiragana correctly

The hiragana name of the farm correctly translates to:

```
Kosho Ranch Ltd  →  more commonly known as  →  Kosho Bokujo
```

Kosho Bokujo is one of the largest thoroughbred breeders in Urakawa, Japan.

### Step 5 — Format the flag

```
farm name = kosho bokujo
flag format = all lowercase, words separated by underscore
→ ictf{kosho_bokujo}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Google Reverse Image Search | Find where the horse photo was posted | ⭐⭐ Medium |
| Twitter/X | Find the farm's account and website link | ⭐ Easy |
| Japanese translation | Correctly translate the hiragana farm name | ⭐⭐ Medium |

---

## Key Takeaways

- **Reverse image search** is the first tool to reach for in any photo-based OSINT challenge
- Machine translations are not always correct — always double check hiragana/kanji translations
- Social media profiles often link to official websites that contain the real answer
- The flag hints (`first letter = b`, `ends in jo`) help confirm you have the right answer
