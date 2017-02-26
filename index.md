# Overview

The platform is a multi tennant, multi user system fot builing isolated single and multiplayer games. It uses the latest technologies and archtectural best practice to bring stability, reliability, and performance together in an integrated best-of-breed platform  

# Technology Stack

Containerised microservices with REST API first design.

* Ubuntu Linux
* Kubernetes
* Docker
* Mongo
* Postgres
* Nginx
* CircleCI
* Git + GitHub 
* Java 8 + Scala 2.12
* JUnit + Selenium WebDriver + RestAssured
* Spring Boot
* Swagger 2 (OpenAPI)
* Ruby + Middleman
* HTML + CSS
* JavaScript + jQuery + Phaser (PIXIJS)
* Email

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

## Modelling a game

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
* Spinning the wheel: a table with bets remove AND some winnings to payout

Take a look at the [roulette model](https://github.com/phoebus-games/gf/tree/master/src/main/scala/roulette/model).

### Testing random behaviour

It's hard to test random behaviour, yet games are full of them. Normally, in a game we use random numbers created by a *random number generator* (RNG) to produce this behaviour. But this make it hard to test. To manage this, it help to create an abstraction around the RNG that can then be replaced by something a little less - random.

It the case of roulette, the key random element is what pocket the ball lands on, so we can extract that into a function that just chooses a new random pocket:

     () => Pocket

We can then replace this with either a [random version](https://github.com/phoebus-games/gf/blob/master/src/main/scala/roulette/model/Roulette.scala#L13) or, for testing purposes, a [predicable one](https://github.com/phoebus-games/gf/blob/master/src/test/scala/roulette/model/RouletteTest.scala#L11).
