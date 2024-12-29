---
draft: false
title: 'Sports Betting Devigger'
cover:
    image: "/projects/devigger/profit.png"
    alt: "Profit chart"
    caption: "hello" # display caption under cover
    relative: false # when using page bundles set this to true
---

This tool ({{< newtabref href="https://github.com/jjc256/devigger" title="GitHub" >}}) looks at de-vigorished sports betting odds for bets offered by Pinnacle, a sharp bookmaker, and uses those to find positive EV betting opportunities on FanDuel, a soft bookmaker. Tools like this are not unique, but they are offered as a service for {{< newtabref href="https://oddsjam.com/subscribe" title="high fees" >}}, so I decided to make a simple version myself.

Rather than search for arbitrage opportunities, it looks for statistical arbitrage-esque market inefficiencies since sports betting "markets" are somewhat inefficient.

## Details

I included support for two devigging methods: multiplicative (scales implied probabilities equally so they sum to 1) and power (does the same thing but with exponents).

To determine bet sizing, I used the (logarithmic) wager limit on Pinnacle, which indicates their confidence in their odds for a bet, and Kelly criterion, assuming the devigged implied probabilities were the true probabilities.

{{< newimgref src="/projects/devigger/GUI.png" alt="GUI" width="60%" >}}
<figcaption>Fig. 1. Screenshot of GUI showing positive EV bets. Bets are sorted by risk amount and are highlighted green if they meet certain criteria.</figcaption>

## Exploration

Simply placing all bets is not profitable. Pinnacle is known to be sharper for moneylines, handicaps, and totals, not so much player props. To test this idea, I simply observed results for all bets for about two weeks (backtesting is hard).

As expected, hypothetical profit for all bets steadily declined, but limiting bets to moneylines, handicaps, and totals shorter than or equal to -120 *was* profitable. Of course, you could use hypothesis testing here, but I like vibes. Underdogs being unprofitable suggests that even though the power method accounts for a larger vigorish on the underdog side, it is not enough.

{{< newimgref src="/projects/devigger/profit.png" alt="Chart showing profit" width="80%" >}}
<figcaption>Fig. 2. Results from 11/17/2024 to 12/5/2024 using a bankroll size of around $1,000 and filtering bets. ROI during this span was about 20%. Since bets have shorter odds, losses on losing bets are generally greater than profits on winning bets.</figcaption>

Finally, I added another GUI to periodically monitor for new opportunities and send notifications, but it looks pretty much the same as the first GUI.