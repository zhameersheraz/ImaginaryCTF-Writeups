# Vacation Photos — ImaginaryCTF Writeup

**Challenge:** Vacation Photos  
**Category:** OSINT  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{34.98593,135.74842}`

---

## Description

> moai_man was missing for the last week of February but they left this photo, find the gps coordinates of where it was taken. Include 5 digits of precision. Least significant digit of latitude is 3, longitude is 2.

**Attachment:** A vacation photo

---

## Background Knowledge (Read This First!)

### What is Geolocating a Photo?

**Geolocating** means figuring out the exact real-world location where a photo was taken by identifying landmarks, signs, or distinctive features and matching them to known locations.

### What is Google Reverse Image Search?

**Reverse image search** lets you upload an image and find where else it appears on the internet — often revealing the location if it is a well-known place.

### What is Umekoji Park?

**Umekoji Park** (梅小路公園) is a large public park in Kyoto, Japan, known for its plum trees and the nearby Railway Museum. It is a popular tourist destination.

---

## Solution

### Step 1 — Reverse image search the photo

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://images.google.com"
# → Upload the vacation photo → search by image
```

Results show the photo was taken at **Umekoji Park, Kyoto, Japan**.

### Step 2 — Find the exact spot on Google Street View

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://maps.google.com/?q=Umekoji+Park+Kyoto"
# → Switch to Street View → navigate to match the photo's angle and background
```

The exact location is next to coordinates **34.9859301, 135.7484277**.

### Step 3 — Format to 5 decimal places and verify hints

```
Latitude:  34.98593  → last digit = 3 ✅
Longitude: 135.74842 → last digit = 2 ✅
```

### Step 4 — Submit the flag

```
ictf{34.98593,135.74842}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Google Reverse Image Search | Identify the location from the photo | ⭐ Easy |
| Google Maps / Street View | Find the exact GPS coordinates | ⭐ Easy |

---

## Key Takeaways

- Reverse image search is always the first tool to reach for in photo OSINT challenges
- Google Street View lets you navigate to match the exact camera angle
- The digit hints (last digit of lat = 3, long = 2) confirm you have the right coordinates
