# Overview

The platform is a multi tennant, multi user system fot builing isolated single and multiplayer games. It uses the latest technologies and archtectural best practice to bring stability, reliability, and performance together in an integrated best-of-breed platform  

# Technology Stack

Containerised microservices with REST API first design.

* Kubernetes
* Docker
* Mongo
* Postgres
* Nginx
* CircleCI
* Git/GitHub 
* Spring Boot
* Playframework
* Swagger 2 (OpenAPI)
* Ruby
* Middleman
* Jekyll
* HTML 
* CSS
* JavaScript
* jQuery
* Phaser (PIXIJS)

## How to create a game

This guide shows you how to create a game that runs on the platform. It covers:

The backend:

* Modeling a game
* Testing random behaviour
* Montecarlo simulation
* "Critical components"
* Interaction with the wallet
* Mocking the wallet
* Reading and writing game state
* Developing a REST API to your game

The front end:

* Stubbing the API for easier frontend development
* Building a frontend to your game's backend

The whole package:

* Running the platform locally
* Writing smoke tests
* Deployment

### Modelling a game

The core of a game is a model of its behaviour. This is usually modelled based on the game's domain, and therefore divorced for conerns such as HTTP or databases. For example, roulette has the key terms:

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

### Testing random behaviour

It's hard to test random behaviour, yet games are full of them. Normally, in a game we use random numbers created by a *random number generator* (RNG) to produce this behaviour. But this make it hard to test. To manage this, it help to create an abstraction around the RNG that can then be replaced by something a little less - random.

It the case of roulette, the key random element is what pocket the ball lands on, so we can extract that into a function that just chooses a new random pocket:

     () => Pocket

We can then replace this with either a [random version](https://github.com/phoebus-games/gf/blob/master/src/main/scala/roulette/model/Roulette.scala#L13) or, for testing purposes, a [predicable one](https://github.com/phoebus-games/gf/blob/master/src/test/scala/roulette/model/RouletteTest.scala#L11).

### Montecarlo Simulation

Along with normal testing, such as unit or integration, it is important to test out random behaviour in games. This can be achieved by performing montecarlo simulations. These make sure a game paysout the correct return to the player. This is typically around 97-99% for a casino game, 95-97% for a slot. A simulation may need to run upwards of 10m to 100m plays to produce an estimate that is useful.

### "Critical components"

Some components are considered "critical". These are the ones that are critically important to the bahaviour of the game. this includes the RNG and the model. As changing these is typically more risky than changing other components, its worthwhile separating them out into their own software package or module. This reduces the risk of accidentally changing them.

### Interacting with the wallet

Every time a player plays a game, we will want to be able to take a payment. This comes from a virtual wallet, which was originally funded by a payment from the player. There are two primary operations on a wallet:

* Get the current balance
* Create a transaction

In the case of a transaction, this is likely to be either a wager or a payout. Typically only one wallet is needed per request for a single player game. There can be others, e.g. to hold funds in escrow for big jackpot payout. 

For reporting purposes, funds should not be held either in memory or within state associated with a game (see below).

The wallets are held in a microservice that games interact with over a REST API.

### Reading and writing game state

Games may wish to store state with a game, e.g.:

* The current bets on the game
* The players
* Any trophies or achievements the player has earnt

This can be done by connecting to a Mongo database. Each game gets its own database, so can store what it likes.

### Developing a REST API to your game

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
