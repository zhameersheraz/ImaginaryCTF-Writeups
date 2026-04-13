# Horse Stalking 5 — ImaginaryCTF Writeup

**Challenge:** Horse Stalking 5  
**Category:** OSINT  
**Difficulty:** Easy-Medium  
**Points:** 50 pts  
**Flag:** `ictf{36.2459,136.2326}`

---

## Description

> why is shishamo giving the side-eye, anyways can you find where this photo was taken, submit your flag as the gps coordinates of where you believe this was taken, use 4 decimal points of precision and round, separate latitude and longitude by comma, for example: `ictf{35.6674,139.7768}`

**Hint:** The least significant digit of latitude is 9, and of longitude is 6.

**Attachment:** Photo of a horse named Shishamo

---

## Background Knowledge (Read This First!)

### What is Geolocating a Photo?

**Geolocating** means figuring out the exact real-world location where a photo was taken. You do this by identifying landmarks, signs, or other clues in the image and then matching them to known locations.

### What are GPS Coordinates?

**GPS coordinates** are a pair of numbers (latitude, longitude) that pinpoint any location on Earth:

- **Latitude** — north/south position (e.g., 36.2459)
- **Longitude** — east/west position (e.g., 136.2326)

### How do you find where a horse lives?

Horses that are pets or competition horses are often tracked on **owner blogs** or **stable websites**. If you know the horse's name, you can search for it and find where it currently lives.

---

## Solution

### Step 1 — Identify the horse

The description tells us the horse's name is **Shishamo**. Search for it online.

### Step 2 — Find the owner's blog

Searching for "Shishamo horse" leads to the owner's blog:

```
https://note.com/akio_ichihara/n/n6cfed8d195ce
```

The blog reveals that Shishamo now lives at the **Padudu Club Riding** stable.

### Step 3 — Find the stable address

The stable's website reveals the address:

```
http://padodo.net/content3.html
Address: 50-4-1 Awara, Fukui 910-4273, Japan
```

### Step 4 — Get the GPS coordinates

Search the address on Google Maps:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://maps.google.com/?q=50-4-1+Awara+Fukui+Japan"
```

Right-click the location on the map → **What's here?** → coordinates appear.

Coordinates: **36.2459, 136.2326**

Confirm with the hints: last digit of latitude = **9** ✅, last digit of longitude = **6** ✅

### Step 5 — Submit the flag

```
ictf{36.2459,136.2326}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Google Search | Find the horse's owner blog | ⭐ Easy |
| note.com (owner blog) | Find which stable Shishamo moved to | ⭐ Easy |
| padodo.net (stable website) | Find the physical address | ⭐ Easy |
| Google Maps | Get GPS coordinates from the address | ⭐ Easy |

---

## Key Takeaways

- Horse owners often document their horses' lives in detail on blogs — very useful for OSINT
- Once you have an address, Google Maps gives you precise GPS coordinates instantly
- The digit hints in the description let you verify you have the right coordinates before submitting
- Always check both the blog AND the stable's own website — they may have different info
