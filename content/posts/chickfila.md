---
date: '2025-08-28T10:04:58-04:00'
title: 'Generating Free Chick-fil-A QR Codes'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

In this blog post, I will describe the process I went through to obtain theoretically unlimited Chick-fil-A with a few caveats, which I will list later. Since Chick-fil-A does not have a bug bounty and if I actually exploited this information, they would take legal action, I will simply make this blog post describing my methods.

## Background

I recently went to a minor league baseball game, and when I was there, they handed out gift cards that you could redeem for one chicken sandwich or an 8-count nuggets. Each card had a QR code on it which you could scan into the Chick-fil-A app and a serial number. Since everyone in my family had received a card, we were able to observe that our serial numbers all shared the same first 11 digits and only differed in their last 3 digits. This made it highly likely that if we could generate the QR code corresponding to another serial number with a different 3 last digits, then it would be accepted by the app.

Therefore, all we had to do was figure out how to go from serial number to QR code.

## Approach

Our general approach was the following: try a new method of going from serial number to QR code and then try it on 10 randomly generated serial numbers (same first 11 digits concatenated with 3 randomly generated digits) to increase confidence that failure was from the QR code generation and not the serial number selection.

First, we tried simply using online tools to generate a QR code for new serial numbers. But those were not accepted by the app.

### Checksum

After scanning directly in the camera app, we realized the QR code did not even render to the serial number on the card. It appended three digits, a dash and checksum, as well as a bunch of spaces:

```
043                  81337015160824-                4
```

Among the four cards we had, each had the 043 thankfully, so we just had to figure out how they generated the checksum digit. After giving GPT-5 our four codes, it realized that the checksum method was to alternately multiply each digit by 1 and 3 then take that modulo 10 with an offset.

### QR Code Generation

Next, 

Version 3 (29×29 modules)
Error correction level M (Medium)
Mask pattern 0
No ECI, no structured append
Segmentation is automatic; given the payload (digits, spaces, hyphen), it encodes as alphanumeric (with numeric subsegments where possible)