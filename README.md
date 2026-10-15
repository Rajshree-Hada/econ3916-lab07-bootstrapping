# econ3916-lab07-bootstrapping

# Estimation & Bootstrapping — Quantifying Uncertainty Without a Formula

## Objective
I measured how uncertain sample estimates are on the UCI Adult Census Income data, using both formula-based (CLT) and bootstrap confidence intervals, and tested where each one works.

## Methodology
- Loaded the UCI Adult Census Income data (32,561 respondents).
- Separated the standard deviation of the data from the standard error of an estimate, and checked the s/sqrt(n) relationship across four sample sizes.
- Built parametric confidence intervals from the CLT for age and for capital gains.
- Ran the bootstrap by hand, then with `scipy.stats.bootstrap`.
- Compared percentile and BCa intervals on a heavily skewed variable.
- Bootstrapped a two-sample difference in mean weekly hours.
- Asked an AI to write a bootstrap-interval function, revised my prompt once, and checked the function against my own intervals.
- Ran a coverage simulation on lognormal data at n = 50, for both the mean and the median.

## Key Findings
- The standard deviation describes the spread of individuals and does not change with sample size. The standard error shrinks as n grows, following s/sqrt(n).
- The CLT converges quickly for age and slowly for capital gains, so a parametric interval is trustworthy at a much smaller sample size for age than for capital gains.
- The bootstrap standard error of the median of the 32,561 whole-number ages is exactly 0.0000. With that many ties, every resample returns the same median. This is a real property of the data, not a failure of the bootstrap, and it does not mean the population median is known exactly.
- In the two-sample comparison of mean weekly hours, the interval excluded zero. That shows the gap is unlikely to be sampling noise. It does not show why the gap exists.
- The AI-written function gave different intervals on repeated calls until I added a seed. With a seed it reproduced my own intervals, so the differences I saw earlier were simulation noise and not a different method.
- Coverage simulation (lognormal data, n = 50, nominal level 95%, true mean 37.52, true median 33.12):
  - **Mean:** the parametric interval covered 92.5% and the percentile bootstrap covered 91.5%. The 1.0-point gap is smaller than the Monte Carlo standard errors (1.3 and 1.4 points), so I can't tell the two methods apart here. Both under-cover the nominal 95%.
  - **Median:** the mean-centred parametric interval covered only 66.8% (Monte Carlo SE 2.4), while the bootstrap covered 93.2% (Monte Carlo SE 1.3), close to its nominal level.
