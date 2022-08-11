---
title: "Olympus Breakdown"
date: 2021-11-01T21:35:54Z
draft: true
disqus: true
---

I want to preface this by saying that I have no pre-existing biases for or against OlympusDAO. I hold no OHM bags so I have no reason to shill and if anything I expect to collaborate with most DeFi protocols over time so I have no reason to put OHM down either. I'm simply a protocol founder who has heard great things about the [DeFi 2.0](https://thedefiant.io/olympusdao-uniswap-defi-2-0-liquidity-mining/) protocols and finally got around to digging in. 

If you know nothing about OlympusDAO, [this](https://docs.olympusdao.finance/main/) is a good place to get started. Basically, OlympusDAO has created OHM to become a reserve-backed digital currency. Think about a stablecoin, but with only a floor price ($1) and no upper bound on price. It is loosely [modeled](https://docs.olympusdao.finance/main/basics/basics#is-ohm-a-stable-coin) after the US dollar when it was on the gold standard.

There are two main actions you can take on Olympus V1: [Staking](https://docs.olympusdao.finance/main/basics/staking) and [Bonding](https://docs.olympusdao.finance/main/basics/bonding).

Staking is when you put your OHM in a Olympus-controlled contract that gives you more OHM over time. 

Bonding is when you buy OHM tokens directly from Olympus for a small discount.

Congratulations, you now understand the basics of Olympus!

Let's go into a bit more depth about what each of these actions are. 

## Staking

The elephant in the room is the >8,000% APY currently offered to stakers. How does this work?

First, the current "monetary policy" of OHM is that of EXTREME inflation. 

Every 5 days, the total amount of OHM tokens increases by roughly 6% as seen [here](https://app.olympusdao.finance/#/stake).

Once you take into account compounding, the OHM token supply increases by 44% every month and >8,000% every year.

Let's put this into context:

If you own 1 OHM token when there are 100 OHM tokens, you own 1% of the OHM supply. 

After 1 year, your 1 OHM token will now be amongst ~8,100 other OHM tokens, giving you 0.012% of the supply. Your ownership of the OHM supply has decreased by ~99%.

This is problematic. So how do we fix it? Staking.

If you stake your 1 OHM token, it grows (roughly) proportionally to the OHM token supply, meaning you would still own ~1% of the total OHM supply after a year. 

(Note: The estimate is "rough" because the OHM supply is never 100% staked, which we'll talk about later.)

With staking, we can maintain our ownership of the OHM supply!

In case you didn't know, one of the biggest misconceptions in crypto and traditional finance is that a lower token (or stock) price equals a lower valuation. This is why people think they are getting a screaming deal when they buy [Shiba Inu token](https://www.coingecko.com/en/coins/shiba-inu) at a price of $0.00007 but won't buy any [Compound tokens](https://www.coingecko.com/en/coins/compound) because they cost $364 each. The truth is that Shiba Inu's market cap is almost 20 times higher than Compound's! This makes Compound much cheaper than Shiba Inu. Each individual Shiba Inu coin may be cheaper but there are 550 trillion (no joke) of them out there, while there are only 6 million Compound tokens in existence. For us, it means we have to think about everything in terms of what % of the market cap (or token supply) we own.

This is where the 8,000% APY for staking OHM falls apart.

Imagine a raffle where the prize pool is $100. The organizer of the raffle comes to you and says there is only one ticket for this raffle. If there's only one ticket, you're guaranteed to win. You offer to pay $100 for the ticket (you could offer a lower price but the organizer is a friend).

By the way, the organizer of the raffle owes you $1000 in an unrelated gambling debt. Just before the raffle starts, the organizer presents 10 more tickets and says they are all eligible to win the raffle. Before you say anything, the organizer graciously gives you the 10 tickets. Then he says, "I sold a raffle ticket for $100 earlier, so I assume each of these 10 tickets is worth $100. My $1000 gambling debt is now paid off." At this point, you may be reconsidering your friendship with the organizer. Of course, if the prize pool is $100 and there are now 11 tickets eligible to win, each ticket is not worth $100. 

As you may have guessed, Olympus is the raffle organizer. The 8,000% APY is akin to saying a raffle ticket was just sold for $100, so all future raffle tickets created should be worth $100. 

If we go back to thinking in terms of token supply, it becomes quite obvious what is happening. If you don't stake your OHM, your ownership of the supply will decrease >99% in one year. If you do stake, you simply maintain the same ownership of the supply. Staking promises huge APYs, but in reality staking is a mandatory exercise to maintain your status quo ownership of the protocol. 

Some fun stats: Because thousands of new OHM tokens are created every day, we have to think about OHM in terms of market cap and total OHM supply. Right now, OHM is stated to have a market cap of [~$4.8Bn](https://www.coingecko.com/en/coins/olympus) (fully diluted). Olympus is currently promising an APY of [8,174%](https://app.olympusdao.finance/#/stake) and [~92%](https://dune.xyz/shadow/Olympus-(OHM)) of the OHM supply is staked right now. In order for people to actually get an 8,174% APY, the OHM market cap will have to increase to ~$365Bn in 12 months. This would make it the [third largest cryptocurrency](https://www.coingecko.com/en) in the world behind Bitcoin and Ethereum and 4x more valuable than the current 3rd largest cryptocurrency. I'm not saying this won't happen, I'm just saying it *has to* happen to make the APYs real. 

At this point, you might think I have a negative view of staking. I don't. Staking is a really cool mechanism to reward a certain action by tokenholders. For Olympus, the action is the staking itself. If you are staked, it means you aren't selling in that moment. If Olympus wanted to take things a step further, they'd create a lockup mechanism in the staking contract that paid more rewards to people who locked up for longer. If you lock up for a year, you DEFINITELY aren't selling in that moment. Aave has a very cool staking mechanism called the [Safety Module](https://docs.aave.com/aavenomics/safety-module) which locks up tokens for 10 days and uses the staked funds to fund any shortfalls in the protocol. [Sherlock](https://sherlock.xyz/) is considering a staking mechanism that incentivizes and weights governance voting similar to Curve's [voting contract](https://resources.curve.fi/faq/voting).

Olympus is just another protocol that has found an interesting use for staking. While I don't agree with their [marketing](https://docs.olympusdao.finance/main/basics/staking) (it's not really value accrual, more like prevention of value depletion) or their advertised APYs, I think it's a fascinating way to incentivize users to keep their tokens in a certain contract. 

## Bonding

Phew, we made it through staking. Don't worry, bonding is easier.

I think Olympus's bonding mechanism is really cool. If you're familiar with secondary offerings in the stock market (Tesla [does them](https://www.cnbc.com/2020/12/08/tesla-to-raise-up-to-5-billion-in-share-offering.html) all the time) then bonding will make a lot of sense.

If not, read on. Bonding is just a way for a protocol to sell their governance token for money. Because there are many places to buy a governance token (SushiSwap, etc.), a protocol's bonding mechanism usually sells the governance tokens at a discount to market price. For Olympus, bonding is a continuous process. The mechanism is constantly trying to sell a target amount of OHM over a time period, so the bond "discount" is constantly adjusting to stay close to the target amount. It's quite cool actually and it allows a protocol to get a very good price for their token without tanking the market too much at any given time. And of course, the protocol keeps the money that is paid for the tokens. 

The original use case for the bonding mechanism was to accumulate LP tokens for [OHM liquidity pools](https://app.sushi.com/add/0x383518188c0c6d7730d91b2c03a03c837814a899/0x6b175474e89094c44da98b954eedeac495271d0f) on SushiSwap. Because protocols often have to pay governance tokens to LPs in order to incentivize them, this LP bonding mechanism allows protocols to slowly own the LP tokens directly, removing the need to incentivize as many external LPs.

Because of the novelty of the Olympus bonding mechanism, Olympus has created Olympus Pro which allows other protocols to access the same mechanism and work towards owning their own liquidity (for a [3.3% fee](https://olympusdao.medium.com/introducing-olympus-pro-d8db3052fca5)).

Astute readers may say "If Olympus offers me a discount on OHM compared to the SushiSwap price, why wouldn't I just immediately sell my OHM on SushiSwap?" Great question. To combat arbitrageurs, Olympus has instituted a 5-day linear vesting period for OHM received in bonding. Basically, you can access half of your vesting OHM after 2.5 days and you can access all of it after 5 days. As we'll see, this vesting combines explosively with the staking mechanism. 

At the time of writing, the current "discount" on [OHM-DAI LP token bonding](https://app.olympusdao.finance/#/bonds/ohm_dai_lp) is 3.5%. Seems like a good discount right? Not so fast. The advertised discount is HIGHLY misleading. The "catch" is that the OHM you start vesting is NOT staked during the 5-day vesting period. If you remember from the beginning of this article, the "inflation" of OHM after 5 days is 6%. If you miss out on 5 days of staking, you lose ~6% of your ownership of the OHM market cap. That 3.5% discount you thought you were getting? Not looking so good anymore. Of course, you can claim and stake partial amounts of your vested OHM before the 5 day period. If you were to perfectly claim and stake your earned OHM every block over 5 days and gas fees didn't exist, you would still lose ~3% of your ownership of the protocol. This is why the advertised discount on bonding is very misleading. You must read the fine print.

To give them credit, Olympus does plan to fix the lack of staking during vesting in the [V2](https://olympusdao.medium.com/introducing-olympus-v2-c4ade14e9fe). Expect advertised bonding discounts to drop accordingly.

## Recap
At its core, there are only two actions to take in Olympus: staking and bonding. 

Staking is simply a mandatory action that prevents you from losing % ownership in the protocol. It's table stakes.

Bonding is a way for people to buy into the protocol (usually at a small discount).

There is nothing more to Olympus than this.

This brings us to a very interesting question. 

## Is OHM a Ponzi scheme?
This is not an unfamiliar question to Olympus. It was even a question in their offical [FAQ](https://docs.olympusdao.finance/main/basics/basics) at one point, but seems to have been removed.

Here's an excerpt of the SEC definition of a [Ponzi scheme](https://www.investor.gov/introduction-investing/investing-basics/glossary/ponzi-schemes):

`A Ponzi scheme is an investment fraud that pays existing investors with funds collected from new investors...Ponzi scheme organizers often promise high returns with little or no risk. Instead, they use money from new investors to pay earlier investors and may steal some of the money for themselves.`

Of course, people have called the [US dollar](https://www.alleywatch.com/2019/01/the-us-dollar-is-a-ponzi-scheme/) and [Bitcoin](https://www.cnbc.com/2021/04/23/bitcoin-a-gimmick-and-resembles-a-ponzi-scheme-black-swan-author-.html) ponzi schemes as well. So take all of this with a grain of salt.

But one thing that stands out to me: The creators of Bitcoin and the US dollar never promised "high returns" while the same can't be said for OHM and the 8,000% APYs currently promised to investors. 

Outside of the high returns, the main criteria for a ponzi (according to the definition above) seems to be whether the "high returns" are paid for with funds from new investors. 

Basically, if Olympus generates 8,000% returns with anything but investor funds, then it would seem to be in the clear. 

According to Olympus, the value of OHM is based on [two criteria](https://docs.olympusdao.finance/main/basics/basics#ohm-is-backed-not-pegged.): the intrinsic value and a premium.

Let's talk intrinsic value first. At the time of writing, there are 4.4M 









