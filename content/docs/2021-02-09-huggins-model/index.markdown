---
title: Huggins Model
author: Matt Weldy
date: '2021-02-09'
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

## JAGS Model Fit


```r
data <- list(
  y = y
)
modelstring <- textConnection(
  "
	  model{
		for (i in 1:n) {
		  x[i]~dnorm(mu,tau)
		}
		mu~dnorm(cc,d)
		tau~dgamma(a,b)
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
## Stan Model Fit




## Comparison




# We could also do it like this 

{{< tabs "uniqueid" >}}
{{< tab "JAGS" >}} 

```r
data <- list(
  y = y
)
modelstring <- textConnection(
  "
	  model{
		for (i in 1:n) {
		  x[i]~dnorm(mu,tau)
		}
		mu~dnorm(cc,d)
		tau~dgamma(a,b)
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
