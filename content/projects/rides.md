---
date: '2025-01-14T01:59:49-08:00'
title: 'Optimal Theme Park Ride Order'
cover:
    image: "/projects/rides/magic_kingdom.png"
    alt: "Map of Magic Kingdom"
    relative: false # when usng page bundles set this to true
hideMeta: true
---

[WORK IN PROGRESS] If we assign happiness to certain rides and have information about how much time it takes to ride a string of rides in a certain order, how can we maximize happiness in one day at a theme park? There is a lot of information we can work with:

* Each ride has an associated happiness.
* Each ride has a wait time.
* Total time must not exceed the time the park is open.
* Walking between any two rides takes a certain amount of time.
* Throughout the day, wait times and walk times (crowd levels) change.

If we only incorporate happiness and distance between rides (even accounting changes during the day), we can use a simple maximum flow reduction with {{< newtabref href="https://en.wikipedia.org/wiki/Ford%E2%80%93Fulkerson_algorithm" title="Ford-Fulkerson" >}}.