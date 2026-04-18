# school — ImaginaryCTF Writeup

**Challenge:** school  
**Category:** OSINT  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{03-11-1793}`

---

## Description

> My friend sent me this screenshot of their drive to school. What was the birthday of the person whom their school is named after? Submit in `ictf{dd-mm-yyyy}` format.

**Attachment:** Screenshot of a Google Maps route to school

---

## Background Knowledge (Read This First!)

### What is the OSINT Process Here?

1. Identify the school from the map screenshot
2. Find the person the school is named after
3. Look up their birthday

### Who is Stephen F. Austin?

**Stephen F. Austin** (November 3, 1793 – December 27, 1836) is known as the "Father of Texas." He led one of the first Anglo-American colonization efforts in Texas and is one of the most important figures in Texas history. Many schools and places in Texas are named after him.

---

## Solution

### Step 1 — Identify the road intersection in the screenshot

Look at the map screenshot — it shows **Old Richmond Rd and FM 1464 N**. The route also passes **Pheasant Creek Dr** as a sanity check landmark.

### Step 2 — Search the intersection on Google Maps

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://maps.google.com/?q=Old+Richmond+Rd+FM+1464+N"
# → Identifies a school near that intersection
# → School name: Stephen F. Austin High School (or similar)
```

### Step 3 — Look up Stephen F. Austin's birthday

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://en.wikipedia.org/wiki/Stephen_F._Austin"
# → Born: November 3, 1793
# → In dd-mm-yyyy format: 03-11-1793
```

### Step 4 — Submit the flag

```
ictf{03-11-1793}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Google Maps | Identify the school from the road intersection | ⭐ Easy |
| Wikipedia | Find Stephen F. Austin's birthday | ⭐ Easy |

---

## Key Takeaways

- Road intersections visible in map screenshots are searchable on Google Maps
- Cross-referencing with secondary landmarks (Pheasant Creek Dr) confirms the location
- OSINT birthday challenges just require finding the right Wikipedia article
