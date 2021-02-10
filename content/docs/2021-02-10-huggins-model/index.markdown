---
title: Huggins Model
author: Matt Weldy
date: '2021-02-10'
slug: []
categories: []
tags: []
weight: 1
---



## Model Description

## Algebra

## Simulation


```r
y <- 1
```

{{< tabs "uniqueid" >}}
{{< tab "JAGS" >}} 

```r
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
{{< /tab >}}
{{< tab "Stan" >}}

```r
Stan <- code <- 1
```
{{< /tab >}}
{{< tab "Greta" >}} 

```r
Greta <- code <- 1
```
{{< /tab >}}
{{< /tabs >}}

## Comparison




