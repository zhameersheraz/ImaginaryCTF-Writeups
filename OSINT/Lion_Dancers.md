# Lion Dancers — ImaginaryCTF Writeup

**Challenge:** Lion Dancers  
**Category:** OSINT  
**Difficulty:** Easy-Medium  
**Points:** 50 pts  
**Flag:** `ictf{201_San_Pedro_Dr_SE_Suite_B1}`

---

## Description

> Find the address of where this photo was taken and earn some good luck for the new year! Do not include zip code, city, or state. Capitalize all parts. Separate with underscores. eg: `ictf{123_Main_St_NW_Suite_A}`

**Attachment:** Photo of a lion dance performance

---

## Background Knowledge (Read This First!)

### What is a Lion Dance?

A **lion dance** is a traditional Chinese performance often seen at Lunar New Year celebrations. The dancers wear an elaborate lion costume and perform choreographed movements to bring good luck.

### What is the OSINT Chain?

1. Find a phone number visible on a sign in the image
2. Google the phone number → identifies a building in Albuquerque, NM
3. Use Google Maps to find the area
4. Identify the Vietnamese bakery across the street (from the Nail & Spa reference)
5. The Vietnamese bakery's address is the flag

---

## Solution

### Step 1 — Inspect the image for a phone number

Look at the right side of the image — there is a sign for a **phone repair store** with a visible phone number.

### Step 2 — Google the phone number

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://google.com"
# → Search the phone number
# → Results show a building in Albuquerque, NM
```

### Step 3 — Navigate to the location on Google Maps

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://maps.google.com"
# → Search the Albuquerque address
# → Find the nail and spa place across the street
# → Find the Vietnamese bakery across from that
```

### Step 4 — Get the Vietnamese bakery's address

The address is: **201 San Pedro Dr SE, Suite B1**

### Step 5 — Format and submit the flag

```
ictf{201_San_Pedro_Dr_SE_Suite_B1}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Image inspection | Find the phone number on the repair store sign | ⭐ Easy |
| Google Search | Identify the building from the phone number | ⭐ Easy |
| Google Maps | Navigate to find the Vietnamese bakery | ⭐ Easy |

---

## Key Takeaways

- Phone numbers visible in images are searchable and reveal addresses
- OSINT chains: phone number → building → neighborhood → specific business across the street
- Lion dances happen around Lunar New Year — a geographic and cultural timing clue
- Bonus: Can you find the exact time the photo was taken? 😃
