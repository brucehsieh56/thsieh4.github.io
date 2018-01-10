---
layout: default
title: Python VS R command
permalink: /python_vs_R
---


### Python VS R command

---

#### Simulation


Python  | R   | Description
---		| --- | ---
`np.random.binomial(n=2, p=.5, size=10)`| `rbinom(n=10, size=2, prob=.5)`	| **Binomial distribution**; <br>Flip 2 coins for 10 times with prob of head equals to 0.5.
`np.random.normal(mean=, std=, size=10)`| `rnorm(n=10, mean=, sd=)` 		| **Normal distribution**
`np.random.poisson(lam, size=10)` 		| `rpois(n=10, lambda=)` 			| **Poission distribution**
`np.random.geometric(p=.1, size=10)`	| `rgeom(n=10, prob=.1)` 			| **Geometric distribution**; <br>generate 10 draws from the geometric distribution with prob of success 0.1.