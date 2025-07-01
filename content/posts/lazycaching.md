---
date: '2025-07-01T01:28:34-07:00'
title: 'Lazy Caching'
draft: false
---

Let's say you have an app where users can search up any book. From the user side, it might seem like we need a database row for every single edition, with title, publisher, description, cover image, and so on. Especially for descriptions and cover images, when considering up to 50 million editions, costs can accrue quickly, not to mention acquiring such a database would be difficult.

A solution which I am quite partial to is what is known in system design as lazy caching. What this means is that we only load something in the first time it is needed and we don't have it, and for subsequent requests, since we already have it, we don't need to load it again. What I find elegant about it is that for the cost of one user having slightly higher latency, database size is much smaller, it allows the database to be built over time rather than all at once, and most importantly, to the user it appears as if the whole database is present.

For our scenario, lazy caching can be applied in a number of ways. We could cache the editions themselves; if we don't have the requested ISBN, we look it up along with its information and simultaneously store it and serve it to the user. We could also cache descriptions, cover images, or even all the metadata, together or separate.

I opted for a hybrid solution. Since OpenLibrary had dumps of edition metadata that did not include covers, I simply used that for title, author, ISBN, publisher, etc., but lazy cached descriptions for cost-saving reasons. Separately, I used two flags for retrieving cover images since not every edition has a cover image: `checked_for_image` and `has_cover_iamge`. Finally, to keep my editions rows themselves up-to-date, I did implement an ISBN-level cache, whereby if a new book was published, we would simply load everything aforementioned upon request.