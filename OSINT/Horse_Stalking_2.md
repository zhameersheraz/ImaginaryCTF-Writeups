# Horse Stalking 2 — ImaginaryCTF Writeup

**Challenge:** Horse Stalking 2  
**Category:** OSINT  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{el_condor_pasa_mr._prospector}`

---

## Description

> Find the name of the horse circled in red and the most recent common ancestor of the 2 horses in the 2 images. All lowercase, words separated by underscore.

**Attachment:** Two low-resolution images

---

## Background Knowledge (Read This First!)

### What is Reverse Image Search?

**Reverse image search** lets you upload a photo and find where else it appears online — revealing the identity of people, animals, or locations in the photo.

### What is the Prix de l'Arc de Triomphe?

The **Prix de l'Arc de Triomphe** is one of the most prestigious horse races in the world, run at Longchamp, France. The main sponsor between 1999 and 2007 was **Groupe Lucien Barrière**.

### What is a Common Ancestor in Horse Racing?

Thoroughbred horses have well-documented bloodlines. You can trace any two horses back to find their **most recent common ancestor** using pedigree databases like **Equinbase** or **Blood-Horse**.

---

## Solution

### Step 1 — Identify the horse circled in red

Reverse image search `horse.jpg` → the horse is **Skibidi Rizz**.

### Step 2 — Identify the race in the second image

Key clues in the second image:
- The words **"Groupe Lucien Barrière"** are visible → main sponsor 1999–2007
- There are **14 horses** in the starting area → only one year had exactly 14: **1999**
- The jockey wearing bright yellow with a blue horizontal stripe → **El Condor Pasa**

### Step 3 — Find the most recent common ancestor

Look up the pedigrees of **El Condor Pasa** and **Skibidi Rizz** on a bloodline database:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://www.equineline.com"
# → Search both horses → compare pedigrees
```

Most recent common ancestor: **Mr. Prospector**

### Step 4 — Format the flag

```
ictf{el_condor_pasa_mr._prospector}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Google Reverse Image Search | Identify horse in first image | ⭐⭐ Medium |
| Historical race knowledge | Identify El Condor Pasa from race clues | ⭐⭐⭐ Hard |
| Equinbase / pedigree database | Find most recent common ancestor | ⭐⭐ Medium |

---

## Key Takeaways

- Sponsor names visible in images can pinpoint the exact year of a race
- Jockey silks (clothing colors/patterns) are unique and publicly documented — use them to ID riders
- Thoroughbred pedigree databases trace bloodlines back hundreds of years
