---
layout: default
title: Modelling a game
---
The core of a game is a model of its behaviour. This is usually modelled based on the game's domain, and therefore divorced for concerns such as HTTP or databases. For example, roulette has the key terms:

* A table for placing bets on
* Players
* Bets
* The roulette wheel
* Chips
* Winnings

It's then worthwhile thinking about the events that can happen in the game:

* Spinning the wheel
* Placing a bet
* Paying winnings

This might lead to some states:

* Starting state: an empty table
* Adding a bet: a table with a bet on it
* Spinning the wheel: a table with bets removed AND some winnings to payout

Take a look at the [roulette model](https://github.com/phoebus-games/gf/tree/master/src/main/scala/roulette/model).

Next: [Testing random behaviour](testing-random-behaviour)
