---
layout: guide
title: Testing random behaviour
---
It's hard to test random behaviour, yet games are full of them. Normally, in a game we use random numbers created by a *random number generator* (RNG) to produce this behaviour. But this make it hard to test. To manage this, it help to create an abstraction around the RNG that can then be replaced by something a little less - random.

It the case of roulette, the key random element is what pocket the ball lands on, so we can extract that into a function that just chooses a new random pocket:

     () => Pocket

We can then replace this with either a [random version](https://github.com/phoebus-games/gf/blob/master/src/main/scala/roulette/model/Roulette.scala#L13) or, for testing purposes, a [predicable one](https://github.com/phoebus-games/gf/blob/master/src/test/scala/roulette/model/RouletteTest.scala#L11).

Next: [Monte Carlo simulation](monte-carlo-simulation)
