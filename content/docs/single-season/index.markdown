---
title: Single Season
author: Matt Weldy
date: '2021-02-10'
slug: []
categories: []
tags: []
weight: 1
bibliography: ../../ecological_models.bib
link-citations: yes
---

## Model Description

## Algebra

## Simulation

``` r
y <- 1
```

{{&lt; tabs “uniqueid” &gt;}}
{{&lt; tab “JAGS” &gt;}}

``` r
data <- list(
  y = y
)
modelstring <- textConnection(
  "
      model{
        # Likelihood
      for(i in 1:nSites) {
        # biological model
        z[i] ~ dbern(psi[i])
        # observation model
        y[i] ~ dbin(p * z[i], n[i])
      }
    
      # Priors
      psi ~ dunif(0, 1)
      p ~ dunif(1, 1)
      
      # Derived variable
      N <- sum(z)
      }
    "
)
parameters <- c()

inits <- function() {
  list()
}
ni <- 10000
nt <- 1
nb <- 5000
nc <- 3
#JagsModel <- jags(data, inits, parameters, modelstring, n.chains = nc, n.thin = nt, n.iter = ni, n.burnin = nb)
```

{{&lt; /tab &gt;}}
{{&lt; tab “Stan” &gt;}}

``` r
Stan <- code <- 1
```

{{&lt; /tab &gt;}}
{{&lt; tab “Greta” &gt;}}

``` r
Greta <- code <- 1
```

{{&lt; /tab &gt;}}
{{&lt; /tabs &gt;}}

## Comparison

## References

<div id="refs">

</div>
