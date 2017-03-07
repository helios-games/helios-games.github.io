---
layout: default
title: Developing a REST API to your game
---
To allow a game frontend to interact with a game backend, it needs to implement a loose conract, which typically have at least two operations:

#### Get the game

This takes the player's ID and the game's name as an input and outputs the current state of the game, e.g.:

    curl http://roulette:8080/games/roulette -H "PlayerId: 123"
    HTTP 200 OK
    {
      "pocket": 24
    }

#### Place bet

This takes the player's ID, the location of the wallet, and game name, and be details as input, and outputs the new game state:

    curl http://roulette:8080/games/roulette/bets/red -H "PlayerId: 123" -H "Wallet: http://wallet:8080/234" -H "Content-Type: application/json" -d "{'amount': 1.23}"
    HTTP 201 Created
    {'balance': 3.45, 'pocket': 8}
