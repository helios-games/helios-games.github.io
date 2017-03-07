---
layout: guide
title: Interacting with the wallet
---
Every time a player plays a game, we will want to be able to take a payment. This comes from a virtual wallet, which was originally funded by a payment from the player. There are two primary operations on a wallet:

* Get the current balance
* Create a transaction

In the case of a transaction, this is likely to be either a wager or a payout. Typically only one wallet is needed per request for a single player game. There can be others, e.g. to hold funds in escrow for big jackpot payout.

For reporting purposes, funds should not be held either in memory or within state associated with a game (see below).

The wallets are held in a microservice that games interact with over a REST API.

Next: [Mocking the wallet](mocking-the-wallet)
