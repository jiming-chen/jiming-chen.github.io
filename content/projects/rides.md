---
date: '2025-01-14T01:59:49-08:00'
title: 'Optimal Theme Park Ride Order'
cover:
    image: "/projects/rides/magic_kingdom.png"
    alt: "Map of Magic Kingdom"
    relative: false # when usng page bundles set this to true
hideMeta: true
---

If we assign happiness to certain rides and have information about how much time it takes to ride a string of rides in a certain order, how can we maximize happiness in one day at a theme park? {{< newtabref href="https://jjc256.github.io/rides/" title="This app" >}} (also embedded below) seeks to do exactly that.

{{< newtabref href="https://github.com/jjc256/rides" title="Here" >}} is the Github link.

## Demo

<iframe src="https://jjc256.github.io/rides/" width="100%" height="600px"></iframe>

## Motivation

There is a lot of information we can work with:

* Each ride has an associated happiness.
* Each ride has a wait time.
* Total time must not exceed the time the park is open.
* Walking between any two rides takes a certain amount of time.
* Throughout the day, wait times and walk times (crowd levels) change.

My first thought was to use a maximum flow reduction with {{< newtabref href="https://en.wikipedia.org/wiki/Ford%E2%80%93Fulkerson_algorithm" title="Ford-Fulkerson" >}}. However, there seemed to be too many factors which made Ford-Fulkerson seem infeasible.

## Design

Instead, I opted for a greedy solution: say you assign a rating to each ride. At any point for any ride, you know the its wait time as well as how long it will take to get to it. Call the sum of those two times the cost to ride that ride. Then, we can calculate its happiness per cost and repeatedly choose the ride with the greatest happiness per cost.

Although not necessarily optimal, this algorithm ended up being very easy to implement. For example, I thought that estimating distances between rides would be hard and not generalizable to other parks. However, the API I used for wait times conveniently included rides already sectioned into lands, meaning I could assign short walk times to rides in the same or adjacent lands and longer walk times to rides in non-adjacent lands.

Additionally, this greedy algorithm is similar to judgments people actually make in the park, but it allows us to have better information, which makes it appealing.

For practicality reasons, I wanted to be able to use this app outside of park hours, which was when live data was available on the API, so I scraped average wait times and approximately adjusted based on a wait time vs. time of day graph I saw for Magic Kingdom.

This allowed two modes: a pre-park planning mode and an in-park planning mode. The pre-park mode gives an entire day's ride itinerary based on estimations of wait times (which hopefully account for crowd level changes). The in-park planning mode uses live wait time data although I am currently working on letting it recommend based on already ridden rides.