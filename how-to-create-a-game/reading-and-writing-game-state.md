---
layout: guide
title: Reading and writing game state
---
Games may wish to store state with a game, e.g.:

* The current bets on the game
* The players
* Any trophies or achievements the player has earnt

This can be done by connecting to a Mongo database. Each game gets its own database, so can store what it likes.

Next: [Developing a REST API](developing-a-rest-api-to-your-game)
