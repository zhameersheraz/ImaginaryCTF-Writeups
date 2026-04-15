# Horse Stalking 3 — ImaginaryCTF Writeup

**Challenge:** Horse Stalking 3  
**Category:** OSINT  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{kotohira_shrine}`

---

## Description

> This horse is wearing a very cute menko, can you find out where he lives as of October? Submit the closest shrine to his stable. Hint: it's the same horse as in part 1.

**Attachment:** Photo of a horse wearing a menko (face cover)

---

## Background Knowledge (Read This First!)

### What is a Menko?

A **menko** (面子) is a decorative face cover worn by horses in Japan, often embroidered with patterns or the stable's logo. It can help identify which stable or trainer owns the horse.

### What is Kasamatsu Racecourse?

**Kasamatsu Racecourse** is a regional horse racing track in Gifu Prefecture, Japan. Trainers at this track have stables located right next to or near the track.

### How do you find where a horse currently lives?

Follow the **owner's social media** — many Japanese horse owners post regular updates about their horses including which stable they are at and which trainer is handling them.

---

## Solution

### Step 1 — Recall the horse from Part 1

The horse is **Shishamo** (from Horse Stalking Part 1).

### Step 2 — Stalk the owner's social media

Search for Shishamo's owner online and find their social media accounts. Their posts reveal:

```
Shishamo is currently living at his trainer's stable
at the Kasamatsu Racecourse in Gifu Prefecture, Japan.
```

### Step 3 — Find the closest shrine to Kasamatsu Racecourse

Search Google Maps for shrines near Kasamatsu Racecourse:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://maps.google.com/?q=kasamatsu+racecourse+gifu"
# → Search for shrines nearby
```

The closest shrine is: **Kotohira Shrine**

### Step 4 — Format the flag

```
ictf{kotohira_shrine}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Social media / owner's blog | Find current location of Shishamo | ⭐⭐ Medium |
| Google Maps | Find the closest shrine to the stable | ⭐ Easy |

---

## Key Takeaways

- Japanese horse owners are very active on social media — great for OSINT
- The menko design can hint at the trainer or stable — look for logos or patterns
- Google Maps "search nearby" is essential for location-based CTF questions
