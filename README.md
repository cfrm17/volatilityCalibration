# Volatility Calibration

Let Xt be the set of clean returns for a non-proxy risk factor. Certain risk factors exhibit periods of stale data (sequences of 0 return) followed by a non-zero return. This is indicative of a sample distribution with a large peak at 0.  To smooth out the return series, the return following a series of zero returns is spread over the previous returns with a sequence of positive and negative values. 

If a return series has a sequence of 0’s at the end of the return series, these 0’s are replaced with NaN (not a number).   Let Yt be the adjusted (‘processed’) return series of Xt.  If the return series Xt is good (no 0’s), Yt = Xt.  NaN data in Yt is then filtered out when computing the mean and volatility. The mean and volatility for each non-proxy risk factor is computed using the adjusted return series Yt (not Xt) and stored.

If rn is the first return after a series of N stale or missing data points, the missing data is filled-in as follows:

•	First   points replaced with  
•	Remaining points with  

The robust estimate of volatility methodology is described.  The distribution is estimated as a t-distribution with degrees of freedom parameter ν= 4.5.  The value of ν was selected by calculating the estimation error in recovering a known volatility from simulated data from a t-distribution. The estimation error was shown to exhibit a small spread across all input distributions with ν > 2 for values of the estimation parameter 4 ≤ν ≤ 5. 

Consider a stochastic representation of a t-distributed random variable,

 

where Zt are independent standard normal variables that are also independent of St, and St are independent  χ2 ν/( ν-2) random variables. We can determine an iterative scheme for calculating the mean and variance of the distribution. The following set of equations is solved iteratively, converging to σ-hat, μ-hat for sufficiently large k,

  

where ν is the degree of freedom and is set to 4.5

The original estimate does not affect the final result of the iterations, but only the number of iterations (rate of convergence).  The current version uses the sample median and sample standard deviation as the starting point for the mean and volatility respectively.  The iterations are repeated until the following convergence criterion is met:

 

If the estimate does not converge within 10,000 iterations, a warning is issued.  If the variance converges but is less than 1x10-12, then a value of zero is assigned.

The standard form of MLE places the same weight on all data points.  One can use a weighted MLE with weights   to place more or less weight on different parts of the data series.  In particular for time series, an exponential weighting can be applied so that more recent observations receive greater weight.  This makes the volatility estimate more responsive to recent market conditions.

We can determine an iterative scheme for calculating the mean and variance of the distribution. The following set of equations is solved iteratively, converging to σ-hat, μ-hat for sufficiently large k:

 

where   with  

The lambda value is set at 0.969 to provide a 3 month effective window (i.e. the area under a uniform 3 month window is equivalent to the area under an exponential decay curve with lambda equal to 0.969).  The original estimate does not affect the final result of the iterations, but only the number of iterations (rate of convergence). The current version uses the sample median and sample standard deviation as the starting point for the mean and volatility respectively. The iterations are repeated until the following convergence criteria is met:

 .

If the estimate does not converge within 10,000 iterations, a warning is issued.  If the variance converges but is less than 1e-12, then a value of zero is assigned.

The capped volatility is defined as:

 

Where   represents the average volatility based on the robust t estimate with uniform weighting,   represents the exponentially weighted volatility based on the robust t-estimate with weighting to produce a 3 month effective window, cap = 1.25.  NOTE: in the future it may be desirable to have a different cap value for each asset class (or curve).

This produces 3 regimes depending on the calculated value of the average and exponentially weighted volatility:

 


The advantages of using the capped volatility are as follows:

1.	Reacts as fast as exponentially weighted volatility for periods of increasing volatility above average volatility

2.	Has a stable peak – behaves like average volatility when exponentially weighted volatility is much larger than average volatility

3.	Provides buffer above average volatility for periods of higher volatility (buffer is a parameter)


Reference:

https://finpricing.com/product.html
