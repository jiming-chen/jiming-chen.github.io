---
date: '2025-08-28T10:04:58-04:00'
title: 'Free Chick-fil-A QR Code Exploit'
draft: true
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---

In this blog post, I will describe the process I went through to obtain theoretically unlimited Chick-fil-A (with a few caveats which I will list later). Since Chick-fil-A does not have a bug bounty and abusing it could result in legal action, I will simply make this blog post describing my methods.

## Background

I recently went to a minor league baseball game, and when I was there, they handed out gift cards that you could redeem for one chicken sandwich or an 8-count nuggets. Each card had a QR code on it which you could scan into the Chick-fil-A app and a serial number. Since everyone in my family had received a card, we were able to observe that our serial numbers all shared the same first 11 digits and only differed in their last 3 digits. This made it highly likely that if we could generate the QR code corresponding to another serial number with a different 3 last digits, then it would be accepted by the app.

Therefore, all we had to do was figure out how to go from serial number to QR code.

## Approach

Our general approach was the following: try a new method of going from serial number to QR code and then try it on 10 randomly generated serial numbers (same first 11 digits concatenated with 3 randomly generated digits) to increase confidence that failure was from the QR code generation and not the serial number selection.

First, we tried simply using online tools to generate a QR code for new serial numbers. But those were not accepted by the app.

### Checksum

After scanning directly in the camera app, we realized the QR code did not even render to the serial number on the card. It appended three digits, a dash and checksum, as well as a bunch of spaces:

```text
043                  81337015160824-                4
```

Among the four cards we had, each had the 043 thankfully, so we just had to figure out how they generated the checksum digit. After giving GPT-5 our four codes, it realized that the checksum method was to alternately multiply each digit by 1 and 3 then take that modulo 10 with an offset.

### QR Code Generation

At this point, our goal became to see if we could generate a QR code given a serial number from a known (serial number, QR code) pair to see if we could produce the "canon" QR code.

I had Claude Code generate a script, which allowed us to see that Chick-fil-A uses

- Version 3 (29×29 modules)
- Error correction level M (Medium)
- Mask pattern 0
- No ECI, no structured append
- Segmentation is automatic; given the payload (digits, spaces, hyphen), it encodes as alphanumeric (with numeric subsegments where possible)

It was at this point that we were able to reproduce known QR codes, so we tried QR generation on a "new" serial number (e.g. 81337015160*123*), and the resulting QR code was accepted by the app. Yay!!!

## Caveats

There are a few caveats here, both practical and ethical.

1. If you try to scan a QR code with an incorrect checksum, your account gets flagged, and you can no longer scan rewards/gift cards. This requires you to create a new account.

2. The app only allows you to have three rewards in your account at once; this is not really a problem if you continually use them.

3. There is a chance that the reward has already been redeemed since this exploit uses existing gift cards.

4. Since we are only changing the last three digits, there are only 1,000 serial numbers. However, based on some research and looking at past codes, there is reason to believe that the first four digits correspond to the campaign (8133 seems to mean that it is redeemable at any location). The next two digits seem to be a "subcampaign", and the second of those two digits is always always 0. The last five digits seem to be the actual identifying code. This implies that there are at least 10,000 rewards for this campaign. On top of that, one can easily test other subcampaigns for at least 100,000.

5. Actively taking advantage of this exploit is definitely against Chick-fil-A's terms.

6. When you redeem a generated QR code, the person (who could be a little kid) who receives the gift card with that reward will not be able to redeem it. :cry: