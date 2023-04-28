# Introduction

The function implements the panel Moving Blocks Bootstrap (MBB) suggested and
analysed by Gonçalves, S. (2011, *The moving blocks bootstrap for panel linear
regression models with individual fixed effects*, Econometric Theory).

The panel MBB is different from the standard MBB of Kunsch (1989, *The 
jackknife and the Bootstrap for General Stationary Observations*, The Annals of 
Statistics) and Liu and Singh (1992, *Moving Blocks Jackknife and Bootstrap 
Capture Weak Dependence*. In: R. Lepage and L. Billard, Eds., Exploring the 
Limits of Bootstrap, John Wiley, New York), because what is drawn in the 
resampling is the vector containing the *n* individual observations at each 
point in time. The panel MBB is robust to serial correlation (like the standard 
MBB), but also to arbitrary forms of cross sectional dependence.

The simulations carried out in the paper show that the panel MBB performs well
even when the degree of serial and cross sectional correlation is large, 
provided that the block size is appropriately chosen.
In the simulations, Gonçalves adopts a data-driven approach, with a block size
equal on average to:
- 4.4 when T = 25;
- 7.9 when T = 50;
- 12.2 when T = 100.

Please report bugs or comments on github (https://github.com/atecon/pmbb) or
write to giuseppe.vittucci@gmail.com.

# Public function

```
pmbb(const series y, const list X, int blocksize[1::4], int nboot[100::1000],
     int pseed[0::0])
```

The function computes and shows also the symmetric bootstrap percentile-t
confidence intervals of the coefficients at the 1%, 5% and 10% significance
level. The bootstrap percentile-t confidence intervals are calculated as:
beta_hat +/- q_alpha x se_beta
where:

- beta_hat is the FE estimate of the coefficient;
- se_beta is the sqrt of the bootstrap variance estimator based on B bootstrap
replications;
- q_alpha is the alpha x 100 percentile of the absolute value of the 
Studentized bootstrap statistic.

The bootstrap t-statistic is Studentized using the multivariate analogue of the
Götze and Kunsch (1996, Second-Order Correctness of the Blockwise Bootstrap for
Stationary Observations, The Annals of Statistics) variance estimator, adapted
to panel models by Gonçalves (2011).

Please note that the function works only with perfectly balanced panels (no
missing observation).


## Parameters

- `y`:           series, dependent variable
- `X`:           list, regressors (without the constant which is added by 
default)
- `blocksize`:   int, Length of blocksize (default = 4)
- `nboot`:       int, Number of bootstrap replications (default = 1000)
- `pseed`        int, Seed of pseudo-random number generator (default = 0 
(none))

## Returns

The function returns a bundle with:

- `blocksize`: int, Choosen block length
- `coeff`: matrix, Column vector of length `k` referring to the point estimates 
of the coefficients (intercept coefficient not shown)
- `coeffs`: matrix, `nboot` x `k` matrix holding the individual coefficient 
estimates for each bootstrap replication
- `confidence_levels`: matrix, Row column holding the confidence levels
- `N`: int, Number of cross-sectional units
- `T`: int, Length of time-series of the panel dataset
- `nT`: int, Total number of observations
- `nboot`: int, Number of bootstrap iterations set
- `parnames`: strings, Array of regressor names (without intercept)
- `stderr`: matrix, Column vector of length `k` referring to the standard 
errors for each coefficient (not shown for intercept coefficient)
- `tstats`: matrix, `nboot` x `k` matrix holding the Studentized test 
statistics for each bootstrap replication
- `q90`: matrix, Holds the lower and upper bounds (in columns) for each 
regressor (in rows) for the 90 pct. confidence interval of the coefficient 
estimates.
- `q95`: matrix, See description for `q90`.
- `q99`: matrix, See description for `q90`


# Changelog

* **v1.8 (April 2023)**
  * Rewrite help text (using Markdown now) and add references
  * Internal refactoring and optimization

* **v1.7 (March 2018)**
  * re-write of the code leads to speed-up by factor 8
  * pre-allocation of some big matrices
  * re-write of some loop functions leads to more efficient computations incl.
    vectorization
