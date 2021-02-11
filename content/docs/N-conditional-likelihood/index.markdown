---
title: Conditional Likelihood
author: Matt Weldy
date: '2021-02-11'
slug: []
categories: []
tags: []
weight: 1
bibliography: ../../ecological_models.bib
link-citations: yes
---

## Model Description

## Algebra

`$$\hat{N} = \frac{M_{t+1}}{1-\prod^{t}(1-p_t)}$$`

$$y_{i,t} \sim Bernoulli(p_t) $$

## Simulation

``` r
N <- 150 #True population size
n_occ <- 4 #Number of trapping occasions
p <- 0.50 #Probability of first detection

true_detections <- array(NA, dim=c(N,n_occ))
for (t in 1:n_occ){
  true_detections[,t] <- rbinom(n=N,size=1,prob=p)
}
observed <- true_detections[apply(true_detections,1,max) == 1,]
MNKA <- nrow(observed)
print( paste0("Number ever detected: ", MNKA,sep = " ") ) #number ever detected
```

    ## [1] "Number ever detected: 140 "

## JAGS

``` r
library(R2jags)
data <- list(
  y=observed,
  nsites=nrow(observed),
  MNKA=MNKA,
  n_occ=n_occ
)

modelstring <- textConnection(
  "
  model {
  
  # Likelihood
  for(i in 1:nsites) {
    # observation model
    for(j in 1:4) {
      y[i, j] ~ dbern(p)
    }
  }
  for(t in 1:4){
    p_un[t] <- (1-p)
  }
  # Priors
  p ~ dunif(0, 1) # Uninformative prior

  # Derived values
  N <- (MNKA / (1-prod(p_un[])))
}
"
)
parameters <- c("p","N")

inits <- function() {
  list( 
    
  )
}
ni <- 10000 ; nt <- 1 ; nb <- 5000 ; nc <- 3
model <- jags(data, inits, parameters, modelstring, n.chains = nc, n.thin = nt, n.iter = ni, n.burnin = nb)
```

``` r
library(knitr)
```

    ## Warning: package 'knitr' was built under R version 4.0.3

``` r
kable(round(model$BUGSoutput$summary[c(1,3),c(1,2,3,7,8,9)],2))
```

|     |   mean |   sd |   2.5% |  97.5% | Rhat | n.eff |
|:----|-------:|-----:|-------:|-------:|-----:|------:|
| N   | 147.98 | 1.47 | 145.45 | 151.21 |    1 | 15000 |
| p   |   0.52 | 0.02 |   0.48 |   0.56 |    1 | 15000 |

## Stan

``` r
library(rstan)
data <- list(
  y=observed,
  nsites=nrow(observed),
  MNKA=MNKA,
  n_occ=n_occ
)
stan_model <- "
data {
  int<lower=0> MNKA;
  int<lower=0> nsites;
  int<lower=0> n_occ;
  int<lower=0,upper=1> y[MNKA, n_occ];
}

parameters {
  real<lower=0, upper=1> p;
}

model {  
 for(i in 1:nsites)
    for(j in 1:4) 
      y[i, j] ~ bernoulli_lpmf(p);
}

generated quantities {
  real N = MNKA / (1-(1-p)^4);
}
"
nc <- 4

stan.samples <- stan(model_code = stan_model, data = data, iter = 10000, chains = nc, cores = nc)
```

``` r
library(knitr)
kable(round(summary(stan.samples)$summary[1:2,c(1,2,3,4,8,10,9)],2))
```

|     |   mean | se\_mean |   sd |   2.5% |  97.5% | Rhat |  n\_eff |
|:----|-------:|---------:|-----:|-------:|-------:|-----:|--------:|
| p   |   0.52 |     0.00 | 0.02 |   0.48 |   0.56 |    1 | 6823.51 |
| N   | 148.02 |     0.02 | 1.49 | 145.41 | 151.22 |    1 | 6866.30 |

## Comparison

## References

<div id="refs">

</div>
